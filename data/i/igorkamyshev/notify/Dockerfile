FROM keymetrics/pm2:latest  
  
RUN echo -e 'http://dl-cdn.alpinelinux.org/alpine/edge/main\nhttp://dl-
cdn.alpinelinux.org/alpine/edge/community\nhttp://dl-
cdn.alpinelinux.org/alpine/edge/testing' > /etc/apk/repositories  
RUN apk add --no-cache yarn  
  
WORKDIR /usr/src/app  
COPY package.json ./  
COPY yarn.lock ./  
RUN yarn install  
COPY . .  
  
WORKDIR /usr/src/app/landing  
RUN yarn install  
RUN yarn build  
RUN yarn export  
  
WORKDIR /usr/src/app  
  
EXPOSE 8080  
CMD [ "pm2-docker", "start", "pm2.json" ]

