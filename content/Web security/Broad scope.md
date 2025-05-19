We need to set an attack stategy before starting attacking, how else would we even recognize a suiteable target if we saw it?
#### Google dorking:
- `Site:*.target.com -www`
- Open subdomain + investigate
- `Site:*.target.com -www -login`
- Open nex subdomain + invistigate
- `Site:*.target.com -www -login -register`
#### WaybackMachine (not waybackurls)
Wayback machine bring us a lot of functionallity
	![[Pasted image 20250506234106.png]]
	We could see the mimetypes most registered, if this mimetypes are jpg, the taget might not be very interesting to investigate.

#### Don't stick in google, alse go to yahoo, duckduckgo, bing

#### General manual recon tips
- Investigate all the subdomains
	- If it's static page move on
	- If you fin functionallity test it using your [[main app methodology]]
- Use https://pentest-tools.com/information-gathering/google-hacking
- First run your automation, then start manual testing
	- Check automation results after manual testing
- If the subdomain looks interesting, dig deep
- Trust you rat instincts 
- If you come across a new technology, look up how to hack it
- If you see a custom login page, try directory brute forcing
	- YOu might find unprotected pages
	- Make sure the programs allows it
	- Keep the amount of requests/sec low
- Do write you own tools or contribute to others