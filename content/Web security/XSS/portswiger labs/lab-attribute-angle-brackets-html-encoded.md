https://portswigger.net/web-security/cross-site-scripting/contexts/lab-attribute-angle-brackets-html-encoded
Our input gets reflected in the HTML, however the app doe html encoding angle brackets. The app also escape backslashes and html encode the qingle quotes, however is ingoring that out input also get reflected in the value atribute, which is a value very used in the inputs.

### Solution
We need to escape the value query and we need an atribute that does not require a value, such as `autofocus=""`, the following payload works for me
```url
https://0a8f00790397911d80bd03cf00090025.web-security-academy.net/?search=%22%20onfocusin=alert(1)%20autofocus=%22
```
and will result in:
![[Pasted image 20250611010201.png]]