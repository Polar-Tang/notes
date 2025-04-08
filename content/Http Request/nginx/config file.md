
#### Config file def:
The configuration in the nginx file are like instructions for our nginx proxy
- the key value pairs used in nginx.conf are called directives
- and the context are the blose which hold those directives
#### **Example**
#### Serving static html
For the static files, these should be served from different local directories, tipically the html files are all saved in `/data/www` and `/data/mages` for the images. If we want to serve this static files we will need an [server](https://nginx.org/en/docs/http/ngx_http_core_module.html#server) block, inside an [http](https://nginx.org/en/docs/http/ngx_http_core_module.html#http) block with two locations blocks
```nginx
server {
	server_name example.com
	location / {
		root /data/www;
	}
	location /images/ {
		root /data;
	}
}
```

## `location` directive
https://nginx.org/en/docs/http/ngx_http_core_module.html#location
location first select the endpoint of the URI. All the htmls will be serve with /data/www as the root of the URI.
Now in our nginx config, the html will be served in `http://localhost/` and the images in the `http://localhost/images/example.png`
*note*:
>`/var/log/nginx/access.log`it's a symbolic link (a pointer to another file or directory) to `/dev/stdout` if you're using docker and want to see this logs use `docker logs <container-name>` 

#### Routing static files with react
React routes all the endpoint in strange ways, because it's a SPA, we need to configure nginx for this behaviour, otherwise nginx will search literally for the specified endpoint:
```sh
"GET /login HTTP/1.1" 404 465 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0" "-" 
```
Here the browser sends an HTTP request to the server for `/login` and the server responds with a 404 because `/login` doesn't exist as a file.
At the same time that react is rendered the component of the same endpoint
```sh
"GET /assets/index-DOYdmDVo.js HTTP/1.1" 304 0 "http://localhost/login" "Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0" "-"
```
So we are doing two request at the same time.
We tell nginx to handle this behaviour with
- `try_files $uri /index.html;` : When a file or directory is matching not found routes to `index.html`
- The root directive is set outside the location 
```ngix
server {
    root /data/www;
    index index.html;

    location / {
        try_files $uri /index.html;
    }
}
```
### Setting Nginx as a proxy server
#### Proxy def:
A proxy is a server that receives requests, passes them to the proxies servers, retrieves the response from them, and sends them back to the clients.
#### `listen` directive
https://nginx.org/en/docs/http/ngx_http_core_module.html#listen
For this we could declare the listen directive. The `listen` directive in an NGINX `server` block specifies the port on which NGINX will listen for incoming connections.
```nginx
server {
	listen 5000;
  
	root /data/www;

	location / {
	}
```
Here nginx will be listeng to the port 5000 instead of the port 80
**Form example** here's listening to the port 8080 to serve files, and with the location / directive, is serving directly in the root of the url. And the root
#### `proxy_pass`
https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass
The `proxy_pass` directive sends the incoming request from the client to a different server (a proxy server).
You could set this in the server block, rather as a address or as a uri
###### URI
- As a uri
	```nginx
	location /name/ {
    proxy_pass http://127.0.0.1/remote/;
	}
	```
	- Now the request to `localhost/name/test` are [normalized](https://nginx.org/en/docs/http/ngx_http_core_module.html#location) and routed to `localhost/remote/test`

###### Adress
- Now as an address the thing change
	```
	location /some/path/ {
	    proxy_pass http://127.0.0.1;
	}
	```
	The entire original URI is passed unchanged to the server.

#### pending to check https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_cache_bypass


### Upstream server
Upstream server in nginx refers to the backend server that nginx proxies to

### Load balancing
To load balance our website we need to specify the number of worker process. The work processes are like the number of nginx instance, and this number use to be the number of the server cpu cores

```nginx
worker_processes auto;

# In event we define the number of connections handle for each worker proces
events {
	worker_connections 1024;
}

# In http we define theconfiguration for the nginx server
# How to listen for connections
# Which domain or subdomain the configuration applies to 
# how to use those request
http {
	# indlude the extensions type in the request
	include mime.types;

	# The upstreams are the server where the nginx forwards its request
	upstream nodejs_cluster {
		# forward the request to the nginx server less busy
		least_conn;

		server 127.0.0.1:3001;
		server 127.0.0.1:3002;
		server 127.0.0.1:3003;
	}

	server {
		# Port
		listen 8080;
		server_name localhost;

		# The location block is about how to handle that endpoint
		location / {
			# Pass the a to the nginx traffic
			proxy_pass http://nodejs_cluster;
			proxy_set_header Host $host;
			# Pass the real IP 
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header X-Forwarded-Proto $scheme;

			proxy_set_header Authorization $http_authorization;
			
			# The downstream are the server where the request pass when return
		}
	}
}
```
![[Pasted image 20250406154418.png]]


Hello, i want to add header to my backend in a docker compose. this backend is made with node js, and the frontend with react, is have a nginx.conf file to set nginx as a proxy in my dfocker container:
```
COPY --from=build /app/nginx.conf /etc/nginx/conf.d/default.conf
```
in this command from the docker file of the frontend i embedded directives to my nginx.conf file.
In this configuration i have a server with two different locations:

```
server {
			# The server is listening in port 5173, so i could use the same port at localhost which is used for vite in pre-production
            listen 5173;

			# the server location isn't actually a directory of 
            location /api/ {
	            # redirect the traffic to the 3000 port
                proxy_pass http://127.0.0.1:3000;

				# Add a header for debugging
				add_header X-server-header "my server header content!";
            }

			location / {
				# ...
			}
        }
```
So if we request the url literal `http://localhost:5173` we got a 301:
```http      
HTTP/1.1 301 Moved Permanently
Server: nginx/1.27.4
Date: Mon, 07 Apr 2025 04:51:38 GMT
Content-Type: text/html
Content-Length: 169
Location: http://localhost:5173/api/
Connection: keep-alive
X-server-header: my server header content!

<html>
<head><title>301 Moved Permanently</title></head>
<body>
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.27.4</center>
</body>
</html>
```
You can see i'm seeing the `X-server-Header` header, however, the proxy pass is used "To pass a request to an HTTP proxied server, the [proxy_pass](https://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass) directive is specified inside a [location](https://nginx.org/en/docs/http/ngx_http_core_module.html#location)." So i assume if i request `http://127.0.0.1:3000` which is the url from my backend and also the url the nginx is proxing, i will see `X-server-header: my server header content!`, but the response only seems to contain the headers i already had set from the backend
````http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://localhost:5173
Vary: Origin
Access-Control-Allow-Credentials: true
Content-Type: application/json; charset=utf-8
Cache-Control: public, max-age=604800
Content-Length: 281
ETag: W/"119-T9sXqr61w4Gb1niOIDf+1B3Gso4"
Date: Mon, 07 Apr 2025 05:16:32 GMT
Connection: keep-alive
Keep-Alive: timeout=5

[{"title":"magic mushrooms","description":"Once you eat one you will never want to stop"},{"title":"Magic ide","description":"Nothing of emacs, you want to stay awake while you code? use this magic ide"},{"title":"mana posion","description":"You will need to recover your energy"}]   
```

So i wrap all of this in a http with the directive i should mention before:

```
http {
        proxy_cache_path / keys_zone=mycache:10m;
# here goes the server context i showed you before
}
```

But in my docker i rewriting the default.conf file, at this line `COPY --from=build /app/nginx.conf /etc/nginx/conf.d/default.conf`. at the moment i deploy my docker compose all directories works, except for the "frontend" directory where i do have this nginx configuration. the specifiv error it's showed at this logs:

```
frontend-1  | 2025/04/06 20:42:00 [emerg] 1#1: "http" directive is not allowed here in /etc/nginx/conf.d/default.conf:1
frontend-1  | nginx: [emerg] "http" directive is not allowed here in /etc/nginx/conf.d/default.conf:1
frontend-1 exited with code 1
```

It seems like default.conf should not used with the http context. If i need to use the http context what should i do to incorporate the `proxy_cache_path / keys_zone=mycache:10m;` directive so i could utilize cache functionallities in my nginx proxy?