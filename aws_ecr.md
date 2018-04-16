# AWS ECR

#### awscli

https://github.com/aws/aws-cli

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

#### credential helper

https://github.com/awslabs/amazon-ecr-credential-helper

https://aws.amazon.com/ru/blogs/compute/authenticating-amazon-ecr-repositories-for-docker-cli-with-credential-helper/
