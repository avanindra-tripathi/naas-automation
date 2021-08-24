FROM ubuntu:bionic
LABEL Description="This is a base image, which provides a selenium environment"

ARG user=jenkins
ARG group=jenkins
ARG uid=1341
ARG gid=1341
RUN getent group ${gid} || groupadd -g ${gid} ${group}
RUN useradd -c "Jenkins user" -d /home/${user} -u ${uid} -g ${gid} -m ${user}

VOLUME /mvn

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get install -y apt-utils automake pkg-config libpcre3-dev zlib1g-dev liblzma-dev && \
    apt-get install -y curl net-tools build-essential software-properties-common bzip2 libbz2-dev git wget unzip vim openjdk-8-jdk && \
    apt-get install -y git xz-utils xvfb libxi6 libgconf-2-4 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ENV INSTALL_DIR=${INSTALL_DIR:-/usr}

ARG MAVEN_VERSION=${MAVEN_VERSION:-3.6.0}
ENV MAVEN_VERSION=${MAVEN_VERSION}
ENV MAVEN_HOME=/usr/apache-maven-${MAVEN_VERSION}
ENV PATH=${PATH}:${MAVEN_HOME}/bin
RUN curl -sL http://archive.apache.org/dist/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  | gunzip \
  | tar x -C /usr/ \
  && ln -s ${MAVEN_HOME} /usr/maven

ENV PATH=${PATH}:${JAVA_HOME}/bin

RUN curl -sS -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add
RUN echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list
RUN apt-get -y update
RUN apt-get -y install google-chrome-stable
RUN apt update && \
    apt-get install -y firefox && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


RUN usermod -a -G root ${user}
