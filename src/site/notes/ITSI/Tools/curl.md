---
{"dg-publish":true,"permalink":"/itsi/tools/curl/","title":"curl","tags":["tool","curl","http","requests","recon"]}
---

# Curl
[Official Link](https://curl.se/docs/manpage.html)

Curl mit POST 
```zsh
curl -i -X POST https://exercises.itsi.rocks/ue3/api/ \
  -H "Cookie: PHPSESSID=0265d5fb754d9b29a041f7f424972b8e" \
  --data "godmode=on&name=GPenTester"
```

Basic Curl
```zsh
curl -H "Authorization: Basic aXRzaTpzZWN1cml0eWJ5b2JzY3VyaXR5" -Ss "https://exercises.itsi.rocks/ue3/index.php"
```