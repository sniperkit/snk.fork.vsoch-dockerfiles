FROM ubuntu:16.04  
# Install required packages  
RUN echo 'mysql-server mysql-server/root_password password rootpassword' |
debconf-set-selections  
RUN echo 'mysql-server mysql-server/root_password_again password rootpassword'
| debconf-set-selections  
RUN apt-get update && apt-get --assume-yes install \  
apache2 \  
git \  
php \  
curl \  
php-dev \  
php-curl \  
libapache2-mod-php \  
zip \  
nodejs \  
npm \  
imagemagick \  
mysql-server \  
php7.0-mysql  
  
# Install coffee-script  
RUN ln -s /usr/bin/nodejs /usr/bin/node  
RUN npm install -g coffee-script  
  
# Prepare apache  
RUN rm -rf /var/www/html  
  
# Make terminal functional by default  
RUN echo "export TERM=xterm" >>/root/.bashrc  
  
# Install biom via conda  
RUN curl -sS https://repo.continuum.io/miniconda/Miniconda3-latest-
Linux-x86_64.sh >miniconda.sh  
RUN chmod +x miniconda.sh  
RUN ./miniconda.sh -b -f -p /usr/local  
RUN echo "export LC_ALL=C.UTF-8" >>/etc/apache2/envvars  
RUN echo "export LANG=C.UTF-8" >>/etc/apache2/envvars  
ADD explicit-conda-spec.txt /tmp/explicit-conda-spec.txt  
RUN cd /tmp && conda install --yes --file explicit-conda-spec.txt  
  
# Clone git repo  
RUN git clone -b iimog_master https://github.com/molbiodiv/Phinch.git
/var/www/html  
RUN cd /var/www/html && coffee -o scripts -c src  
RUN chown -R www-data:www-data /var/www/html/biomFiles  
  
# Add custom php ini  
ADD php.ini /etc/php/7.0/apache2/php.ini  
# Add custom apache conf  
ADD apache2.conf /etc/apache2/apache2.conf  
ADD .htaccess /var/www/html/.htaccess  
  
# Create and fill database  
ADD create_mysql_db.sql /tmp/create_mysql_db.sql  
ADD dbconfig.php /var/www/html/server/dbconfig.php  
  
CMD service mysql start; mysql --password=rootpassword
</tmp/create_mysql_db.sql; mysql --user=phinch --password=phinch phinch
</var/www/html/data/dbschema.sql; service apache2 start; while true; do sleep
60; done  

