The implemented authentication schemes it's vulnerable to spoofing attacks

### IP authentication vulnerability
```java
String sourceIP = request.getRemoteAddr();  
if (sourceIP != null && sourceIP.equals(APPROVED_IP)) {
	authenticated = true;
}
```
THis scheme relies on IP to authenticat an user. So to spoof the ip address mean to lure the authentication/authorization system, [[CWE-291(IP-for-AUTH)]]

### UDP authentication vulnerabillity
```java
String ip = request.getRemoteAddr();
InetAddress addr = InetAddress.getByName(ip);
if (addr.getCanonicalHostName().endsWith("trustme.com")) {
    trusted = true;
}
```
This snippet of code relies on the [[DNS]] lookup of the host header

### Children CWE
- [[CWE-291(IP-for-AUTH)]]
- [[CWE-293(Using-referer-field-for-auth)]]
- [[CWE-350(Reliance-on-a-DNS-resolution-for-a-security-and-critical-action)]]

| Nature   | Type                                                       | ID                                                       | Name                                                                         |
| -------- | ---------------------------------------------------------- | -------------------------------------------------------- | ---------------------------------------------------------------------------- |
| ChildOf  | ![Class](https://cwe.mitre.org/images/icons/class.gif)     | [1390](https://cwe.mitre.org/data/definitions/1390.html) | Weak Authentication                                                          |
| ParentOf | ![Variant](https://cwe.mitre.org/images/icons/variant.gif) | [291](https://cwe.mitre.org/data/definitions/291.html)   | [[CWE-291(IP-for-AUTH)]]                                                     |
| ParentOf | ![Variant](https://cwe.mitre.org/images/icons/variant.gif) | [293](https://cwe.mitre.org/data/definitions/293.html)   | [[CWE-293(Using-referer-field-for-auth)]]                                    |
| ParentOf | ![Variant](https://cwe.mitre.org/images/icons/variant.gif) | [350](https://cwe.mitre.org/data/definitions/350.html)   | [[CWE-350(Reliance-on-a-DNS-resolution-for-a-security-and-critical-action)]] |
| PeerOf   | ![Class](https://cwe.mitre.org/images/icons/class.gif)     | [602](https://cwe.mitre.org/data/definitions/602.html)   | Client-Side Enforcement of Server-Side Security                              |
| ChildOf  | ![Class](https://cwe.mitre.org/images/icons/class.gif)     | [287](https://cwe.mitre.org/data/definitions/287.html)   | [[CWE-287(improper-authentication)]]                                         |

