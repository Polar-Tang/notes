https://portswigger.net/web-security/cross-site-scripting/dom-based/lab-document-write-sink-inside-select-element

The product details has a select with some option, which are dynamically writted in the html with document write.
```js
var stores = ["London","Paris","Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');
document.write('<select name="storeId">');
if(store) {
    document.write('<option selected>'+store+'</option>');
}
for(var i=0;i<stores.length;i++) {
	if(stores[i] === store) {
	    continue;
	}
	document.write('<option>'+stores[i]+'</option>');
}
document.write('</select>');
```
 These stores options can also be supplied by the user in the params of the url
![[Pasted image 20250606003949.png]]
This weird feature could easily be avoided by using a white list:
```js
var stores = ["London","Paris","Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');
if (!stores.includes(store)) {
	document.write('<h1>NOPE</h1>');
}
```