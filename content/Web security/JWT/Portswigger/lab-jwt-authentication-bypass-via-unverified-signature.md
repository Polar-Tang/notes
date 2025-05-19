https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-unverified-signature
For this lab i installed the bap extension from the bapp store called JWT decode


Intercept the request, in the JSON Web Token tab, change to administrator and in the atactk select use the attack called "Sign with empty key"

![[Pasted image 20250425154122.png]]As the backend is not verifying the key, we could sign with an empty key and the server will take it as a valid