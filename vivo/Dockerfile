FROM tomcat:9.0.21-jdk11-openjdk

MAINTAINER Ralph OFlinn <roflinn@uab.edu>
MAINTAINER Justin Merz <jrmerz@ucdavis.edu>
MAINTAINER Quinn Hart <qjhart@ucdavis.edu>

RUN apt-get update
RUN apt-get install -y maven git

# ENV VIVO_MD5 861865a57ded2bc99237e1c78f90d45c
# ENV VIVO_VERSION 1.10.0

# RUN mkdir -v /opt/vivo
# WORKDIR /opt/vivo
# RUN echo "$VIVO_MD5  vivo.tar.gz" > vivo.tar.gz.md5
# RUN wget -O vivo.tar.gz https://github.com/vivo-project/VIVO/releases/download/vivo-${VIVO_VERSION}/VIVO-${VIVO_VERSION}.tar.gz && \
#     md5sum vivo.tar.gz.md5 && \
#     tar -xzf vivo.tar.gz && rm vivo.tar.gz

WORKDIR /usr/local/
RUN git clone https://github.com/vivo-project/Vitro.git Vitro -b rel-1.11.0-RC
RUN git clone https://github.com/vivo-project/VIVO.git VIVO -b rel-1.11.0-RC
WORKDIR /usr/local/VIVO/
RUN mvn install -s installer/example-settings.xml

# clean up
# RUN rm -r /opt/vivo
# RUN rm -r /usr/local/tomcat/webapps/vivosolr
RUN rm -r /usr/local/tomcat/webapps/docs
RUN rm -r /usr/local/tomcat/webapps/examples

# Set properties
# Adjust logging, you can mount over this in docker-compose to further adjust
COPY ./log4j.properties /usr/local/tomcat/webapps/VIVO/WEB-INF/classes
WORKDIR /usr/local/VIVO/home/config
COPY ./dockercompose.runtime.properties runtime.properties
COPY ./example.applicationSetup.n3 applicationSetup.n3

RUN export CATALINA_OPTS="-Xms512m -Xmx512m -XX:MaxPermSize=128m"
