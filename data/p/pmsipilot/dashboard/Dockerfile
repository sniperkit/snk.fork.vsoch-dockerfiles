FROM node:alpine  
  
# Create app directory  
WORKDIR /usr/src/app  
  
# Install app dependencies  
# A wildcard is used to ensure both package.json AND package-lock.json are
copied  
# where available (npm@5+)  
COPY package*.json ./  
  
COPY gulpfile.babel.js ./  
  
RUN npm install  
# If you are building your code for production  
# RUN npm install --only=production  
# Bundle app source  
COPY . .  
  
RUN npm run build  
  
EXPOSE 3000  
CMD [ "npm", "start" ]

