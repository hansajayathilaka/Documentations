# Docker Compose and Intract with docker container
Docker compose is use to deploy whole project with many docker containers.
This is a python plugin.
Configurations are written in yml file. Normaly file name is "docker-compose.yml".

docker-compose.yml example:
```yaml
web-server:
	container_name: web
	image: seqvence/static-site
	ports:
		-"8080:80"		# Local:Docker
	mem_limit: 500M
	restart: always
```

## Run Docker compose
```console
docker-compose up -d
	-d	For Background
```
## Go inside the Docker container
```console
docker ps
docker exec -it <container_name> <terminal_type>
docker exec --user <user_name> -it <container_name> <terminal_type>
docker exec -it docker-zulip_database_1 /bin/bash
```
