Generic CI Build Image for Amazon Lambda (Node Environment)
========================================================

Introduction
------------

This project intend is to provide a lightweight but still feature-full continues integration build image for Amazon Lambda (Node Environment). It bundles several system packages with node and the Amazon Command Line tool.

Image includes:

* Node Runtime 4.3.2 (Current Lambda Node Environment)
* Latest AWSCLI Tool
* System packages like wget, jq, zip, unzip, curl, ssh, git, etc.

The system also includes the docker runtime engine allowing to build docker images within the build environment.

Runtime vs Build
-------------

This image is not a runtime image but rather used during the build process.

A typical pipelines uses this image to prepare the NPM modules, zip into an archive and push it to the designated Lambda function.

Usage
-------------

This image is usually used as build image during an CI process however the image can also be used standalone.

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
