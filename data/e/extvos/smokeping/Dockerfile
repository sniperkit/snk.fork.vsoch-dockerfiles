FROM debian:jessie  
MAINTAINER David Personette <dperson@dperson.com>  
  
# Install lighttpd and smokeping  
RUN export DEBIAN_FRONTEND='noninteractive' && \  
apt-get update -qq && \  
apt-get install -qqy --no-install-recommends smokeping ssmtp dnsutils \  
fonts-dejavu-core echoping curl lighttpd \  
$(apt-get -s dist-upgrade|awk '/^Inst.*ecurity/ {print $2}') &&\  
apt-get clean && \  
/bin/echo -e '+ EchoPingHttp\n\nbinary = /usr/bin/echoping\n' >> \  
/etc/smokeping/config.d/Probes && \  
/bin/echo -e '+ EchoPingHttps\n\nbinary = /usr/bin/echoping\n' >> \  
/etc/smokeping/config.d/Probes && \  
sed -i '/server.errorlog/s|^|#|' /etc/lighttpd/lighttpd.conf && \  
sed -i '/server.document-root/s|/html||' /etc/lighttpd/lighttpd.conf && \  
/bin/echo -e '\n# redirect to the right Smokeping URI' >> \  
/etc/lighttpd/lighttpd.conf && \  
/bin/echo 'url.redirect = ("^/$" => "/smokeping/smokeping.cgi",' >> \  
/etc/lighttpd/lighttpd.conf && \  
/bin/echo -e '\t\t\t"^/smokeping/?$" => "/smokeping/smokeping.cgi")' >> \  
/etc/lighttpd/lighttpd.conf && \  
sed -i '/^#cgi\.assign/,$s/^#//; /"\.pl"/i \ \t".cgi" => "/usr/bin/perl",'\  
/etc/lighttpd/conf-available/10-cgi.conf && \  
sed -i -e '/CHILDREN/s/[0-9][0-9]*/16/' \  
-e '/max-procs/a \ \t\t"idle-timeout" => 20,' \  
/etc/lighttpd/conf-available/15-fastcgi-php.conf && \  
grep -q 'allow-x-send-file' \  
/etc/lighttpd/conf-available/15-fastcgi-php.conf || { \  
sed -i '/idle-timeout/a \ \t\t"allow-x-send-file" => "enable",' \  
/etc/lighttpd/conf-available/15-fastcgi-php.conf && \  
sed -i '/"bin-environment"/a \ \t\t\t"MOD_X_SENDFILE2_ENABLED" => "1",'\  
/etc/lighttpd/conf-available/15-fastcgi-php.conf; } && \  
sed -i 's|/usr/bin/smokeping_cgi|/usr/lib/cgi-bin/smokeping.cgi|' \  
/usr/share/smokeping/www/smokeping.fcgi.dist && \  
mv /usr/share/smokeping/www/smokeping.fcgi.dist \  
/usr/share/smokeping/www/smokeping.fcgi && \  
lighttpd-enable-mod cgi && \  
lighttpd-enable-mod fastcgi && \  
[ -d /var/cache/smokeping ] || mkdir -p /var/cache/smokeping && \  
[ -d /var/lib/smokeping ] || mkdir -p /var/lib/smokeping && \  
[ -d /var/run/smokeping ] || mkdir -p /var/run/smokeping && \  
chown -Rh smokeping:www-data /var/cache/smokeping /var/lib/smokeping \  
/var/run/smokeping && \  
chmod -R g+ws /var/cache/smokeping /var/lib/smokeping /var/run/smokeping &&\  
rm -rf /var/lib/apt/lists/* /tmp/* && \  
ln -s /usr/share/smokeping/www /var/www/smokeping && \  
ln -s /usr/lib/cgi-bin /var/www/ && \  
ln -s /usr/lib/cgi-bin/smokeping.cgi /var/www/smokeping/  
COPY smokeping.sh /usr/bin/  
  
VOLUME ["/etc/smokeping", "/etc/ssmtp", "/var/cache/smokeping", \  
"/var/lib/smokeping", "/var/run/smokeping"]  
  
EXPOSE 80  
  
ENTRYPOINT ["smokeping.sh"]  

