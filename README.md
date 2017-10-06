# docker-alpine-java
Simple docker image with java JDK installed

We will create a docker image for a JAVA Application (Simple or not)

**[Note]**
- First of all we will add custom JDK because  sometime we MUST use a specific one



**[Pre requisite]**
- Docker installed

- for execute script Docker deamon must be started : dockerd with  --iptables=false



## A - Create a docker file 
Who begin by that:

\# Dockerfile
FROM  alpine:3.5

MAINTAINER  Author Name <author@email.com>

ENV JAVA_VERSION 8u31

ENV BUILD_VERSION b13

