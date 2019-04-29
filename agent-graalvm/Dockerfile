FROM quay.io/cloudfirst/jenkins-agent-maven-35-centos7

MAINTAINER Joshua Mathianas <mathianasj@cloudfirst.dev>

ARG GRAAL_VERSION=1.0.0-rc16

ENV GRAAL_CE_URL=https://github.com/oracle/graal/releases/download/vm-${GRAAL_VERSION}/graalvm-ce-${GRAAL_VERSION}-linux-amd64.tar.gz
ENV GRAALVM_HOME /opt/graalvm
ENV JAVA_HOME /opt/graalvm
ENV MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"

ENV PATH=$PATH:$JAVA_HOME/bin

USER 0

RUN  yum update -y && \
	 yum install -y --setopt=skip_missing_names_on_install=False tar gzip gcc glibc-devel zlib-devel curl && \
     ###
     ### Installation of GraalVM
	 mkdir -p ${GRAALVM_HOME} && \
	 cd ${GRAALVM_HOME} && \
	 curl -fsSL $GRAAL_CE_URL | tar -xzC ${GRAALVM_HOME} --strip-components=1  && \
     ### Cleanup     
     yum clean all && \
     rm -f /tmp/graalvm-ce-amd64.tar.gz && \
     rm -rf /var/cache/yum && \
     mkdir -p /project
     ###

ENV GRAALVM_HOME /opt/graalvm

RUN chown -R 1001:0 $HOME && \
    chmod -R ug+rwX $HOME

USER 1001
