FROM  ubuntu:20.04 as builder 
################################################
# Gurdbox setup
################################################
ENV TZ=Asia/Jerusalem
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# Make sure the package repository is up to date.
RUN apt-get update && \
    apt-get -qy upgrade && \
    apt-get install -qy git && \
# Install JDK 8 (latest stable edition at 2019-04-01)
    apt-get install -qy openjdk-8-jdk && \
# Autoremove
    apt-get -qy autoremove 
#
RUN apt-get install -y iputils-ping patchelf git default-jdk plantuml doxygen graphviz xsltproc python3-pip curl vim cifs-utils nfs-common

RUN pip3 install requests==2.22.0 && \
    pip3 install Atlassian-python-API && \
    git config --global http.sslVerify false
RUN mkdir -p /home/ciuser/sdk && \
    mkdir -p /home/ciuser/.jfrog && \
    mkdir -p /home/ciuser/.ssh
