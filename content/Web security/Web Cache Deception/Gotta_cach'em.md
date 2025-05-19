### Cache in CND
- The cache it's an old internet feature that allows to store a response so the user can get the same response without request the resource again, this could increase performance and reduce the latency
- The CDN that act as a proxy are very popular and widely used for most of the websites
### How CDNs know which responses to catch and which not?
CDN works **fingerprinting the request using a key**. This key is usually generated using the URL and host header.
To determine what path to be cached exist a "cache rules" defined by the origin server.
**ORIGIN SERVER** parse and route the request, based on factors like headers, HTTP methods.
**ORIGIN SERVER** must extract the absolute URL, the cache path (the endpoint with no including cache buster or any parameter).
![[Pasted image 20250404232907.png]]
### Exploit techniques
So the RFC define certain special character with an special meaning, called **delimiters** but RFC is quite permissive with these characters. This is why the CDN caches could in many cases be different from the origin server cache rules. 
There are different techniques for explotation, first we need to understand:
- Delimiters discrepancies
- Normalization discrepancies
These also may be combined with the cache rules used by the cdn:
- cache by extension
- static file
- static directory
------
## Delimiters
RDC defines different characters which do have a special meaning. However this widely vary assuming the recnology used by the backend, framework, http server, and the recnology used by the frontend, cdn, cache proxy.
#### Delimiters in frameworks
###### Semicolon in Spring
- Many frameworks have special delimiters in **Spring** which is a Java framework, the semicolon is used as a delimiter for matrix variables, for example you got an endpoint filtering by products `/products;color=red/size=medium`   which is practly the same that this in express: `/products?color=red&size=medium`
	**more examples**:
  
	**URL:** `/MyAccount;var1=val` → **Path:** /MyAccount  
  
	**URL:** `/hello;var=a/world;var1=b;var2=c` → **Path:** /hello/world
###### Dot in ruby rails
- The dot in ruby rails is for file extension but also is a delimiter, by default it's an html extension.

	**URL:** `/MyAccount.html` → **Path:** /MyAccount (default HTML view)  
  
	**URL:** `/MyAccount.css` → **Path:** /MyAccount (CSS view or error if not present)  
  
	**URL:** `/MyAccount.aaaa` → **Path:** /MyAccount (default HTML view)
###### Null encoded byte in OPenLiteSpeed
- Null encoded byte in **OpenLiteSpeed**: This **HTTP server** (mostly used on wordpress) uses the null encoded byte as a classic delimiter to truncate the path.  
      
    **URL:** /MyAccount%00aaa → **Path:** /MyAccount
###### Newline encoded byte, in nginx
- `%0a` can sometimes be interpreted by certain servers (like Nginx) as a **path delimiter** or may alter the way the URL is parsed or rewritten.
	 **URL:** /users/MyAccount%0aaaa → **Path:** /account/MyAccount
	 
	When Nginx parses this path, the newline character might cause it to interpret `MyAccount` and `aaa` as separate segments, potentially affecting URL rewriting or request handling.

##### Detecting **ORIGIN** delimiters
1. Look for methods that aren't idempotent, such as post, or request with headers like `Cache-Control: no-store` or `Cache-Control: private`. The response (R0) will be used to compare the behavior to another requests with special characters.
2. Use the same request (R1) and apend a random value, if the endpoint it's `/home` try `/homeabc`. If the response (R1) is the same as R0, repeat step 1 and 2 with a different endpoint.
3. Now use a delimiter between the path and the random sufix, now R1 `/homeabcd` becomes `/home$abcd`, as R2. Compare this response (R2) with the base one (R0).  
If the messages are identical, the character or string is used as a delimiter.

##### Detecting **CACHE** delimiters
Cache servers often don't use delimiters aside from the question mark. It's possible to test this using a static request and response:
1. Identify cacheable requests, by looking for header like X-Cache with hit value. Once found use that request to compare to another requests (R0)
2. Send the same request with a URL path suffix followed by the possible delimiter and a random value.  
  
	GET `/static-endpoint<DELIMITER><Random>`
3. Compare the response with R0. If the messages are identical, the character or string is used as a delimiter.

------
## Normalization
- RFC defines the url-encoding to decode the characters to avoid changing the pathname meaning. 
- The origin server and the cache system, both, try to mapping the url by utilizing **URL parsers**. 
	- This process start by locating the delimiters to determine where the pathname start and where it ends. And after extracting it it’s normalized, which means decoding certain characters and also resolve the dot segment ((`/.`) for the parent directory and (`/..`) for the grandpa directory). 
- Some proxies may forward certain HTTP server may forward the pathname still encoded, eg:
	**Nginx, Node, CloudFlare, CloudFront and Google Cloud**
#### Detecting decoding behaviour

To test if a character is being decoded, compare a base request with its encoded version. For example:

/home/index → /%68%6f%6d%65%2f%69%6e%64%65%78

If the request are the same, and the x-cache didn’t change (it continues as HIT), it means that the origin server decodes the path before using it

#### **Dot-segment normalization**

RFC defines the behaviour to dive in the path of the URL, by using the dots exaclty as the directories navigation
##### **Detecting dot-segment normalization**
- Use path traversal payloads, and then replace the dots in its url-encoded versions
- Test in a non-cached endpoint or with a cache buster

GET /home/index?cacheBuster

GET **`/aaa/../`home/index?cacheBuster** or 
GET **`/aaa\..\`home/index?cacheBuster

![[Pasted image 20250404232652.png]]

If the first response is identical to the second response, this means that the origin server is normalizating before mapping. To detect this behaviour in a cacheable response, just do the sameand compare the X-Cache and Cache-Control headers to verify if the resource was obtained from the cache memory.

#### **Normalization discrepancies**

This table illustrate how different HTTP server and proxies normalize the path `/hello/..%2fworld.`
some of them normalize to` /world`, while other do not normalize it at all

![[Pasted image 20250403015931.png]]
So let’s combine the proxy and http server that have discord...

##### Microsoft IIS
- IIS normalize backslashes as normal slashes.
- No cdns recognize backslashes
![[Pasted image 20250403205801.png]]

-----
## **Cache rules**
To see how do combine these two discrepancies let's dive into the cache rules, in a vulnerable site an attacker could perform
- **Arbitrary Web Cache Deception**
- **Cache an xss**
- Perform DDOS

Let's dive in how CDN cache response, for example
### By extensions
- Some proxies store the thing as static assuming its file extension, so consider the following list, that clourflare considare to be static:
	![[Pasted image 20250403020250.png]]

##### Delimiters discrepancies
- When a character is used as a delimiter by the origin server but not the cache,
- it's possible to include an arbitrary suffix to trigger a cache rule and store any sensitive response. Example:
	![[Pasted image 20250403020540.png]]
- cosider [[lab-wcd-exploiting-path-mapping]]
##### delimiters normalization 
*(good in Nginx, Node, CloudFlare, CloudFront and Google Cloud)*
You could also do the same with the decode character, which could works nice if the origin server it’s decodes before parsing, or if the proxy is rewriting the url before forwardint it.
![[Pasted image 20250403020835.png]]

To avoid issues you could also decode the hashtag characters
![[Pasted image 20250403020904.png]]

----
### Static files
some files such as, `favicon.ico`, `robots.txt` are stored just by name
##### **Exploting static files by normalization**
Now let's mix the cdn that normalize with the origin server that don't normalize
![[Pasted image 20250404221301.png]]
	cdn that normalize before mapping the url. and servers that don't normalize at all
#### **Arbitrary cache deceptioning via normalization** 
![[Pasted image 20250404230800.png]]
	the frontend do normalize but not the backend

##### **Exploiting static directories with delimiters**
This could be exploting using the static paths
`GET /<Dynamic_Resource><Delimiter><Encoded_Dot_Segment><Static_File>`

![[Pasted image 20250403210413.png]]
###### Explot: path traversal
- If a character is used as delimiter by the origin server but not by the caching 
- the cdn server normalize the path before aplying the rule
	you could find a path traversal
	GET `/<Dynamic_Resource><Delimiter><Encoded_Dot_Segment><Static_Directory>`
![[Pasted image 20250403021556.png]]
- consider: [[lab-wcd-exploiting-cache-server-normalization]]

###### Exploit: cache a reflected XSS
- When a character is used a delimiter for the origin server but not by the cache proxy, it's possible to generate an arbitrary cache key
	`GET /<Backend_Path><Delimiter><Path_Traversal><Poisoned_Path>`![[Pasted image 20250403213350.png]]
>Cache server ignores the backend delimiter, so normalize the path and then cache it. But the backen utilizes the delimiter so it ignores everything that comes after the delimiter. Imagine we got a XSS reflected payload, so by doinf this we could cache the XSS in the website
![[Pasted image 20250404232826.png]]
  
Amazon, CloudFront, Microsoft Azure, and Imperva normalize the path before evaluating the cache rules by default.


-------
#### **Static directories**
A common behaviour in many cdns is to cache all the files which come from static directories, common examples are

- /static
- /assets
- /wp-content
- /media
- /templates
- /public
- /shared
##### **Exploiting static directories with normalization**
- When the origin server normalizes the path before mapping the endpoint 
- and the cache doesn't normalize the path before evaluating the cache rules, 
	you could add a directory path trasversal
	`GET /<Static_Directory><Encoded_Dot_Segment><Dynamic_Resource>`
![[Pasted image 20250403205014.png]]
-----
### Is `#`a delimiter
To fin a delimiter which is used by the cache proxy but not the server is rare
- Used to be rare characters
- Are ignored by the browser just like the hashtag
- As cache posoning don't require user interaction we could use such delimiters
	`GET /<Poisoned_Path><Front-End_Delimiter><Path_Traversal><Backend_Path>`
![[Pasted image 20250403213823.png]]

# Detect key normalization

To detect key normalization send a path in the request partially urlencoded, as the follows:

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e9cf9b3c-cf07-4efc-aa5c-840fe610a999/20d2b3b2-4c9f-48a9-8dfd-aeeb8bbbc975/image.png)

This behaviour it’s by default in imperva, azure and partially in akamai, also it’s configurable in cloudlfare, google cloud, GPC and fastly. So by using something like an XSS reflected we could combine it with the delimiter

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e9cf9b3c-cf07-4efc-aa5c-840fe610a999/d1d2ca89-997e-4333-9c53-f6f4f281c17f/image.png)

FRAGMENT

Usually the fragment is just ignored by the browser, so if u send it to the victim it wouldn’t be possible, depending on the cdn it may vary

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e9cf9b3c-cf07-4efc-aa5c-840fe610a999/611b8153-dc32-4ff5-afa3-2ae8ebd55778/image.png)

There are many dissents about the hash as a dalimiter, the inresting thing is in imperva becuase they are normalizing the key

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e9cf9b3c-cf07-4efc-aa5c-840fe610a999/e518750f-41e8-46a8-be34-a575d91a6bec/image.png)

## CACHE-WHAT-WHERE

The open redirect critical, it’s a redirect control by whatever header, which is not by itself a vulnerabillity. But supposed we could use a delimiter

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e9cf9b3c-cf07-4efc-aa5c-840fe610a999/a34d1c36-a240-4f3c-8f82-d8b69864f7e3/image.png)

so we control the logout with that header, using a cacheable endpoint

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e9cf9b3c-cf07-4efc-aa5c-840fe610a999/b8bd52de-3b7d-48b1-8776-8ade69660a25/image.png)

which is normalized

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/e9cf9b3c-cf07-4efc-aa5c-840fe610a999/2fb58df5-4b00-4226-a5be-e2cadcdeb32e/image.png)

so we poison the main js

### Takeaways
Use chache control on every sinhle request
Store request with sensitve data with the [PRIVATE CACHE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Caching#private_caches) header
