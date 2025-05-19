--------
**Difficulty**: EASY
**CWE-lists**:
- [[CWE-282(Improper-Ownership-Management)]]
- [[CWE-640(Weak-recovery-password-mechanism)]]
-------
The login process seems good at first glance, but the post has a login body, 
- there's no cookie, 
- not encription, 
- just a login `username2=jessamy&mfa=346707`
it seems like the backend is selecting the acount by the username, which can by easily bypassed

```PHP
<?php
require '../db.php';

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    if (isset($_POST["mfa"]) && isset($_POST["username2"])) {
        // check if the mfa code is correct
        $mfa = $_POST["mfa"];
        $username = $_POST["username2"]; // doesn't validate username
        $sql = "SELECT mfa FROM auth0x02"; // 👀
        $result = $conn->query($sql);
        $mfa_check = 0;
        if ($result->num_rows > 0) {
            while ($row = $result->fetch_assoc()) {
                if ($row['mfa'] == $mfa) {
                    $message = "You have sucessfully logged in!";
                    $status = 1;
                    $complete = 1;
                }
            }
        } 
?>

```
