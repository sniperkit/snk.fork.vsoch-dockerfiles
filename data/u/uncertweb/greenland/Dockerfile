FROM node  
  
RUN mkdir -p /usr/src/app  
WORKDIR /usr/src/app  
  
COPY package.json /usr/src/app/  
RUN npm --quiet install  
  
COPY . /usr/src/app  
  
RUN npm run gulp build  
  
EXPOSE 8080  
CMD [ "./node_modules/http-server/bin/http-server", "--cors", "-p", "8080",
"dist"]

