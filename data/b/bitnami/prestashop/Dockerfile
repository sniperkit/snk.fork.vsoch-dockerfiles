FROM bitnami/minideb-extras:jessie-r67  
LABEL maintainer "Bitnami <containers@bitnami.com>"  
# Install required system packages and dependencies  
RUN install_packages libbz2-1.0 libc6 libcomerr2 libcurl3 libexpat1 libffi6
libfreetype6 libgcc1 libgcrypt20 libgmp10 libgnutls-deb0-28 libgpg-error0
libgssapi-krb5-2 libhogweed2 libicu52 libidn11 libjpeg62-turbo libk5crypto3
libkeyutils1 libkrb5-3 libkrb5support0 libldap-2.4-2 liblzma5 libmcrypt4
libncurses5 libnettle4 libp11-kit0 libpcre3 libpng12-0 libpq5 libreadline6
librtmp1 libsasl2-2 libsqlite3-0 libssh2-1 libssl1.0.0 libstdc++6 libsybdb5
libtasn1-6 libtidy-0.99-0 libtinfo5 libxml2 libxslt1.1 zlib1g  
RUN bitnami-pkg unpack apache-2.4.33-3 --checksum
55bc664737e2ff9d534468088424ee4b04e770e67800e317c9db5b1e58cf2ffe  
RUN bitnami-pkg unpack php-7.0.30-4 --checksum
e6c48cb951db7abeb83319bf06155385a5170a833e2ebfd1ee61e137633c0c7a  
RUN bitnami-pkg unpack mysql-client-10.1.33-0 --checksum
1b1608f1c1f4e21f05037225e095c49035ad8bfdb8b580abbb904c9f2448a5c6  
RUN bitnami-pkg install libphp-7.0.30-7 --checksum
ee53b007b7ba11b7202fff5c112d0b13fd78cabab8fabf13a94723475f325f2e  
RUN bitnami-pkg unpack prestashop-1.7.3-3-0 --checksum
3077422b5e847f07b36d7a5fdb79f9d37369541f51bc2a59aff469001cb25032  
  
COPY rootfs /  
ENV ALLOW_EMPTY_PASSWORD="no" \  
APACHE_HTTPS_PORT_NUMBER="443" \  
APACHE_HTTP_PORT_NUMBER="80" \  
BITNAMI_APP_NAME="prestashop" \  
BITNAMI_IMAGE_VERSION="1.7.3-3-r4" \  
MARIADB_HOST="mariadb" \  
MARIADB_PORT_NUMBER="3306" \  
MARIADB_ROOT_PASSWORD="" \  
MARIADB_ROOT_USER="root" \  
MYSQL_CLIENT_CREATE_DATABASE_NAME="" \  
MYSQL_CLIENT_CREATE_DATABASE_PASSWORD="" \  
MYSQL_CLIENT_CREATE_DATABASE_PRIVILEGES="ALL" \  
MYSQL_CLIENT_CREATE_DATABASE_USER="" \  
PATH="/opt/bitnami/apache/bin:/opt/bitnami/php/bin:/opt/bitnami/mysql/bin:/opt/bitnami/prestashop/bin:$PATH"
\  
PRESTASHOP_COOKIE_CHECK_IP="yes" \  
PRESTASHOP_DATABASE_NAME="bitnami_prestashop" \  
PRESTASHOP_DATABASE_PASSWORD="" \  
PRESTASHOP_DATABASE_USER="bn_prestashop" \  
PRESTASHOP_EMAIL="user@example.com" \  
PRESTASHOP_FIRST_NAME="Bitnami" \  
PRESTASHOP_HOST="" \  
PRESTASHOP_LAST_NAME="User" \  
PRESTASHOP_PASSWORD="bitnami" \  
SMTP_HOST="" \  
SMTP_PASSWORD="" \  
SMTP_PORT="" \  
SMTP_PROTOCOL="" \  
SMTP_USER=""  
  
EXPOSE 80 443  
  
ENTRYPOINT ["/app-entrypoint.sh"]  
CMD ["nami","start","--foreground","apache"]  

