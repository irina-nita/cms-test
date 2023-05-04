# syntax=docker/dockerfile:1
FROM --platform=linux/amd64 ubuntu:20.04

RUN apt-get update
RUN apt-get install -y \
    build-essential \
    cgroup-lite \
    cppreference-doc-en-html \
    fp-compiler \
    git \
    haskell-platform \
    libcap-dev \
    libcups2-dev \
    libffi-dev \
    libpq-dev \
    libyaml-dev \
    mono-mcs \
    openjdk-8-jdk-headless \
    php7.4-cli \
    postgresql-client \
    python2 \
    python3-pip \
    python3.8 \
    python3.8-dev \
    rustc \
    sudo \
    wait-for-it \
    zip

# Create cmsuser user with sudo privileges
RUN useradd -ms /bin/bash cmsuser && \
    usermod -aG sudo cmsuser
# Disable sudo password
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
# Set cmsuser as default user
USER cmsuser

COPY --chown=cmsuser:cmsuser . /home/cmsuser/cms

WORKDIR /home/cmsuser/cms

RUN sudo pip3 install -r requirements.txt
RUN sudo pip3 install -r dev-requirements.txt
RUN sudo python3 setup.py install

RUN sudo python3 prerequisites.py --yes --cmsuser=cmsuser install; exit 0

RUN sudo chmod +s /usr/local/bin/isolate
RUN sudo chown root:cmsuser ./isolate/isolate
RUN sudo chmod u+s ./isolate/isolate

RUN sudo apt install tmux -y
RUN sudo apt install vim -y
RUN sudo apt install systemctl -y
RUN sudo apt install nginx -y
RUN sudo apt-get install libcgroup-dev -y

COPY ./config/cms.ranking.conf.sample /usr/local/etc/cms.ranking.conf
COPY ./cmdline.txt /boot/firmware/cmdline.txt
COPY ./config/cms.conf.docker.sample /usr/local/etc/cms.conf
COPY ./config/nginx.conf.sample /usr/local/etc/nginx.conf
COPY ./isolate-check-environment /home/cmsuser/cms/isolate-check-environment

RUN sudo chmod 777 /home/cmsuser/cms/isolate-check-environment
# RUN sudo chmod 777 /etc/nginx/nginx.conf
# RUN sudo cat /usr/local/etc/nginx.conf >> /etc/nginx/nginx.conf

ENV LANG C.UTF-8

CMD [""]
