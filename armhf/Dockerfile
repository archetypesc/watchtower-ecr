FROM alpine AS build

RUN apk add --no-cache libc6-compat gcc g++ git go \
	&& go get -u github.com/awslabs/amazon-ecr-credential-helper/ecr-login/cli/docker-credential-ecr-login

FROM v2tec/watchtower:armhf-latest AS watchtower
COPY --from=build /root/go/bin/docker-credential-ecr-login /bin/docker-credential-ecr-login