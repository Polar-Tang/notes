Imagine all the ways we can create impact on theese vulnerabillities

#### Manually explore your application
Test at least 8 hours
Just explore the application with a proxy intercepting at background
##### Set proxy
- show only parameterized requests

#### Parameters analysis
- Explore the application: Business Logic Flaw
- URL parameters that get resolved: SSRF
- Parameters grabbing files either localy or remotly: LFI/RFI
- CSRF parameters: CSRF
- Uploading an image: XXE via SVG
- Uploading documents: XXE via DOCX/XLSX
- Looking for hidden fields, both in the request and response 
### Broken access control
Look at the privilege levels.
What a user is allowed to do.

- **some terms**
	- Account = organization 
	- User = empleyee from an organization
- **preparation**
	- Create 2 accounts (from different organizations)
	- Invite at least 2 users per account
- **Test for IDOR**
	- IDOR between two accounts
	- Betweeen 2 users within 1 account

#### Test for
- **Check for documentation**
- Test between all users from all privileged leves
- Check if the functionalities for a normal user are allowed as a low privileged user
- So try to do the admin invoices with the user invoices, and so on
- ![[Pasted image 20250426012417.png]]
### Injection
- If it tells you about high privileged functionallities, try it
	- While do use an XSS atack vector every possible field
	- `<img src=x> '"${{7*7}}`
##### SQLI
- **Test** on every database read or write
### Business Logic Flaw Vulnerabilities 
- Whatever the manual tell you to not do, do it
- Mess every single parameter and analyze results
	- I.E. if the developer wants us to rate from 1 to 5
		- Try 0
		- Try 6
		- Try -1
		- Try a
		- Try ...
- Mess the order of the requests 

### Server Side Request Forgery
- SSRF give us access to internal server that we are should not be able to access
- **Test on**
	- Any URL that gets resolverd by the server and we controll
	- Partial URLs in the body instead of a URL
	- URLS within data files such as XML files or CVS files
	- The referer header can sometimes contain SSRF defects
- **Test for** 
	- **SSRF** against the server itself
	- **SSRF** against the bakend system (I.E another ports in the backend)
	- Blind SSRF (Requires extensive knoledge of the network and bruteforcing)

### OS command injection
- input get sometimes executed in a sh script
- **Test**
	- Use a worlist with command separtors
	- Encode the list
	- Test every single parameter

### CSRF
- **Test using**
	- match and replace
		- Add match and replace rule
			- Type: Request body
			- Match: "`CSRF=*`" (replace this value with how your target does CSRF tokens)
			- Replace: "`CSRF=*`" << SEE NEXT SLIDE
			- Regex match true
	- Autorepeter plugin in burp
	- CSRF scanner burp
- **Test techniques**
	- Remove CSRF from the request
	- Replave the CSRF value with a random string
	- Replace CSRF with a random token of the same restrain (I.E replace 12345 for 123456 )
	- Remove the CSRF value
	- Use CSRF token that has been used before
		- See if you can request request a CSRF by executing the call manually and use that token in the request

### Find more endpoints
- JS analysis 
- Wayback URL
- Google dorking
- Api Documentation