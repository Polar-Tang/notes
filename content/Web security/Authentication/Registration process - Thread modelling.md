https://www.bugbountyhunter.com/methodology/zseanos-methodology.pdf 
// page 41

#### 1. What's require to sign up?
 If there's a lot of information (Name, location, bio, etc), where is this then reflected after signup?
- Uploading photo
- Display name and profile description
#### 2. Can i register my social media account
if this yes, 
- is this implemented via some type of `Oauth` flow which contains leakable tokens
- What social media accounts are allowed
- What information do they trust from my social media profile?

#### 3. What characters are allowed?
At this stage,
enter the XSS process testing. `<script>`Test may not work but `<script` does.

#### 4. Can I sign up using @target.com or is it blacklisted?
If yes to being blacklisted, wuestion why? 
#### 5. What happen if i revisit the page after sign up
Does it redirect, and i can control this control this with a parameter?
#### 6. What parameters are used in this endpoint?
Any listed in the source or JavaScript
#### 7. What JS files do on this page? 

#### 8. Dork for register page
```
site:example.com inurl:register inurl:&
```
```
site:example.com inurl:signup inurl:&
```
```
site:example.com inurl:join inurl:&.
```
