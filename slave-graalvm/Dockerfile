FROM quay.io/cloudfirst/jenkins-agent-maven-35-centos7

MAINTAINER Joshua Mathianas <mathianasj@cloudfirst.dev>

ENV GRAALVM_VERSION=1.0.0-rc14

# Install Maven
RUN tar --ussage && \
    curl -LOk https://github.com/oracle/graal/releases/download/vm-${GRAALVM_VERSION}/graalvm-ce-${GRAALVM_VERSION}-linux-amd64.tar.gz && \
    tar -xzf graalvm-ce-${GRAALVM_VERSION}-linux-amd64.tar.gz --directory /opt/ && \
    rm -f graalvm-ce-${GRAALVM_VERSION}-linux-amd64.tar.gz && \
    ln -s /opt/graalvm-ce-${GRAALVM_VERSION}-linux-amd64 /opt/graalvm && \
    yum clean all -y && \
    mkdir -p $HOME/.m2 && \
    mkdir -p $HOME/.gradle

# When bash is started non-interactively, to run a shell script, for example it
# looks for this variable and source the content of this file. This will enable
# the SCL for all scripts without need to do 'scl enable'.
ADD contrib/bin/scl_enable /usr/local/bin/scl_enable
ADD contrib/bin/configure-slave /usr/local/bin/configure-slave
ADD ./contrib/settings.xml $HOME/.m2/
ADD ./contrib/init.gradle $HOME/.gradle/

ENV GRAALVM_HOME /opt/graalvm

RUN chown -R 1001:0 $HOME && \
    chmod -R g+rw $HOME

USER 1001
