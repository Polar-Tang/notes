HTTP protocol specifies that browser should send an User Agent header, so the server can infer the broswer capabillities of the client

The XSS vector contains one or several html tags or attributes
The payload is a piece of JS code
THe payload format is a special way to decode the format


The browsers first parses the html and then the JS, this make the scenarios xplotable in many cases
https://portswigger.net/web-security/cross-site-scripting/contexts#xss-between-html-tags#xss-into-javascript