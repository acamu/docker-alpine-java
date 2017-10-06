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

    # Dockerfile
    FROM  alpine:3.5

    MAINTAINER  Author Name <author@email.com>
    ENV JAVA_VERSION 8u31
    ENV BUILD_VERSION b13


**1. Update the package repository**

    #Upgrading system on alpine
    RUN apk upgrade
    RUN apk add wget

**2. Expose Container VOLUME**

    #Expose volume
    VOLUME /app
    VOLUME /tmp

**3. Clean your image**

    RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


**4. Add your Java/SpringBoot component**

    ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]

