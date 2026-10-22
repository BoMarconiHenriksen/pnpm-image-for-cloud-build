# PNPM Image for Google Cloud Build and Gitlab CI

This image can be used in `Google's Cloud Build` and `Gitlab's CI` if you need an image with [pnpm](https://pnpm.io/).

## Latest Version and Tags

Latest version `pnpm:latest` and `pnpm:node-lts-2.0.3` is using:

NodeJS: 24.22.1

PNPM: 11.17.0

Migration from Pnpm version 10 to version 11 https://pnpm.io/11.x/migration

`pnpm:node-lts-2.0.3` includes the fix for the eMudhra Intermediate CA Compromise (October 2026). The image no longer trusts the compromised eMudhra certificates. Tags older than `pnpm:node-lts-2.0.2` do not have the fix.

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
 - name: "bomarconi/pnpm:node-lts-2.0.3"  
    id: pnpm install  
    entrypoint: pnpm  
    args: ["install"]  
    dir: "frontend"  

 # Run frontend unit tests
  - name: "bomarconi/pnpm:node-lts-2.0.3"  
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
    - pnpm install --no-frozen-lockfile
    - pnpm test:unit
  artifacts:
    when: always
    paths:
      - frontend/coverage
    expire_in: 30 days
```
