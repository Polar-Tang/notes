https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-broken-logic
------
**Difficulty**: EASY
**CWE-lists**:
- [[CWE-282(Improper-Ownership-Management)]]
- [[CWE-287(improper-authentication)]]
- [[CWE-294(bypass-by-capture-replay)]]
- [[CWE-640(Weak-recovery-password-mechanism)]]
-------
- To reset a password we need to provide the user or email account. 
- The server look for the email account to redirect the user an endpoint 
- The endpoint has a signed token which is utilized once, but it's not verifying the ownership by this token
- Actually a body parameter it's utilized to reset the pass, which could be modified by an attacker  