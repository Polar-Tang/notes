https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-path-traversal

Our PHP files get uploaded, however the code is commited.
![[Pasted image 20250518183404.png]]
The explanation for this is that the files in the avatar directory are automatically commited

If the filename field is not restricted we could uploaded files in arbirtary location of the filesystem . `..%2ffile.php`
![[Pasted image 20250518185142.png]]