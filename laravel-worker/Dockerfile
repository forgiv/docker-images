FROM ubuntu:20.04
# Install PHP tooling
RUN apt-get update \
    && apt-get install -y software-properties-common \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y \
    	curl \
    	python3 \
    	python3-pip \
    	php7.4 \
    	php7.4-fpm \
    	php7.4-curl \
    	php7.4-mbstring \
    	php7.4-gd \
    	php7.4-imagick \
    	php7.4-xml \
    	php7.4-mysql \
    	php7.4-redis \
    	php7.4-memcached \
    	php7.4-mongodb
# Setup NodeJS
RUN apt-get install npm -y \
    && npm i -g n \
    && n 12
# Install supervisor
RUN pip3 install supervisor
# Install sudo
RUN apt-get install sudo
# Create new user `docker` and disable 
# password and gecos for later
# --gecos explained well here:
# https://askubuntu.com/a/1195288/635348
RUN adduser --disabled-password --gecos '' docker
#  Add new user docker to sudo group
RUN adduser docker sudo
# Ensure sudo group users are not 
# asked for a password when using 
# sudo command by ammending sudoers file
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
# now we can set USER to the 
# user we just created
USER docker
# Run supervisor
CMD sudo supervisord && sudo tail -f /tmp/supervisord.log
