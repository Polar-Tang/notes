https://cwe.mitre.org/data/definitions/315.html

The product stores sensitive information in cleartext in a cookie.
### Example
This code is writting a cookie with the user credentials in plain text:
```php
function persistLogin($username, $password){

$data = array("username" => $username, "password"=> $password);  
setcookie ("userdata", $data);

}
```
