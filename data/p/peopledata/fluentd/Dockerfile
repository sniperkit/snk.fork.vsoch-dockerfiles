FROM fluent/fluentd:latest  
WORKDIR /home/fluent  
ENV PATH /home/fluent/.gem/ruby/2.3.0/bin:$PATH  
  
RUN apk --no-cache --update add build-base ruby-dev libffi-dev && \  
gem install fluent-plugin-record-reformer fluent-plugin-
kubernetes_metadata_filter && \  
rm -rf /home/fluent/.gem/ruby/2.3.0/cache/*.gem && \  
gem sources -c && \  
apk del build-base ruby-dev && rm -rf /var/cache/apk/*  
  
RUN mkdir -p /mnt/pos  
EXPOSE 24284  
RUN mkdir -p /fluentd/conf.d && \  
mkdir -p /fluentd/etc && \  
mkdir -p /fluentd/plugins  
  
# Default settings  
ENV LOG_FORMAT "json"  
ENV FLUSH_INTERVAL "30s"  
ENV NUM_THREADS "1"  
ENV SOURCE_CATEGORY "%{namespace}/%{pod_name}"  
ENV SOURCE_CATEGORY_PREFIX "kubernetes/"  
ENV SOURCE_CATEGORY_REPLACE_DASH "/"  
ENV SOURCE_NAME "%{namespace}.%{pod}.%{container}"  
ENV KUBERNETES_META "true"  
COPY ./conf.d/* /fluentd/conf.d/  
COPY ./etc/* /fluentd/etc/  
COPY ./plugins/* /fluentd/plugins/  
  
CMD exec fluentd -c /fluentd/etc/$FLUENTD_CONF -p /fluentd/plugins
$FLUENTD_OPT  

