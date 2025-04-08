By default, NGINX Plus caches all responses to requests made with the HTTP `GET` and `HEAD`

```sh
 /docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
frontend-1  | /docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
frontend-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: /etc/nginx/conf.d/default.conf differs from the packaged version
frontend-1  | /docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
frontend-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
frontend-1  | /docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
	frontend-1  | 2025/04/06 20:42:00 [emerg] 1#1: "http" directive is not allowed here in /etc/nginx/conf.d/default.conf:1
	frontend-1  | nginx: [emerg] "http" directive is not allowed here in /etc/nginx/conf.d/default.conf:1
	frontend-1 exited with code 1 
```