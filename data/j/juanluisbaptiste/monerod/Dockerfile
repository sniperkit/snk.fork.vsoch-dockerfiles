FROM juanluisbaptiste/monero-wallet:latest  
COPY start_monerod.sh /  
USER root  
RUN apt-get update && \  
apt-get install -y supervisor && \  
chown monero:monero /start_monerod.sh && \  
chmod 755 /start_monerod.sh  
#Supervisord configuration  
COPY etc/supervisor/monerod.conf /etc/supervisor/conf.d/  
# blockchain loaction  
VOLUME /home/monero/.bitmonero  
EXPOSE 18080  
EXPOSE 18081  
ENTRYPOINT ["/start_monerod.sh"]  

