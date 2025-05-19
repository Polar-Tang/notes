https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-broken-logic
 -----
**Difficulty**: EASY
**CWE-lists**:
- [[CWE-799(improper-control-interaction-frequency)]]
-----
- Do the restoring account process with the given credentials, 
- look for the get login 2 and change the verify parameter to carlos, 
![[Pasted image 20241105234821.png]]
- Now let's FUZZ the mfa-code, 
- I right clicked the picture to copy to a file
![[Pasted image 20241105230709.png]]
So i culd use this ffuf:
```sh
ffuf -u https://0a660037044547959cb70ba40057002b.web-security-academy.net/login2 -request REQUEST.txt -w /usr/share/seclists/Fuzzing/4-digits-0000-9999.txt -replay-proxy http://127.0.0.1:8080 
```
- Then filter in the HTTP history
![[Pasted image 20241105235806.png]]
And when it apper, right click `show response in browser` and paste that link on the browser