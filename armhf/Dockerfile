FROM arm32v6/alpine AS build

ADD bin/ /usr/bin/

RUN ["cross-build-start"]

RUN apk add --no-cache libc6-compat gcc g++ git go
RUN go get -u github.com/awslabs/amazon-ecr-credential-helper/ecr-login/cli/docker-credential-ecr-login

FROM v2tec/watchtower:armhf-latest AS watchtower
COPY --from=build /root/go/bin/docker-credential-ecr-login /bin/docker-credential-ecr-login