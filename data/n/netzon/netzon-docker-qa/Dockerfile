FROM netzon/netzon-docker:latest  
COPY requirements .  
RUN pip install -r requirements && \  
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key
add \- && \  
echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >
/etc/apt/sources.list.d/google-chrome.list && \  
apt-get update && apt-get -y install google-chrome-stable  

