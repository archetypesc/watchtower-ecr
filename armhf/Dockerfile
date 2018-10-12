FROM arm32v6/alpine AS build

# https://resin.io/blog/building-arm-containers-on-any-x86-machine-even-dockerhub/
ADD bin/ /usr/bin/

RUN ["cross-build-start"]

# https://github.com/awslabs/amazon-ecr-credential-helper#installing
RUN apk add --no-cache libc6-compat gcc g++ git go \
	&& go get -u github.com/awslabs/amazon-ecr-credential-helper/ecr-login/cli/docker-credential-ecr-login

# final image has to be alpine, scratch doesn't support our env vars or credentials file
FROM arm32v6/alpine
COPY --from=v2tec/watchtower:armhf-latest / /
COPY --from=build /root/go/bin/docker-credential-ecr-login /bin/docker-credential-ecr-login

ENTRYPOINT ["/watchtower"]