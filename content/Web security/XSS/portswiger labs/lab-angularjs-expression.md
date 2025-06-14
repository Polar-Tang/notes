https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-angularjs-expression
Open dom invader -> inject form and click search
![[Pasted image 20250607113610.png]]
Alternatively if we have search for the name of the url params, the word "search", we have find the same function in the Angular bundle
![[Pasted image 20250606223450.png]]

The application is filtering most of the special characters though, in spite of that ng-app is enabling the javascript execution in the client side
![[Pasted image 20250607115327.png]]
Whenever we submit double curly braces `{{}}` is interpreted and executed as javascript code

### Solution
The first payload of [payloadallthethings Angular wordlist](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/5%20-%20XSS%20in%20Angular.md ) will work