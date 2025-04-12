---
tags: 
aliases:
---
https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass
-----
**Difficulty**: EASY
**CWE-lists**:
- [[CWE-294(bypass-by-capture-replay)]]
- [[CWE-282(Improper-Ownership-Management)]]
-------

Login answer with a cookie
![[Pasted image 20241106005331.png]]
login2 use that cookie to log the page for seting the code
![[Pasted image 20241106005414.png]]
then we send a code, which is a post request, with that same cookie as a header
![[Pasted image 20241106005654.png]]
But inmediatly after we send that request the server deletes and sends back a new cookie.
#### Solution
if we do a little bit of [[A-B testing]] we will send the wiener's OTP from the Carlos account, the cookie that the server will generate will be for the carlos's account. That's because we send a correct OTP with the carlos's cookie, but the backend isn't linking the OTP to an account.


