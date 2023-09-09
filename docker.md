
- compile inferno in Alpine.
- statically link to musl C on Alpine.
- make image less than 10MB.
- mount volume for /keydb


	docker run --rm -v /tmp/.X11-unix/:/tmp/.X11-unix -e DISPLAY -h caerwyn-rpi2 -v /home/pi/.Xauthority:/home/inferno/.Xauthority -v $HOME:$HOME acme-sac



# publish on docker hub.
- linux/arm   arm/alpine
- linux/386   i386/alpine
- On armv7, arm64, x86, x86_64
- inferno-os-arm-linux:alpine
- inferno-os-i386-linux:alpine
- setup minimal inferno runtime. 
- docker manifest create  caerwyn/inferno-os:latest

## commands
	docker tag local-image:tag new-repo:tagname
	docker push new-repo:tagname

- docker builds on different cluster nodes.
- install docker on each node.
- monitor change in git repo of dockerfiles. Add changed file to queue.
- Launch build on node with space. Push image to repo.
- Jenkins?  Slaves on each node?
- Automate build of docker images.


	groupadd inferno
	useradd -g inferno inferno
	chown /usr/inferno inferno
	USER inferno
  
# Changes

replace use of sbrk() with mmap in emu/port/alloc.c because sbrk didn't
work on Alpine. But mmap probably won't work on other Unixes.
Need to make it conditional on it being Linux


Docker compose file:
- docker inferno network for signer, styx, registry, ventisrv, httpd.
- Start network for all services in one go.
- make httpd log to stdout for 12factorApp

	docker run --rm -it --network inf -v /home/pi/github/inferno-contrib:/usr/inferno/contrib inferno-os:alpine-slim /bin/sh
	docker run --rm -it --network inf -v keydb:/usr/inferno/keydb inferno-os:alpine-slim /bin/sh
	docker run --rm -it --network inf -v keyring:/usr/inferno/usr/inferno/keyring inferno-os:alpine-slim /bin/sh

Use contrib to store startup scripts.
