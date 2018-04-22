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

#### Credential helper

[Authenticating Amazon ECR Repositories for Docker CLI with Credential Helper](https://aws.amazon.com/ru/blogs/compute/authenticating-amazon-ecr-repositories-for-docker-cli-with-credential-helper/)

The Amazon ECR Docker Credential Helper is a credential helper for the Docker daemon that makes it easier to use Amazon Elastic Container Registry. [awslabs/amazon-ecr-credential-helper](https://github.com/awslabs/amazon-ecr-credential-helper) automatically gets credentials for Amazon ECR on docker push/docker pull.
