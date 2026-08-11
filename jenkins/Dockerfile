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
		iputils-ping \
    && rm -rf /var/lib/apt/lists/*

USER jenkins

RUN jenkins-plugin-cli --plugins \
        ansicolor \
        configuration-as-code \
        credentials-binding \
        dark-theme \
        pipeline-graph-view \
        ssh-agent \
        ssh-credentials \
        workflow-aggregator

COPY --chown=jenkins:jenkins jenkins.yaml /usr/share/jenkins/ref/jenkins.yaml
