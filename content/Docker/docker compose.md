#### Docker compose
A yml file that allows you to deploy more than two dockerfiles.


In Docker Compose, services can communicate using their service names as hostnames. Instead of using `172.19.0.3`, try connecting to `bb-db` (the service name) from your other containers.

Here are some values that are usually used in the docker compose yaml file 

### [`volumes`](https://docs.docker.com/reference/compose-file/services/#volumes)
The `volumes` attribute define mount host paths or named volumes that are accessible by service containers


## [Example](https://docs.docker.com/reference/compose-file/volumes/#example)

The following example shows a two-service setup where a database's data directory is shared with another service as a volume, named `db-data`, so that it can be periodically backed up.

```yml
services:
  backend:
    image: example/database
    volumes:
      - db-data:/etc/data

  backup:
    image: backup-service
    volumes:
      - db-data:/var/lib/backup/data

volumes:
  db-data:
```

The `db-data` volume is mounted at the `/var/lib/backup/data` and `/etc/data` container paths for backup and backend respectively.

Running `docker compose up` creates the volume if it doesn't already exist. Otherwise, the existing volume is used and is recreated if it's manually deleted outside of Compose.