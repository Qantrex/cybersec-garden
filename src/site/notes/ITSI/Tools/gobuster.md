---
{"dg-publish":true,"permalink":"/itsi/tools/gobuster/","title":"Gobuster","tags":["tool","gobuster","directory-enumeration","discovery","recon"]}
---

# Gobuster
[Official Link](https://github.com/OJ/gobuster)
## Examples
Travers directories with wordlist and header:
```bash
gobuster dir -u https://exercises.itsi.rocks/ue3/ -w Tools/wordlists/SecLists/Discovery/Web-Content/common.txt -H "Authorization: Basic aXRzaTpzZWN1cml0eWJ5b2JzY3VyaXR5"
```