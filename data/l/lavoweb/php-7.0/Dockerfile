FROM lavoweb/php-7.0:magento2  
MAINTAINER Aurélien Lavorel <aurelien@lavoweb.net>  
  
RUN pecl install xdebug  
RUN docker-php-ext-enable xdebug  
RUN echo 'xdebug.remote_enable = On' >> /usr/local/etc/php/conf.d/docker-php-
ext-xdebug.ini && \  
echo 'xdebug.remote_autostart= On' >> /usr/local/etc/php/conf.d/docker-php-
ext-xdebug.ini && \  
echo 'xdebug.remote_connect_back = On' >> /usr/local/etc/php/conf.d/docker-
php-ext-xdebug.ini  
RUN rm -f /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini  

