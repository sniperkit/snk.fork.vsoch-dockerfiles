FROM gliderlabs/alpine:edge  
MAINTAINER jussi.nummelin@gmail.com  
  
RUN apk update && apk --update add ruby ruby-dev ruby-rdoc sqlite-dev ca-
certificates libssl1.0 openssl libstdc++  
  
ADD Gemfile /app/  
ADD Gemfile.lock /app/  
  
RUN apk --update add --virtual build-dependencies build-base ruby-irb openssl-
dev && \  
gem install bundler io-console && \  
cd /app ; bundle install --without development test && \  
apk del build-dependencies  
  
WORKDIR /app  
ADD . /app  
  
ENV PORT 3000  
CMD ["./run.sh"]  

