https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-obfuscated-file-extension

In this lab the web applciation attempts to only acept file extensions such as JPG or pnr. The application seems to match por the last extension using in the filename string, but whne this functions is passed to a language of low level the null byte gets parse, we could confirm that the backend is kinda droping everything after the nullbyte
![[Pasted image 20250518232825.png]]
If we combine the nullbyte with a crlf we got the lab solution `file.php%00%0a.png`