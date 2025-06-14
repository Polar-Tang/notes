https://portswigger.net/web-security/cross-site-scripting/exploiting

For solving this lab i deploy my [own burpcollaborator](https://github.com/Polar-Tang/oast) service onrender.com.
In the dashbord of render i can see the console log from the app
![[Pasted image 20250613174422.png]]
Later i confirm that the XSS exist and is rendering whatever tag.
Then i create the fetching function
![[Pasted image 20250613173033.png]]