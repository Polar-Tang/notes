
-----
- **CWE** map: [[CWE-287(improper-authentication)]]
- Sources:
	https://portswigger.net/web-security/authentication
	https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html
### Authentication
**Authentication** (**AuthN**) means the process of verify who you are in an application. it's a critical process on many application. Many of their vulnerabilities used to be [logic flaw](summary_logic_flaw.md), the logical issues use to be undetected by scanner tools and use to pass overlook . That's why we got to review the authentication flow and come up with [edge cases](https://en.wikipedia.org/wiki/Edge_case).
Basically authentication is "what you are", which is different from authorization which likely means "what you do" and to do more than what you are allowed to do, it would be a [[Broken_authorization]] issue
### Types
nowadays this process rely in a lot of technologies, and there are 3 differents types.
- Something you **have**, This is a physical object such as a mobile phone or security token. These are sometimes called "possession factors".
- Something you **are** or do. For example, your biometrics or patterns of behavior. These are sometimes called "inherence factors".
- Something you **know**, a factor that identify you for authentication, (e.g. password). This are related in **password-based vulnerabilities**  and that's what we are we focusing now
##### Password-based Vulnerabilities
Sometimes to verify as a user you need to provide credentials which only you know, in case an attacker has you password and username is sufficient proof of the user's identity. So there are different ways to obtain the user's password, rather by "guessing" it through a brute force attack, or by enumerating user names via responses and [check if they password is leaked](https://haveibeenpwned.com/API/v3#PwnedPasswords)

As an attacker,you could brute-force different fields besides the password, for example, the email, which use to have the following pattern: `firstname.lastname@somecompany.com` also high-privileged user has a predictable name, such as `admin` or `administrator` check whether the website discloses potential usernames publicly, in post, in verbose errors.
But usually many attempts will be blocked somehow
- Locking the account that the remote user is trying to access if they make too many failed login attempts
- Blocking the remote user's IP address if they make too many login attempts in quick succession
#### Examples

When the password or the name is right normally it shows a response different wrong credential's response ones, this way is possible to enumerate valid usernames:
[[lab-password-reset-broken-logic-2]], [[lab-username-enumeration-via-different-responses]], [[lab-username-enumeration-via-subtly-different-responses]],
[[simple bruteforce on PHP]] 
##### Bypassing controls
It's worth to identify after how many attempts an IP is blocked, and how is reset, maybe after login it can reset, like [[lab-broken-bruteforce-protection-ip-block]]. Other times it will lock a correct account after many atempts: [[lab-username-enumeration-via-account-lock]].
#### Multifactor logic flaws
Sometimes the account is not [1FA]([[CWE-308("1FA")]]), but the logic has security bugs (the hardest bug to detect)
[[lab-2fa-simple-bypass]], 
#### Take aways
A way to preveting is by using rate limit in a application, after many request is a shot period of time you account is locked. 
Typically, the IP can only be unblocked in one of the following ways:

- Automatically after a certain period of time has elapsed
- Manually by an administrator
- Manually by the user after successfully completing a CAPTCHA

