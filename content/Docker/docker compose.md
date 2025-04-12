#### Docker compose
A yml file that allows you to deploy more than two dockerfiles.


In Docker Compose, services can communicate using their service names as hostnames. Instead of using `172.19.0.3`, try connecting to `bb-db` (the service name) from your other containers.