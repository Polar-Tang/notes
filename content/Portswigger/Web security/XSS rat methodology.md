- Start out creating an account. Include the following
	- Wherever possible insert both, SSTI and XSS attack vectors, for example:
	```html
<img src=x onerror=alert()>${{7*7}} 
	```
	- Insert that attack vector into every possible field
	- Create your own attack vector!
- After register explore the website like a normal user would do
- **Make a mindmap** of the functionality. You could use tools like [xmind](https://xmind.app/)
- Keep BurpSuite or any other MitM open in the background
When i approach a new target the very first thing i do is to know my target. I need to know all the **available functionality** and i do that by **register an account and using the application like a normal user would do**. While do this i'm paying special attention to **the privilege levels** as well as the **functionality that was added** later. Every new function represents an integration point, the function by itself may be secure but another integrated function may introduce a vulnerability, (I.E. [[CWE-305(auth-bypass-by-primary-weakness)]]).
While i do this i keep **BurpSuite open** while use my scope set properly **to populate the site map**. 
- Read the documentation available, or swagger docs if its available
- Prove XSS, SSTI attack vector as soon  as i crate an account
All of this manual testing takes me easily a couple of days. 

##### PoC || GTFO
- Prove autamated scans results manually
- you need to prove impact or Get The F** out
#### Exploring request in Burpsuite
Once i know how the target functionallity is supposed to work, i go to burpsuite
- Select (show only parametrized requests)
- Navigate to these request to see if there is any request with parameters i can temper with

#### **What parameters are do i test for what vulnerabilities**?
- We are interested in parameters that can trigger some bussinbes logic 
- Any parameters that seems to contain a URL. If the target resolves the URL that i enter, it might be vulnerable to SSRF
- Any parameter grabbing images from the server, from the HDD of the webserver and being refered to as `image=file.png`. This might be vulnerable to LFI/RFI
- Any CSRF parameter
- Anywhere i can upload an image i will try an XXE via svg
- Anywhere that can upload DOCX or XLSX file i will try an XXE

##### Automated testing
I will pick a target when i can create different users of different privilege levels. THis way i will can
- Create two accounts and test for IDOR
- Create to different users with **the same** privileges levels and test for IDOR
- Create two users of **different** privileged levels and test for Broken Access Control
Create all the different levels and if you can create your own privilege level make sure to do so
##### Identifying endpoints to attack
- If there is an endpoint like `/api/v2/getInvoices` we should try `/api/v1/getInvoices`
- Use tool like [linkfinder](https://github.com/GerbenJavado/LinkFinder) to find http endpoints
- Use [Google dorking]()
- Use [Github Dorking](https://shahjerry33.medium.com/github-recon-its-really-deep-6553d6dfbb1f)

#### General tips
- Spend more time understanding the applcation, using it, but with burp open
- Start with XSS and SSTI as early as possible if in scope, inster them into every field you use
- Create your own fuzzing list
- Expand this list with your own findings
- -VDP over paid if you want less competition
- PoC or GTFO
- Prove your impact