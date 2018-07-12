FROM jenkins/jenkins:lts

MAINTAINER Jason Wordtmann <jason.l.wordtmann@gmail.com>

EXPOSE 8080 50000

ENV DOCKER_VERSION=17.04.0-ce DOCKER_COMPOSE_VERSION=1.14.0 KUBECTL_VERSION=v1.7.12

# Use Root to setup kubectl, docker-ce, docker-compose
USER root
WORKDIR /usr/local/bin

# Update packages
RUN apt-get update -qq && apt-get install -y -qq --no-install-recommends \
    make \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add docker-ce
RUN curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-${DOCKER_VERSION}.tgz \
                && tar --strip-components=1 -xvzf docker-${DOCKER_VERSION}.tgz -C /usr/local/bin \
                && chmod -R +x /usr/local/bin/docker && groupadd -g 999 docker && usermod -aG docker jenkins

# Add docker-compose
RUN curl -L https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-Linux-x86_64 -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose

# Add kubectl
RUN curl -L https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl -o /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl

# Back to Jenkins home
USER jenkins
WORKDIR $JENKINS_HOME
