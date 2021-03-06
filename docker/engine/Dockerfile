FROM php:7.0.6-fpm
MAINTAINER Etienne Dauvergne <contact@ekyna.com>

# Update apt-get
RUN apt-get -qq update

# Install
RUN apt-get -y --force-yes -qq install -y \
    git \
    wget \
    jpegoptim \
    wkhtmltopdf \
    libicu-dev \
    zlib1g-dev \
    libfreetype6-dev \
    libgd-dev \
    libmcrypt-dev

# PHP extensions
RUN curl -L -o /tmp/apcu-5.1.3.tgz https://pecl.php.net/get/apcu-5.1.3.tgz \
    && tar xfz /tmp/apcu-5.1.3.tgz \
    && mv apcu-5.1.3 /usr/src/php/ext/apcu \
    && docker-php-ext-configure gd --with-freetype-dir=/usr \
    && docker-php-ext-install \
        apcu \
        opcache \
        intl \
        mbstring \
        mysqli \
        pdo \
        pdo_mysql \
        zip \
        ftp \
        gd \
        mcrypt \
        json \
        exif

# XDebug
RUN curl -L -o /tmp/xdebug-2.4.0.tgz http://xdebug.org/files/xdebug-2.4.0.tgz \
    && tar xfz /tmp/xdebug-2.4.0.tgz \
    && mv xdebug-2.4.0 /usr/src/php/ext/xdebug \
    && docker-php-ext-install xdebug

# Composer
RUN php -r "readfile('https://getcomposer.org/installer');" | php -- --install-dir=/usr/local/bin --filename=composer \
    && chmod +x /usr/local/bin/composer

# PHPUnit
RUN wget https://phar.phpunit.de/phpunit.phar \
    && mv phpunit.phar /usr/local/bin/phpunit \
    && chmod +x /usr/local/bin/phpunit

# Cleanup
RUN apt-get autoremove -y && apt-get clean all

# Permissions
RUN usermod -u 1000 www-data

# Expose xdebug port
EXPOSE 9090


# SSH demon for phpstom debug features
RUN apt-get update && apt-get install -y openssh-server \
    && mkdir /var/run/sshd \
    && echo 'root:root' | chpasswd \
    && sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config
#    && sed -i 's/#   Port 22/Port 9080/' /etc/ssh/ssh_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

# Expoe SSH port
EXPOSE 22


# Supervisor install
RUN apt-get update && apt-get install -y supervisor && mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["/usr/bin/supervisord"]