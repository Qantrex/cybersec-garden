---
{"dg-publish":true,"permalink":"/itsi/tools/ffuf/","title":"ffuf","tags":["tool","ffuf","fuzzing","discovery","recon"]}
---

# ffuf
[Official Link](https://github.com/ffuf/ffuf)
## Examples
```bash
ffuf -u 'https://f030ceb1154faf1d02ba68c5e54756af.ctf.hacker101.com/index.php?page=view.php&id=FUZZ' -w <(seq 0 1000) -H 'Cookie: id=eccbc87e4b5ce2fe28308fd9f2a7baf3' -o fuzz-output.csv -of csv -fr "Post not found" -t 10 -p 0.2 -timeout 10 -v
```

```bash
ffuf -u https://example.com/FUZZ \
     -w /usr/share/seclists/Discovery/Web-Content/common.txt \
     -H "Cookie: XXX" -e .php -o ue3-fuzz-assets.csv -of csv  
```
## Flags
- `-u` for the URL (mark the input as `FUZZ`)
- `-w` for the wordlist
	- for numbers use <(seq 0 100)
- `-H` for Header's like Cookies or Auth
- `-e` for any other filetypes than HTML
- `-o` Output file
- `-of` formats like json or csv
- `-p` fixed delay between requests in seconds
- `-v` verbose (shows errors)
- `-t` amount of threads (lower=slower)
- `-timeout` time in seconds to wait per HTTP-Request before skipping