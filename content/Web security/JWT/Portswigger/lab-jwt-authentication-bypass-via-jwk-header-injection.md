https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-jwk-header-injection

In the JWT Editor go to create a new RSA
![[Pasted image 20250425183628.png]]
Just click generate.
Now in the repeater tab, select the attack "embed JWK" and select the new generated and send the response.
We successfully embed a jwk that the server which the server is no validating.
