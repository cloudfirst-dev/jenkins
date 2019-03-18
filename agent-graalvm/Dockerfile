FROM quay.io/cloudfirst/jenkins-agent-maven-35-centos7

MAINTAINER Joshua Mathianas <mathianasj@cloudfirst.dev>

ENV GRAALVM_VERSION=1.0.0-rc14

USER 0

# Install Maven
RUN curl -LOk https://github.com/oracle/graal/releases/download/vm-${GRAALVM_VERSION}/graalvm-ce-${GRAALVM_VERSION}-linux-amd64.tar.gz && \
    tar -xzf graalvm-ce-${GRAALVM_VERSION}-linux-amd64.tar.gz --directory=/opt && \
    rm -f graalvm-ce-${GRAALVM_VERSION}-linux-amd64.tar.gz && \
    ln -s /opt/graalvm-ce-${GRAALVM_VERSION} /opt/graalvm && \
    yum clean all -y && \
    mkdir -p $HOME/.m2 && \
    mkdir -p $HOME/.gradle

ENV GRAALVM_HOME /opt/graalvm

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001
