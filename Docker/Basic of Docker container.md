## Run Docker container

```console
docker run -d -p seqvence/static-site
	-d 	Background
	-p	Ports
	--name	identifiable name
```

**When giving ports, follow this - `OurPCPort:MapPort`**

---
<br>

## Find Running Containers

```console
docker ps -a
	-a 	Hidden files
```	
## Delete all docker data from PC [Read more](https://www.digitalocean.com/community/tutorials/how-to-remove-docker-images-containers-and-volumes)
```console
docker system prune -a
```
