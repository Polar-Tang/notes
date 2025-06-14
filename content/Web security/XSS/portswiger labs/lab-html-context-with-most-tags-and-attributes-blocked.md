https://portswigger.net/web-security/cross-site-scripting/contexts/lab-html-context-with-most-tags-and-attributes-blocked

This lab has a waf blocking all the attempting of html tags. Let's bruteforce for all the portswigger list
```sh
ffuf -u "https://0a1a007203eb3c4281f07a1400c400e6.web-security-academy.net/?searc=FUZZ" -w /usr/share/seclists/Fuzzing/XSS/robot-friendly/XSS-Cheat-Sheet-PortSwigger.txt -ac -enc "FUZZ:urlencode"
```
The only one to be accepted by the waf, besides some "custom tags", is body, with the attribute of resize.
Judging from what we can see fro burpsuite seems that we didn't much anywhitn, but the existing body tag is being rewriting:
![[Pasted image 20250613144218.png]]
However the print is never triggered
This is because the body has already it's current size and does required user interaction to be triggered