#
#  Jenkins master slave
#
FROM sktmt/centos6-jst
MAINTAINER sokutou-metsu sktmt

ENV LANG en_US.UTF-8

# Installs core modules
RUN yum -y install curl sudo wget which &&\
    yum -y clean all

# Installs Open JDK 7
RUN yum -y install java-1.7.0-openjdk &&\
    yum -y clean all

# Installs Jenkins
RUN groupadd -g 1001 jenkins &&\
    useradd -c "Jenkins Continuous Build server" \
        -u 1001 -g 1001 -d /var/lib/jenkins -s /bin/bash jenkins
RUN wget -nv -T 60 -t 3 -O /etc/yum.repos.d/jenkins.repo \
        http://pkg.jenkins-ci.org/redhat/jenkins.repo &&\
    rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key &&\
    yum -y install jenkins &&\
    yum -y clean all

# Installs Jenkins Plugins
RUN wget -nv -T 60 -t 3 -P /var/lib/jenkins/plugins \
        https://updates.jenkins-ci.org/latest/scm-api.hpi &&\
    wget -nv -T 60 -t 3 -P /var/lib/jenkins/plugins \
        https://updates.jenkins-ci.org/latest/git-client.hpi &&\
    wget -nv -T 60 -t 3 -P /var/lib/jenkins/plugins \
        https://updates.jenkins-ci.org/latest/git.hpi &&\
    wget -nv -T 60 -t 3 -P /var/lib/jenkins/plugins \
        https://updates.jenkins-ci.org/latest/ansicolor.hpi &&\
    wget -nv -T 60 -t 3 -P /var/lib/jenkins/plugins \
        https://updates.jenkins-ci.org/latest/buildtriggerbadge.hpi &&\
    chown -R jenkins:jenkins /var/lib/jenkins/plugins

# Installs Subversion
RUN yum -y install subversion &&\
    yum -y clean all

# Installs Git
RUN yum -y install git &&\
    yum -y clean all

# Executes command to start Jenkins
EXPOSE 8080
USER jenkins
WORKDIR /var/lib/jenkins
ENTRYPOINT [ \
    "/etc/alternatives/java", \
    "-Djava.awt.headless=true", \
    "-DJENKINS_HOME=/var/lib/jenkins", \
    "-jar", "/usr/lib/jenkins/jenkins.war", \
    "--logfile=/var/log/jenkins/jenkins.log", \
    "--webroot=/var/cache/jenkins/war", \
    "--httpPort=8080", \
    "--ajp13Port=8009", \
    "--debug=5", \
    "--handlerCountMax=100", \
    "--handlerCountMaxIdle=20"]
