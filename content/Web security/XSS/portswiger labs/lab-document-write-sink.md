https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink

For the moment we got a document write which is prented as html pretty raw.
![[Pasted image 20250603214330.png]]
The user input is user with document.write in an image tag
![[Pasted image 20250603223539.png]]
Basically we got a img tag, that look like this:
```html
<img src="/resources/images/tracker.gif?searchTerms=HEREGOESTHEINPUT">
```
We only need to use `">` and then the html will broke
```html
<img src="/resources/images/tracker.gif?searchTerms="><h1>hi</h1>">
```
![[Pasted image 20250603224724.png]]