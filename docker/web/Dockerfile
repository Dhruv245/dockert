FROM php:7.4.22-apache

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

RUN apt-get -y update && \ 
apt-get install -y libfreetype6-dev

RUN docker-php-ext-configure gd --with-freetype --with-jpeg && \
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



# RUN php ./artisan key:generate
# COPY ./composer.* /var/www/app
# RUN ls

# COPY ./composer.json /var/www/app
# RUN echo pwd: `pwd` && echo ls: `ls`
# RUN composer install
RUN docker-php-ext-install bcmath
RUN apt-get -y update \
&& apt-get install -y libicu-dev \
&& docker-php-ext-configure intl \
&& docker-php-ext-install intl


COPY ssl.key /etc/apache2/ssl.key
COPY ssl.crt /etc/apache2/ssl.crt
RUN a2enmod ssl

ENV TZ=Asia/Kolkata
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt install libgmp-dev -y && docker-php-ext-install gmp

RUN curl https://deb.nodesource.com/setup_12.x | bash
RUN curl https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update && apt-get install -y nodejs yarn

ENV PHP_MEMORY_LIMIT=128M


RUN a2enmod proxy && \
 a2enmod proxy_http && \
 a2enmod proxy_balancer && \
 a2enmod lbmethod_byrequests
#########

COPY default.conf /etc/apache2/sites-enabled/000-default.conf

WORKDIR /var/www/app

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

EXPOSE 80
# EXPOSE 9000
# CMD ["php-fpm"]