FROM python:3.6-alpine3.7  
ENV PYTHONUNBUFFERED=1  
RUN mkdir /app  
WORKDIR /app  
  
COPY *requirements.txt /app/  
RUN apk add --no-cache --virtual=.run-deps git &&\  
pip install -r requirements.txt -r test-requirements.txt  
  
COPY . /app  
  
RUN python setup.py install  
  
CMD ["crane"]  
LABEL name=crane version=dev  

