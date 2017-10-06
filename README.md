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
    
    ENV JAVA_VERSION=8 
    JAVA_UPDATE=144 
    JAVA_BUILD=01
    JAVA_PATH=090f390dda5b47b9b721c7dfaa008135    
    JAVA_HOME=/usr/lib/jvm/default-jvm
   

**1. Update the package repository**

    #Upgrading system on alpine
    RUN apk upgrade
    RUN apk add wget

**2. Add JDK Openjdk jre**

    RUN apk add openjdk8-jre-base --update-cache --repository http://dl-3.alpinelinux.org/alpine/edge/testing/ --allow-untrusted \
    && rm -rf /var/cache/apk/*
    
**2 bis . Add Oracle JDK**
 
    RUN apk add --no-cache --virtual=build-dependencies wget ca-certificates unzip && cd "/tmp" && wget --header "Cookie: oraclelicense=accept-securebackup-cookie;" "http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/${JAVA_PATH}/jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && tar -xzf "jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && mkdir -p "/usr/lib/jvm" && mv "/tmp/jdk1.${JAVA_VERSION}.0_${JAVA_UPDATE}" "/usr/lib/jvm/java-${JAVA_VERSION}-oracle" && ln -s "java-${JAVA_VERSION}-oracle" "$JAVA_HOME" && ln -s "$JAVA_HOME/bin/"* "/usr/bin/" && rm -rf "$JAVA_HOME/"*src.zip && wget --header "Cookie: oraclelicense=accept-securebackup-cookie;" "http://download.oracle.com/otn-pub/java/jce/${JAVA_VERSION}/jce_policy-${JAVA_VERSION}.zip" && unzip -jo -d "${JAVA_HOME}/jre/lib/security" "jce_policy-${JAVA_VERSION}.zip" && rm "${JAVA_HOME}/jre/lib/security/README.txt" && apk del build-dependencies && rm "/tmp/"*


**3. Expose Container VOLUME**

    #Expose volume
    VOLUME /app
    VOLUME /tmp

**4. Clean your image**

    RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


**5. Add your Java/SpringBoot component**
	
    ADD gs-spring-boot-docker-0.1.0.jar app.jar
    RUN sh -c 'touch /app.jar'
    #Set your entrypoint
    ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]


## B - Create docker image from docker file

    $ docker build -f Dockerfile -t demo/spring-java:8 --rm=true .

The "." is mandatory :) do not forget it !!!!


## C - RUN the Image freshly created
    $ docker run -p 8080:8080 -t demo/spring-java:8

Or

    $ docker run -p 8080:8080 -t demo/spring-java:8 <map Volume for log for example>