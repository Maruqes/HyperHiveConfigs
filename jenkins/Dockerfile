FROM jenkins/jenkins:latest

USER root

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ansible \
        gcc \
        golang-go \
        make \
        nodejs \
        npm \
		openssh-client \
        git \
    && rm -rf /var/lib/apt/lists/*

USER jenkins
