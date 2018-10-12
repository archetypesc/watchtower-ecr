# Watchtower-ECR
![Docker Build](https://img.shields.io/docker/build/andyault/watchtower-ecr.svg)

[Docker Hub link](https://hub.docker.com/r/andyault/watchtower-ecr/)

A docker image based on [v2tec/watchtower](https://github.com/v2tec/watchtower) for use with AWS ECR.

This image uses [this method](https://resin.io/blog/building-arm-containers-on-any-x86-machine-even-dockerhub/)
to support builds on Dockerhub. Also, this image is no longer `FROM scratch` 
because we need either a home directory for the `~/.aws/credentials` file or we
need environment variables for `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.