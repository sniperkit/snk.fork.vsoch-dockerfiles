FROM alpine:latest  
  
ENV TERRAFORM_VERSION=0.9.11  
ENV
TERRAFORM_SHA256SUM=804d31cfa5fee5c2b1bff7816b64f0e26b1d766ac347c67091adccc2626e16f3  
  
RUN apk add --update git curl openssh && \  
curl
https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip
> terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \  
echo "${TERRAFORM_SHA256SUM} terraform_${TERRAFORM_VERSION}_linux_amd64.zip" >
terraform_${TERRAFORM_VERSION}_SHA256SUMS && \  
sha256sum -cs terraform_${TERRAFORM_VERSION}_SHA256SUMS && \  
unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip -d /bin && \  
rm -f terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \  
apk add --update python python-dev py-pip build-base && \  
pip install --upgrade --user awscli  
  
ENTRYPOINT ["/bin/terraform"]  

