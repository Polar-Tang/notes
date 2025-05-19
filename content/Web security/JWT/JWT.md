https://portswigger.net/web-security/jwt
#### What are
JSON web tokens are standarized format for sending cryptographically signed JSON data betweeen systems.

- Consist of a **header** which says the encription type. 
- **Payload** with the encrypted data
- **Signature** header and payload in Base64 hashed with a secret
see https://jwt.io/

### Signature
The server did not anything about the user and all the information is self-contained on the JWT. This ensure some advantages, but if the server is not properly validating the signature it could be manipulated by an attacker to take over an account. In the following list you will find a list of lab that are a good example of a weak secret signature:
- [[lab-jwt-authentication-bypass-via-unverified-signature]]
- [[lab-jwt-authentication-bypass-via-flawed-signature-verification]]
- [[lab-jwt-authentication-bypass-via-weak-signing-key]]

### Header
The JOSE headers (**JavaScript Object Signing and Encryption**) essentially is a javascript object which tells how to securely sign and encrypt the data. The values we are interested in are the followings:
- `alg`:  It's a mandatory parameter that specifies the algorithm encryption
- `jwk` (JSON Web Key) - Provides an embedded JSON object representing the key.
- `jku` (JSON Web Key Set URL) - Provides a URL from which servers can fetch a set of keys containing the correct key.
- `kid` (Key ID) - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choose from. Depending on the format of the key, this may have a matching `kid` parameter.
### Payload
#### JWE vs JWS
Until now we have seen the JWE lab-jwt-authentication-bypass-via-weak-signing-key. There are 2 JWT specifications in total:
- JWS is the normal base64 token with the signature as the last part.
- JWE is when the payload is encrypted
#### jwk
The JSON Web Signature **(JWS)** specification describes an optional `jwk` header parameter. JWK is a JSON object for representing key (ussually public key) as a JSON object.
###### Example
```json
{ 
	"kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG",
	"typ": "JWT", 
	"alg": "RS256", 
	"jwk": { 
		"kty": "RSA", 
		"e": "AQAB", 
		"kid": "ed2Nf8sb-sD6ng0-scs5390g-fFD8sfxG", 
		"n": "yy1wpYmffgXBxhAUJzHHocCuJolwDqql75ZWuCQ_cb33K2vh9m" 
	} 
}
```
A server should use a limited whitelist of JWT signatures. But a misconfigured server may use  use any key embed in the jwk.