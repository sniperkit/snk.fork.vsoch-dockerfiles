FROM phusion/baseimage:0.9.17  
MAINTAINER DoITMagic <razvan@doitmagic.com>  
  
# Default baseimage settings  
ENV HOME /root  
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh  
CMD ["/sbin/my_init"]  
ENV DEBIAN_FRONTEND noninteractive  
  
# Update software list and install php + nginx  
RUN apt-get update \  
&& apt-get install -y --force-yes \  
nginx \  
php5 \  
php5-fpm \  
php5-cli \  
php5-mysql \  
php5-mcrypt \  
php5-curl \  
php5-gd \  
php5-intl \  
php5-mysql \  
libapache2-mod-php5  
  
# Install the composer  
RUN curl -sS https://getcomposer.org/installer | php -- --install-
dir=/usr/bin/ --filename=composer  
  
# Install the cron service  
RUN apt-get install cron -y  
  
# Clear cache  
RUN apt-get clean \  
&& rm -rf /var/lib/apt/lists/* \  
/tmp/* \  
/var/tmp/*  
  
# Configure nginx  
RUN echo "daemon off;" >> /etc/nginx/nginx.conf  
RUN sed -i "s/sendfile on/sendfile off/" /etc/nginx/nginx.conf  
RUN mkdir -p /var/www  
RUN mkdir -p /var/cron  
  
RUN mkdir -p /etc/ssl/certs/  
RUN mkdir -p /etc/ssl/private/  
  
COPY ssl/nginx.crt /etc/ssl/certs/  
COPY ssl/nginx.key /etc/ssl/private/  
ADD crons.conf /var/cron/crons.conf  
  
# Use the crontab file.  
RUN crontab /var/cron/crons.conf  
  
# Configure PHP  
RUN sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php5/fpm/php.ini  
RUN sed -i "s/;date.timezone =.*/date.timezone = Asia\/Beirut/"
/etc/php5/fpm/php.ini  
RUN sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php5/fpm/php-
fpm.conf  
RUN sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php5/cli/php.ini  
RUN sed -i "s/;date.timezone =.*/date.timezone = Asia\/Beirut/"
/etc/php5/cli/php.ini  
RUN php5enmod mcrypt  
  
# Add nginx volumes  
VOLUME ["/var/www","/var/cron","/etc/nginx/sites-available",
"/etc/nginx/sites-enabled"]  
  
# Add nginx service  
RUN mkdir /etc/service/nginx  
ADD build/nginx/run.sh /etc/service/nginx/run  
RUN chmod +x /etc/service/nginx/run  
  
# Add PHP service  
RUN mkdir /etc/service/phpfpm  
ADD build/php/run.sh /etc/service/phpfpm/run  
RUN chmod +x /etc/service/phpfpm/run  
  
# Set the work directory  
WORKDIR /var/www  
  
# Expose ports  
EXPOSE 80 443 8000  

