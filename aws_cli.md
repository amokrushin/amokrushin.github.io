- [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)
- [Configuration and Credential Files](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html)

#### `Dockerfile`

```bash
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


#### `docker-compose.admin.yml`

```
version: '3.6'

services:
  aws:
    build: modules/awscli
    volumes:
      - ~/.aws:/root/.aws
```


```
COMPOSE_FILE=docker-compose.admin.yml dc run --rm aws configure --profile=test
```
