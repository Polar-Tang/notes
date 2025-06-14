https://portswigger.net/web-security/cross-site-scripting/contexts/lab-javascript-url-some-characters-blocked
### Introduction
In this lab our input is reflected in a JavaScript variable, the backslashes are being parsed, but due to the browser html parsing behavior is still explotaible though.
However something interesting happening in this lab is that the application does url decode the name input
`testing12%33` becomes -> `testing123`
But after decoding is also escaping the chars. However after all tries we could assume that XSS is unexplotable there. 

#### Alternatives
However if we have read the lab description we'll know that the vulnerabillity is in the URL.
In the wild we could deduce that by assuming that the post id is another input controlled by the user, the post id has some delimiters
![[Pasted image 20250613110634.png]]
https://portswigger.net/research/xss-without-parentheses-and-semi-colons