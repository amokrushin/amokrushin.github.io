# AWS ECR

#### awscli

`Dockerfile`

```
FROM alpine:3.6

RUN apk --no-cache add \
        python \
    && apk --no-cache add --virtual build-dependencies \
        py-pip \
    && pip --no-cache-dir install \
        awscli \
    && apk del build-dependencies

VOLUME /root/.aws
VOLUME /project
WORKDIR /project

ENTRYPOINT ["aws"]
```
