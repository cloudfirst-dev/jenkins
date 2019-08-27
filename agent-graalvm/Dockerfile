FROM openshift/jenkins-slave-base-centos7

MAINTAINER Joshua Mathianas <mathianasj@cloudfirst.dev>

ARG GRAAL_VERSION=19.2.0

ENV MAVEN_VERSION=3.5.4 \
    GRADLE_VERSION=4.2.1 \
    BASH_ENV=/usr/local/bin/scl_enable \
    ENV=/usr/local/bin/scl_enable \
    PROMPT_COMMAND=". /usr/local/bin/scl_enable" \
    PATH=$PATH:/opt/gradle/bin \
    MAVEN_HOME=/usr/share/maven


ENV GRAAL_CE_URL=https://github.com/oracle/graal/releases/download/vm-${GRAAL_VERSION}/graalvm-ce-linux-amd64-${GRAAL_VERSION}.tar.gz
ENV GRAALVM_HOME /opt/graalvm
ENV JAVA_HOME /opt/graalvm
ENV MAVEN_OPTS="-XX:+TieredCompilation -XX:TieredStopAtLevel=1"

ENV PATH=$PATH:$JAVA_HOME/bin

# Install Maven
RUN INSTALL_PKGS="java-1.8.0-openjdk-devel.x86_64" && \
    x86_EXTRA_RPMS=$(if [ "$(uname -m)" == "x86_64" ]; then echo -n java-1.8.0-openjdk-devel.i686 ; fi) && \
    yum install -y centos-release-scl-rh && \
    yum install -y --setopt=protected_multilib=false --enablerepo=centosplus $INSTALL_PKGS $x86_EXTRA_RPMS && \
    curl -LOk https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip && \
    unzip gradle-${GRADLE_VERSION}-bin.zip -d /opt && \
    rm -f gradle-${GRADLE_VERSION}-bin.zip && \
    ln -s /opt/gradle-${GRADLE_VERSION} /opt/gradle && \
    rpm -V java-1.8.0-openjdk-devel.x86_64 $x86_EXTRA_RPMS && \
    yum clean all -y && \
    curl -fsSL https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar xzf - -C /usr/share && \
    mv /usr/share/apache-maven-$MAVEN_VERSION /usr/share/maven && \
    ln -s /usr/share/maven/bin/mvn /usr/bin/mvn && \
    mkdir -p $HOME/.m2 && \
    mkdir -p $HOME/.gradle

RUN  yum update -y && \
	 yum install -y --setopt=skip_missing_names_on_install=False tar gzip gcc glibc-devel zlib-devel curl && \
     ###
     ### Installation of GraalVM
	 mkdir -p ${GRAALVM_HOME} && \
	 cd ${GRAALVM_HOME} && \
	 curl -fsSL $GRAAL_CE_URL | tar -xzC ${GRAALVM_HOME} --strip-components=1  && \
         gu install native-image && \
     ### Cleanup     
     yum clean all && \
     rm -f /tmp/graalvm-ce-amd64.tar.gz && \
     rm -rf /var/cache/yum && \
     mkdir -p /project
     ###

ENV GRAALVM_HOME /opt/graalvm

# When bash is started non-interactively, to run a shell script, for example it
# looks for this variable and source the content of this file. This will enable
# the SCL for all scripts without need to do 'scl enable'.
ADD contrib/bin/scl_enable /usr/local/bin/scl_enable
ADD contrib/bin/configure-slave /usr/local/bin/configure-slave
ADD ./contrib/settings.xml $HOME/.m2/
ADD ./contrib/init.gradle $HOME/.gradle/
