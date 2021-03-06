# WorkAdventure - Digitale Gesellschaft

## Introduction

This is a fork of [WorkAdventure (WA)](https://github.com/thecodingmachine/workadventure) specifically for the [Digitale Gesellschaft Schweiz](https://www.digitale-gesellschaft.ch/), forked because:
1. hosting on Lagoon
2. a couple of merges that have not made it into upstream workadventure yet

## Local Development

1. run containers
    ```
    docker-compose up -d
    ```
    Important: WA brings it's own traffik reverse proxy which tries to bind port 80 and 443, if you have any other container or tool running that tries to do the same, stop them first.

2. verify containers running with
    ```
    docker-compose logs -f front
    ```
    wait until you see `Compiled successfully`

3. Visit: http://play.workadventure.localhost/

The local development enviornment has realtime compile & livereload enabled, means if you change anything on the code of the application your browser will automaatically reloaded (it will take a couple of seconds as the compliation takes a bit).

### Common issues & Debugging with local Development enviornment

- The very first start of the containers can be quite slow, give it some time and follow the start progress with `docker-compose logs -f` (for all services) and `docker-compose logs -f [service]` (for a specific service)

## Remote Development

This Workadventure instance is deployed via Lagoon and has the following settings:

- Deploy `develop` branch to https://front.develop.workadventure-digiges.ch4.amazee.io/
- Deploy `main` branch https://front.main.workadventure-digiges.ch4.amazee.io/
- Deploy each PR in a new test environment - just create a PR, the URL schema for the test environment is: `front.pr-X.workadventure-digiges.ch4.amazee.io` (replace the `X` with our PR number)

## Deploy with Lagoon

This Repo is completely Lagoonized, the `.lagoon.yaml` will point Lagoon to use `docker-compose.lagoon.yaml` which contains the required lagoon labels.

As Lagoon needs to know the URLs of the other services it generates the autogenerated URLs by itself, but it doesn't know the cluster URL, this needs to be added via an global project level variable: `LAGOON_CLUSTER`, add it via lagoon-cli:

```
lagoon add variable -p [projectname] -N LAGOON_CLUSTER -V [cluster-suffix] -S global
```
