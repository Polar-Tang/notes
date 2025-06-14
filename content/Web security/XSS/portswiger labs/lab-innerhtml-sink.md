https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-innerhtml-sink
Inner html method is used to trender the input as html, it does not run the script tag, but there are more tags for running javascript, for example 
![[Pasted image 20250606000600.png]]

much more of them can be found in `/usr/share/seclists/Fuzzing/XSS/robot-friendly/XSS-Cheat-Sheet-PortSwigger.txt`, assuming you have seclist wordlist there