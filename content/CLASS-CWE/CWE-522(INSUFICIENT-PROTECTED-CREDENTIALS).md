https://cwe.mitre.org/data/definitions/522.html
The product transmits or stores authentication credentials, but it uses an insecure method that is susceptible to unauthorized interception and/or retrieval.

##### Example
This code changes the user password:
```php
$user = $_GET['user'];  
$pass = $_GET['pass'];  
$checkpass = $_GET['checkpass'];  
if ($pass == $checkpass) {

SetUserPassword($user, $pass);

}
```
It's just checking that the user is introduced password is the same, but isn't confirming that the user changing the password is the same who owns the account.

### Children CWEs
| Nature   | Type                                                   | ID                                                       | Name                                              |
| -------- | ------------------------------------------------------ | -------------------------------------------------------- | ------------------------------------------------- |
| ChildOf  | ![Class](https://cwe.mitre.org/images/icons/class.gif) | [668](https://cwe.mitre.org/data/definitions/668.html)   | Exposure of Resource to Wrong Sphere              |
| ChildOf  | ![Class](https://cwe.mitre.org/images/icons/class.gif) | [1390](https://cwe.mitre.org/data/definitions/1390.html) | Weak Authentication                               |
| ParentOf | ![Base](https://cwe.mitre.org/images/icons/base.gif)   | [256](https://cwe.mitre.org/data/definitions/256.html)   | [[CWE-256(plain-text-storage-of-password)]]       |
| ParentOf | ![Base](https://cwe.mitre.org/images/icons/base.gif)   | [257](https://cwe.mitre.org/data/definitions/257.html)   | [[CWE-257(Storing-password-in-recoveral-format)]] |
| ParentOf | ![Base](https://cwe.mitre.org/images/icons/base.gif)   | [260](https://cwe.mitre.org/data/definitions/260.html)   | [[CWE-260(PASSWORD-IN-CONFIGURATION-FILE)]]       |
| ParentOf | ![Base](https://cwe.mitre.org/images/icons/base.gif)   | [261](https://cwe.mitre.org/data/definitions/261.html)   | [[CWE-261(encoding-for-password)]]                |
| ParentOf | ![Base](https://cwe.mitre.org/images/icons/base.gif)   | [523](https://cwe.mitre.org/data/definitions/523.html)   | [[CWE-523(unproper-transport-creds)]]             |
| ParentOf | ![Base](https://cwe.mitre.org/images/icons/base.gif)   | [549](https://cwe.mitre.org/data/definitions/549.html)   | [[CWE-549(Missing-Password-Field-Masking)]]       |
