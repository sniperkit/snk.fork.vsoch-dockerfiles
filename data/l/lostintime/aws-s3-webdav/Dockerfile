# Build the app  
FROM ekidd/rust-musl-builder:1.25.0 as builder  
  
WORKDIR /home/rust/src  
  
COPY . .  
  
RUN cargo build --release --target x86_64-unknown-linux-musl  
  
# Build using this image: https://hub.docker.com/r/ekidd/rust-musl-builder/  
FROM alpine:3.7  
# install openssl  
RUN apk update && apk add openssl  
  
COPY \--from=builder /home/rust/src/target/x86_64-unknown-linux-
musl/release/aws_s3_webdav /usr/bin/aws_s3_webdav  
  
EXPOSE 8080  
ENTRYPOINT [ "/usr/bin/aws_s3_webdav" ]  

