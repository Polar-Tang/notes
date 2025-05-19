https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-flawed-signature-verification

If the encryption algorithm is none, the signature JWT part can be totally omitted.
- intercept the traffic
- Remove the third JWT part 
- Use jwt.io or JWT editor to decode to base64 the following payload
	```json
	{  
    "iss": "portswigger",  
    "exp": 1745610846,  
    "sub": "administrator"  
    }	
	```
- Send a GET request to `/admin/delete?username=carlos` with the admin token