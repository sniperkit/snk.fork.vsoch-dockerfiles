################## BASE IMAGE ######################  
FROM biocontainers/biocontainers:latest  
  
################## METADATA ######################  
LABEL base.image="biocontainers:latest"  
LABEL version="2"  
LABEL software="ProteinProphet"  
LABEL software.version="201510131012"  
LABEL about.summary="Protein inference for MS/MS analysis"  
LABEL about.home="http://proteinprophet.sourceforge.net/"  
LABEL about.documentation="http://proteinprophet.sourceforge.net/"  
LABEL license="http://proteinprophet.sourceforge.net/"  
LABEL about.tags="Proteomics"  
  
################## MAINTAINER ######################  
MAINTAINER Felipe da Veiga Leprevost <felipe@leprevost.com.br>  
  
USER root  
  
RUN apt-get update && \  
apt-get install -y gnuplot && \  
mkdir -p /usr/local/tpp/bin/  
  
RUN ZIP=DatabaseParser.zip && \  
wget https://github.com/BioDocker/software-
archive/releases/download/DatabaseParser/$ZIP -O /tmp/$ZIP && \  
unzip /tmp/$ZIP -d /usr/local/tpp/bin/ && \  
rm /tmp/$ZIP  
  
RUN ZIP=batchcoverage.zip && \  
wget https://github.com/BioDocker/software-
archive/releases/download/BatchCoverage/$ZIP -O /tmp/$ZIP && \  
unzip /tmp/$ZIP -d /usr/local/tpp/bin/ && \  
rm /tmp/$ZIP  
  
USER biodocker  
  
RUN ZIP=ProteinProphet.zip && \  
wget https://github.com/BioDocker/software-
archive/releases/download/ProteinProphet/$ZIP -O /tmp/$ZIP && \  
unzip /tmp/$ZIP -d /home/biodocker/bin/ && \  
chmod -R 755 /home/biodocker/bin/* && \  
rm /tmp/$ZIP  
  
ENV PATH /home/biodocker/bin:$PATH  
  
WORKDIR /data/  
  
CMD ["ProteinProphet"]  

