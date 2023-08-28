# PNPM Image for Google Cloud Build and Gitlab CI

This image can be used in `Google's Cloud Build` and `Gitlab's CI` if you need an image with [pnpm](https://pnpm.io/).

## Latest Versions and Tags

Latest version is using:

NodeJS: 18.16.1

PNPM: 8.7.0

The image `pnpm:node-lts-0.4` is using:

NodeJS: 16.19.0-buster

PNPM: 7.27.0

## Download the PNPM Image

It's possible to download the image here <https://hub.docker.com/r/bomarconi/pnpm>

## Example Cloud Build

In this example the frontend is in a folder called frontend.

```yaml
steps:  
 # Pnpm install 
 - name: "bomarconi/pnpm:node-lts-0.4"  
    id: pnpm install  
    entrypoint: pnpm  
    args: ["install"]  
    dir: "frontend"  

 # Run frontend unit tests
  - name: "bomarconi/pnpm:node-lts-0.4"  
    entrypoint: pnpm  
    id: Run frontend unit tests  
    args: ["test:unit"]  
    dir: "frontend"  
```

# Example Gitlab CI

An example of test coverage in Gitlab CI with the pnpm image.

```yaml
image: bomarconi/pnpm:latest

stages:
  - test-unit

test-unit:
  coverage: '/Statements\s*:\s*([^%]+)/'
  only:
    - master
  stage: test-unit
  before_script:
    - cd frontend
  script:
    - pnpm install
    - pnpm test:unit
  artifacts:
    when: always
    paths:
      - frontend/coverage
    expire_in: 30 days
```

## Build Image

```bash
git tag -a v1.0.2
git push v1.0.2
```
