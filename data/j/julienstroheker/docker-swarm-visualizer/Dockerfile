#Latest version of node tested on.  
FROM node:4.1.2-slim  
  
WORKDIR /app  
  
# Only run npm install if these files change.  
ADD ./package.json /app/package.json  
  
# Install dependencies  
RUN npm install --unsafe-perm=true  
RUN npm install -g nodemon  
  
# Add the rest of the sources  
ADD . /app  
  
# Build the app  
RUN npm run dist  
  
# Default host is localhost. This is for same-origin policies.  
ENV HOST "localhost"  
# Number of milliseconds between polling requests. Default is 200.  
ENV MS 200  
#Default port to expose.  
ENV PORT 8080  
EXPOSE 8080  
CMD ["nodemon","--debug","server.js"]  

