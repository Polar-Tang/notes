The nginx has the rules to cache
- Cache GET and HEAD with no Set-Cookie response
- Uniqueness defined by raw URL or
- Cache time defined by
	- X-Accel-Expires
	- Cache-Control
	- Expires
RFC defines how an nginx should cache in https://www.ietf.org/rfc/rfc2616.txt
### Create a cache on disk
TGO configure nginx for caching on disk, define the path 

```nginx
proxy_cache_path /data/nginx/cache levels=1:2 keys_zone=mycache:10m inactive=60m;

server {
	listen 80;
	location /uploads/ {
		proxy_pass http://localhost:3000/uploads/;
		proxy_cache mycache;
	}
	
```
![[Pasted image 20250525173743.png]]