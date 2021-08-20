FROM php:7.4.22-apache2-stretch

RUN apt-get update -yqq && \
apt-get -y update && \
  apt-get install -y zip unzip libjpeg62-turbo-dev libpng-dev && \
  apt-get install -y nano && \
  apt-get install -y libzip-dev && \
  a2enmod rewrite && \
  docker-php-ext-install mysqli pdo pdo_mysql exif && \
  docker-php-ext-configure zip && \
  docker-php-ext-install zip && \
  rm -rf /var/lib/apt/lists/*  

RUN docker-php-ext-configure gd  --with-jpeg && \
   docker-php-ext-install -j$(nproc) gd 
  # apk add jpeg-dev libpng-dev && \
  # docker-php-ext-configure gd --with-jpeg && \
  # docker-php-ext-install -j$(nproc) gd
# RUN apt-get update && apt-get install -y \ 
# libfreetype6-dev libjpeg62-turbo-dev \ 
# libgd-dev libpng12-dev
# RUN docker-php-ext-configure gd \ 
# --with-freetype-dir=/usr/include/ \ 
# --with-jpeg-dir=/usr/include/
# RUN docker-php-ext-install gd

# RUN apt-get install -y \
#     libfreetype6-dev \
#     libjpeg62-turbo-dev \
#     && docker-php-ext-configure gd --with-freetype --with-jpeg \
#     && docker-php-ext-install -j$(nproc) gd

RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer

COPY default.conf /etc/apache2/sites-enabled/000-default.conf

# RUN apt-get -y install sudo
# RUN useradd -m docker && echo "docker:docker" | chpasswd && adduser docker sudo

WORKDIR /var/www/app

# RUN php ./artisan key:generate
# COPY ./composer.* /var/www/app
# RUN ls

# COPY ./composer.json /var/www/app
# RUN echo pwd: `pwd` && echo ls: `ls`
# RUN composer install

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

EXPOSE 80
# EXPOSE 9000
# CMD ["php-fpm"]