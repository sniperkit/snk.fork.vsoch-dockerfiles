FROM bitnami/minideb-extras:jessie-r68  
LABEL maintainer "Bitnami <containers@bitnami.com>"  
# Install required system packages and dependencies  
RUN install_packages libbz2-1.0 libc6 libcomerr2 libcurl3 libexpat1 libffi6
libfreetype6 libgcc1 libgcrypt20 libgmp10 libgnutls-deb0-28 libgpg-error0
libgssapi-krb5-2 libhogweed2 libicu52 libidn11 libjpeg62-turbo libk5crypto3
libkeyutils1 libkrb5-3 libkrb5support0 libldap-2.4-2 liblzma5 libmcrypt4
libncurses5 libnettle4 libp11-kit0 libpcre3 libpng12-0 libpq5 libreadline6
librtmp1 libsasl2-2 libsqlite3-0 libssh2-1 libssl1.0.0 libstdc++6 libsybdb5
libtasn1-6 libtidy-0.99-0 libtinfo5 libxml2 libxslt1.1 zlib1g  
RUN bitnami-pkg unpack apache-2.4.33-4 --checksum
f26751a96abdc45db43c4b92eb9f792a4b630ad0edd5667aa73ccafa0aeb3810  
RUN bitnami-pkg unpack php-7.0.30-4 --checksum
e6c48cb951db7abeb83319bf06155385a5170a833e2ebfd1ee61e137633c0c7a  
RUN bitnami-pkg unpack mysql-client-10.1.33-0 --checksum
1b1608f1c1f4e21f05037225e095c49035ad8bfdb8b580abbb904c9f2448a5c6  
RUN bitnami-pkg install libphp-7.0.30-7 --checksum
ee53b007b7ba11b7202fff5c112d0b13fd78cabab8fabf13a94723475f325f2e  
RUN bitnami-pkg unpack wordpress-4.9.6-4 --checksum
8f3eb9b8c293cdd760bd4a836e338f78821a2feb537f4bff25f5073876a87a8e  
  
COPY rootfs /  
ENV ALLOW_EMPTY_PASSWORD="no" \  
APACHE_HTTPS_PORT_NUMBER="443" \  
APACHE_HTTP_PORT_NUMBER="80" \  
BITNAMI_APP_NAME="wordpress" \  
BITNAMI_IMAGE_VERSION="4.9.6-r13" \  
MARIADB_HOST="mariadb" \  
MARIADB_PORT_NUMBER="3306" \  
MARIADB_ROOT_PASSWORD="" \  
MARIADB_ROOT_USER="root" \  
MYSQL_CLIENT_CREATE_DATABASE_NAME="" \  
MYSQL_CLIENT_CREATE_DATABASE_PASSWORD="" \  
MYSQL_CLIENT_CREATE_DATABASE_PRIVILEGES="ALL" \  
MYSQL_CLIENT_CREATE_DATABASE_USER="" \  
PATH="/opt/bitnami/apache/bin:/opt/bitnami/php/bin:/opt/bitnami/mysql/bin:$PATH"
\  
SMTP_HOST="" \  
SMTP_PASSWORD="" \  
SMTP_PORT="" \  
SMTP_PROTOCOL="" \  
SMTP_USER="" \  
SMTP_USERNAME="" \  
WORDPRESS_BLOG_NAME="User's Blog!" \  
WORDPRESS_DATABASE_NAME="bitnami_wordpress" \  
WORDPRESS_DATABASE_PASSWORD="" \  
WORDPRESS_DATABASE_USER="bn_wordpress" \  
WORDPRESS_EMAIL="user@example.com" \  
WORDPRESS_FIRST_NAME="FirstName" \  
WORDPRESS_HOST="" \  
WORDPRESS_HTTPS_PORT="443" \  
WORDPRESS_HTTP_PORT="80" \  
WORDPRESS_LAST_NAME="LastName" \  
WORDPRESS_PASSWORD="bitnami" \  
WORDPRESS_TABLE_PREFIX="wp_" \  
WORDPRESS_USERNAME="user"  
  
EXPOSE 80 443  
  
ENTRYPOINT ["/app-entrypoint.sh"]  
CMD ["nami","start","--foreground","apache"]  

