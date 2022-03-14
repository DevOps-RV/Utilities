#Jenkins JNLP Swarm Client Slave
#Maintainer: Raghu Vamsi <devops.rv@outlook.com>

#https://plugins.jenkins.io/swarm/
#docker run -d --name <<name>>  -e COMMAND_OPTIONS="-master http://<<IP or URL>>:<<port>> -username <<username>> -password <<password>> -labels '<<name>>' -executors <<No of Executors>> -name <<name>>" <<IMAGE>>

ARG IMAGE=centos
ARG IMAGE_VERSION=latest
ARG user=jenkins
ARG group=jenkins
ARG UID=1000
ARG GID=1000


FROM ${IMAGE}:${IMAGE_VERSION}

ENV DIRPATH="/home/jenkins"
ENV SWARM_CLIENT_VERSION="3.9"
ENV COMMAND_OPTIONS ""

RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-* ; \
  sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
RUN dnf distro-sync; \
  yum update -y ; \
  yum install -y java wget

RUN groupadd --gid ${GID} ${group}
RUN useradd -c "Jenkins user" -d /home/${user} -u ${UID} -g ${GID} -m ${user}

RUN useradd -r -g root jenkins \
  && mkdir -p /home/jenkins \
  && chown jenkins ${DIRPATH}

WORKDIR $DIRPATH

RUN wget https://repo.jenkins-ci.org/releases/org/jenkins-ci/plugins/swarm-client/$SWARM_CLIENT_VERSION/swarm-client-$SWARM_CLIENT_VERSION.jar  -O swarm_client.jar \
  && echo -e '#!/bin/bash\njava -jar swarm_client.jar ${COMMAND_OPTIONS}' > run_client.sh \
  && chmod +x run_client.sh

CMD ["./run_client.sh"]