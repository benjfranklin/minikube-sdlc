
FROM ubuntu:22.04

ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

RUN apt-add-repository ppa:ansible/ansible; \
    apt update -y; \
    apt install -y ansible

RUN apt install -y git; \
    git config --global user.name "Jenkins"; \
    git config --global user.email "jenkins@hill.org"
