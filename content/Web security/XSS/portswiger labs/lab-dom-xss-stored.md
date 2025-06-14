https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-stored
We send the post from the frontend 
```js
 xhr.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
            let comments = JSON.parse(this.responseText);
            displayComments(comments);
        }
    };
    xhr.open("GET", postCommentPath + window.location.search);
    xhr.send();

```
As we can see the response is parsed with JSON parse, i submit html encoding payloads:
```js
<script><script>
```
And i see that the response from the backend is not encoding them:
```
1. author: "<script><script>"
2. avatar: ""
3. body: "<script><script>"
4. website: "https://test.com" 
```
The backend only escpae duoble quote and backslashed,and does not normalize anything.

Then the frontend process the fields from the object sent by the backend, changing the less than and bigger than values to their html encoded versions:

```js
function escapeHTML(html) {
    return html.replace('<', '&lt;').replace('>', '&gt;');
}
```
So here `<script><script>` turns into `&lt;script&gt;<script>`

And use innerHTML, there's the sink

### Solution
If we take a closer look to the escapeHTML function, it utilize replace, which only `replace` the first occurrence (if the arg is a string), so the payload should includes multiple less and more than symbols
![[Pasted image 20250608231154.png]]
