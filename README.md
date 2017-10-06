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
    
    ENV JAVA_VERSION=8 
    JAVA_UPDATE=144 
    JAVA_BUILD=01
    JAVA_PATH=090f390dda5b47b9b721c7dfaa008135    
    JAVA_HOME=/usr/lib/jvm/default-jvm
   

**1. Update the package repository**

    #Upgrading system on alpine
    RUN apk upgrade
    RUN apk add wget

**2. Add JDK

    RUN apk add openjdk8-jre-base --update-cache --repository http://dl-3.alpinelinux.org/alpine/edge/testing/ --allow-untrusted \
    && rm -rf /var/cache/apk/*

**3. Expose Container VOLUME**

    #Expose volume
    VOLUME /app
    VOLUME /tmp

**4. Clean your image**

    RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


**5. Add your Java/SpringBoot component**

    ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]


## B - Create docker image from docker file

    $ docker build -f Dockerfile -t demo/spring-java:8 --rm=true .

The "." is mandatory :) do not forget it !!!!


## C - RUN the Image freshly created
    $ docker run -p 8080:8080 -t demo/spring-java:8

Or

    $ docker run -p 8080:8080 -t demo/spring-java:8 <map Volume for log for example>