FROM jenkins/jenkins:lts-jdk11

USER root
RUN apt-get update 
RUN apt-get install -y ca-certificates curl gnupg lsb-release
RUN curl -fsSL https://get.docker.com -o get-docker.sh
RUN sh ./get-docker.sh

# RUN usermod -aG docker jenkins
# USER jenkins