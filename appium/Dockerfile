FROM ubuntu:16.04
FROM appium-base

#=======================================
# Setting JAVA_HOME in PATH
#=======================================
RUN { \
		echo '#!/bin/sh'; \
		echo 'set -e'; \
		echo; \
		echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
	} > /usr/local/bin/docker-java-home \
	&& chmod +x /usr/local/bin/docker-java-home

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/jre
ENV PATH $PATH:$JAVA_HOME/bin

ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

#===============================================================
# Display JAVA version
#===============================================================
RUN java -version

#===============================================================
# Includes latest node LTS, Appium & Appium Doctor version
#===============================================================
ENV NODEJS_VERSION=7.2.0 \
    PATH=$PATH:/opt/node/bin

WORKDIR "/opt/node"

RUN apt-get update -qqy \
    && apt-get install -y curl ca-certificates --no-install-recommends && \
    curl -sL https://nodejs.org/dist/v${NODEJS_VERSION}/node-v${NODEJS_VERSION}-linux-x64.tar.gz | tar xz --strip-components=1 && \
    npm install -g appium && \
    npm install appium-doctor -g && \
    npm cache clean && \
    apt-get remove --purge -y npm && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    apt-get clean

#=======================================
# Display Node version
#=======================================
RUN node -v

#=======================================
# Display npm version
#=======================================
RUN npm -v

#=======================================
# Check health of Appium Android
#=======================================
RUN appium-doctor --android

#============================================
# Add udev rules file with USB configuration
#============================================
ENV UDEV_REMOTE_FILE https://raw.githubusercontent.com/M0Rf30/android-udev-rules/master/ubuntu/51-android.rules
RUN mkdir /etc/udev/rules.d \
  && wget --no-verbose $UDEV_REMOTE_FILE -O /etc/udev/rules.d/51-android.rules

#=======================================
# Expose default port of appium
#=======================================
EXPOSE 4723

CMD appium
