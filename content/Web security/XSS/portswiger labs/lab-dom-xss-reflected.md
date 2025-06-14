https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-dom-xss-reflected
XMLHttpRequest is similar to fetch method, but the main difference is to request some resource from the internet without reloading the page. 
When the request success, utilizes an eval function to redefine variable called `searchResultObj` with the response from the server parsed to string
```js
var xhr = new XMLHttpRequest();
    xhr.onreadystatechange = function() {
        if (this.readyState == 4 && this.status == 200) {
            eval('var searchResultsObj = ' + this.responseText);
            displaySearchResults(searchResultsObj);
        }
    };
    xhr.open("GET", path + window.location.search);
    xhr.send();

```
![[Pasted image 20250607132925.png]]
The response from the backend is an object with an array results and search term,
 `"url/?search="alert(1)` becomes `"{"results":[],"searchTerm":"\"alert(1)"}"`, then this response is utilized in the eval expression: 
 `eval('var searchResultsObj = ' + "{"results":[],"searchTerm":"\"alert(1)"}");`

### Solution
The backend utilizes double quotes but it **Escapes the quote** (`"` → `\"`), this **prevent breaking out** of the string
Later, `searchResultObj.searchTerm` is utilized as innerText so the input is parsed as string and its sanitized
```js
function displaySearchResults(searchResultsObj) {
        var blogHeader = document.getElementsByClassName("blog-header")[0];
        var blogList = document.getElementsByClassName("blog-list")[0];
        var searchTerm = searchResultsObj.searchTerm
        var searchResults = searchResultsObj.results

        var h1 = document.createElement("h1");
        h1.innerText = searchResults.length + " search results for '" + searchTerm + "'";
        blogHeader.appendChild(h1);
        var hr = document.createElement("hr");
        blogHeader.appendChild(hr)
        //...
    }
```
![[Pasted image 20250607132147.png]]
>Innertext should be only vulnerable to xss if it's writting the user input in dangerous tags https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html#usually-safe-methods