FROM golang:1.9.2-alpine  
  
WORKDIR /go/src/strest-grpc  
  
ADD . /go/src/github.com/buoyantio/strest-grpc  
  
RUN go build -o /go/bin/strest-grpc /go/src/github.com/buoyantio/strest-
grpc/main.go  
  
ENTRYPOINT ["/go/bin/strest-grpc"]  

