FROM ubuntu:20.04
# Install PHP tooling
RUN apt-get update \
    && apt-get install -y software-properties-common \
    && add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y \
        curl \
        zip \
        unzip \
        php8.0 \
        php8.0-fpm \
        php8.0-curl \ 
        php8.0-mbstring\
        php8.0-gd \ 
        php8.0-imagick \
        php8.0-xml \ 
        php8.0-mysql \
        php8.0-redis \ 
        php8.0-memcached \
        php8.0-zip \
        php8.0-mongodb
# Copy PHP-fpm pool config into container
RUN mkdir -p /var/www
RUN rm /etc/php/8.0/fpm/pool.d/www.conf
COPY ./www.conf /etc/php/8.0/fpm/pool.d/www.conf
# Setup Composer
RUN cd /tmp \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer --2
# Setup NodeJS
RUN apt-get install npm -y \
    && npm i -g n \
    && n 14
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
# Set /var/www as the base directory
WORKDIR /var/www
# Start PHP-fpm & Watch log
CMD sudo service php8.0-fpm start && sudo tail -F /var/log/php8.0-fpm.log
