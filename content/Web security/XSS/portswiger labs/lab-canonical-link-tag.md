https://portswigger.net/web-security/cross-site-scripting/contexts/lab-canonical-link-tag
https://portswigger.net/research/xss-in-hidden-input-fields

link with rel cannonical are tags used for improve the SEO of a website by telling the browsers which page is most important
In this lab, when we searach for a ranfom value after some delimeter the response is beaing reflected in the brower
![[Pasted image 20250612223911.png]]
this proves once more the necessity of always searching for reflected input, even in the most unexpected cases.

#### Solution
```url
https://0a4a006204356de4802f176d00d00056.web-security-academy.net/?asd%27accesskey=%27X%27onclick=%27alert(1)%27%27
```
Vist that url and press `alt + x`