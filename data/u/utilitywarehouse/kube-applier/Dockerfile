FROM alpine:3.7  
ENV GOPATH=/go  
  
WORKDIR /go/src/github.com/utilitywarehouse/kube-applier  
COPY . /go/src/github.com/utilitywarehouse/kube-applier  
COPY templates/* /templates/  
COPY static/ /static/  
  
RUN \  
apk --no-cache add curl ca-certificates git go musl-dev && \  
curl -sSL -o /usr/local/bin/kubectl https://storage.googleapis.com/kubernetes-
release/release/v1.9.3/bin/linux/amd64/kubectl && \  
chmod +x /usr/local/bin/kubectl && \  
go get -t ./... && \  
go test ./... && \  
CGO_ENABLED=0 go build -ldflags '-s -extldflags "-static"' -o /kube-applier .
&& \  
apk del curl go musl-dev && rm -r /go  
  
CMD [ "/kube-applier" ]  

