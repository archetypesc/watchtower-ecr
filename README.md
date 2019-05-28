# Watchtower-ECR
![Docker Build](https://img.shields.io/docker/build/andyault/watchtower-ecr.svg)

[Docker Hub link](https://hub.docker.com/r/andyault/watchtower-ecr/)

A docker image based on [v2tec/watchtower](https://github.com/v2tec/watchtower) for use with AWS ECR.

This image uses [this method](https://resin.io/blog/building-arm-containers-on-any-x86-machine-even-dockerhub/)
to support builds on Dockerhub. Also, this image is no longer `FROM scratch` 
because we need either a home directory for the `~/.aws/credentials` file or we
need environment variables for `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.

# Explanation
[source](https://github.com/containrrr/watchtower/issues/38#issuecomment-429425120)

> if anyone is still having troubles with this, I was able to get watchtower to play nice by:
> * [adding the credential helper to the image myself](https://github.com/andyault/watchtower-ecr/blob/master/armhf/Dockerfile)
> * adding `{ "credsStore": "ecr-login" }` to `/config.json` (not included in my image)
>    * note that I wasn't able to use "credHelpers" or any of the tricks seen [here](https://github.com/docker/compose/issues/4948#issuecomment-392482085) without getting "no basic auth credentials" again :(
> * exposing my key id and secret key to watchtower by either env vars or the `~/.aws/credentials` file
>    * note that I wasn't able to get either of these to work until I made my `watchtower-ecr` start from `alpine` instead of from scratch

> My final `docker-compose.yml`:
> ```yml
> version: '3.2'
> services:
>   my-service:
>     image: <id>.dkr.ecr.<region>.amazonaws.com/my-image:latest
>   watchtower:
>     image: andyault/watchtower-ecr:armhf-latest
>     volumes:
>       - /var/run/docker.sock:/var/run/docker.sock
>       - /path/to/docker-config.json:/config.json
>     environment:
>       AWS_ACCESS_KEY_ID: <blank or id>
>       AWS_SECRET_ACCESS_KEY: <blank or key>
>     command: --interval 30 --debug
> ```
