It's a vulnerabillity where the application may execute arbitrary javascript code controlled by an attacker. So the attacker might be able to
- Bypass Same control policy
- impersonate the victim
- if the victim is admin, the attacker might be able to gain admin privileges

THe XSS does ocur in several scenearios

### XSS context
The context basically is the location of the response that the user do control.
Also any other data validation being processed for the input
### Reflect XSS
The response includes data controlled by the request in a unsafe way
**For example**, a url param being rendered in raw html:

##### [Impact of reflected XSS](https://portswigger.net/web-security/cross-site-scripting/reflected#impact-of-reflected-xss-attacks)
If the attacker can control scripts used in the victim browser, he can fully compose him

The location on the payload being reflected will determine the payload, also if the application perfom any validation in the data being reflected this will also affact drastically the payload.
Is necessary to check all the contexts where the input is reflected, (e.g. [[lab-attribute-angle-brackets-html-encoded]])
##### Test **reflected XSS** mannualy
- Test separetly every entry point within the application's http request, this includes every parameter or any other data included in the url, as well as the request body
- Test an aphlanumeric value of 8 letters and see if it's being reflected by replay replaying the request from a proxy tool
- **Determine the reflection context**
- Test candidate payloads based in the context
- If the payload is being modified or blocked by the application you will need to try different alternatives
- Finnally, if you have find out a payload that seems to work. You can try it in the browser, most of the proxy tools has an option for seeing the response in the browser

### **Stored XSS** 
**Stored XSS** is similar to **reflected XSS** but the executable javscript is self-contained in tha application, avoiding user interaction, which increase the impact of the vulnerabillity.
For example: [[lab-html-context-nothing-encoded]]

##### [Test **stored XSS** mannualy](https://portswigger.net/web-security/cross-site-scripting/stored#how-to-find-and-test-for-stored-xss-vulnerabilities)
Testing for stored XSS required all the "entry point" of user controllable dataand understand how they links to the "exit points"
Entry point of the application might be:
- Test parameters or other data within the url
- The url file path
- Test the body message
- Any out-of-band route which can deliver data into the application, e.g a blog aggregator that includes any's blog
Is necessary to indentify exit point, and monitored the responses
### DOM-based  XSS
In the dom-based type of XSS the user input is being executed or rendered by the application. 
The function that do render it it's called sink. The source it's where malicious input enters, which is most commonly the URL.
###### Sink
Exists a function, property or API that could execute or render user controlled input in a dangerous way, such functions are called **sinks**.

**These are the functions used be DOM invader:**
https://portswigger.net/web-security/cross-site-scripting/dom-based#which-sinks-can-lead-to-dom-xss-vulnerabilities
Here are some labs enumerated by its sink functionallity

| **Example**                                       | **Risk**                                                                         |
| ------------------------------------------------- | -------------------------------------------------------------------------------- |
| `element.innerHTML = userInput`                   | Renders HTML/JS from input, [[lab-innerhtml-sink]],                              |
| `location.href = userInput`                       | Can execute JS via `javascript:` URLs.                                           |
| `element.setAttribute("href", input)`, `$().attr` | Dangerous if input starts with `javascript:`. [[lab-jquery-href-attribute-sink]] |
| `document.write()`                                | [[lab-document-write-sink]], [[lab-document-write-sink-inside-select-element]]   |
| `document.writeln()`                              |                                                                                  |
| `document.domain`                                 |                                                                                  |
| `element.outerHTML`                               |                                                                                  |
| `element.insertAdjacentHTML`                      |                                                                                  |
| `element.onevent`                                 |                                                                                  |
| `$(window).on('hashchange',() => ())`             | [[lab-jquery-selector-hash-change-event]]                                        |
| Angularjs, `<body ng-app=""/>`                    | [[lab-angularjs-expression]]                                                     |
For testing for a sink use a random string and see where it appears by using dev tools. You must see where is the input reflected and understand its context (e.g. the input is between double quotes, then use double quotes)

test for DOM-based, often the input is not reflected, in such cases you could use the debugger and search in these folders to see when a source is refered, when you find it you could use brackpoint to keep the track of the source. Sometimes is assigned to a variable, in such cases we should look for the variable to see if is passed to a sink

### HTML Parser quirks
- The application do rea

Here are some labs enumerated by its context

| **Example**              | **Risk**                                                                                        |                 |
| ------------------------ | ----------------------------------------------------------------------------------------------- | --------------- |
| Attributes being decoded | [[lab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped]] | Html, attbibute |

