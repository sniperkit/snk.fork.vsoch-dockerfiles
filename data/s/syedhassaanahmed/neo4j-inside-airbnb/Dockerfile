FROM neo4j  
LABEL maintainer="Syed Hassaan Ahmed"  
  
ENV FILE_LIMIT_MB=7  
ADD
https://gist.githubusercontent.com/jexp/1e295d5f5b96e8e42fb614232abdbb4f/raw/ee277d2daec8aa54c031cbeede774abda9c9118b/neo4j-wait.sh
./  
ADD
https://raw.githubusercontent.com/johnymontana/neo4j-datasets/master/airbnb/src/import/import.cypher
./  
  
COPY import.sh ./  
  
RUN chmod +x *.sh && apk add --update grep curl && rm -rf /var/cache/apk/*  
  
CMD ["./import.sh"]

