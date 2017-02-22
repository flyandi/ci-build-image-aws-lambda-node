[![Build Status](https://travis-ci.org/flyandi/ci-build-image-aws-lambda-node.svg?branch=master)](https://travis-ci.org/flyandi/ci-build-image-aws-lambda-node)

Generic CI Build Image for Amazon Lambda (Node Environment)
========================================================

Introduction
------------

This docker image is intended to provide a lightweight but still feature-rich build image for Amazon Lambda Functions. The image includes several standard system packages, node and the Amazon Command Line Tool.

It's purpose is mainly directed towards continues integration build jobs.

Image contains:

* Node Runtime 4.3.2 (Current Lambda Node Environment)
* Latest AWSCLI Tool
* System packages like wget, jq, zip, unzip, curl, ssh, git, etc.

The system also includes the docker runtime engine allowing to build docker images within the build environment.

Runtime vs Build
-------------

This image is not a runtime image but rather used during CI build processes.

A typical pipelines uses this image to prepare the NPM modules, zip into an archive and push it to the designated Lambda function.

Standalone Usage
-------------

```shell
docker pull flyandi/ci-build-image-aws-lambda-node
```

Examples
-------------

GitLab CI:

```yaml
stages:
  -build

build:
  stage: build
  image: flyandi/ci-build-image-aws-lambda-node
  script:
    - mkdir -p ./build 
    - cd ./src && npm install --no-progress
    - zip -X -r ../build/release.zip *
    - aws lambda update-function-code --function-name LambdaFunctionName --zip-file fileb://./build/release.zip 
  cache:
    paths:
      - ./src/node_modules
  artifacts:
    paths:
      - ./build/release.zip
    expire_in: 1 day
```
