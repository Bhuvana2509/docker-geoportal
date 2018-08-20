From tomcat:8-jre8 
MAINTAINER Bhuvana

# Install wget and install/updates certificates
RUN apt-get update \
 && apt-get install -y -q --no-install-recommends \
    ca-certificates \
    git \
    maven \
    openjdk-8-jdk\
    ca-certificates-java\
    #openjdk-8-jdk="$JAVA_DEBIAN_VERSION" \
    #ca-certificates-java="$CA_CERTIFICATES_JAVA_VERSION" \
 && apt-get clean \
 && rm -r /var/lib/apt/lists/*

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

# Copy to images tomcat path 
ADD web.xml /usr/local/tomcat/conf/ 
ADD obclient.properties /etc/ 
ADD WebClient.properties /etc/ 
ADD geoportal.war /usr/local/tomcat/webapps/ 

ADD settings.xml /usr/local/tomcat/conf/
ADD tomcat-users.xml /usr/local/tomcat/conf/


# Install geoportal web application
ENV GPTURL=https://github.com/Bhuvana2509/geoportal_server/archive/v1.2.zip

ADD $GPTURL /tmp/geoportal.zip

WORKDIR $CATALINA_HOME/webapps/
RUN unzip /tmp/geoportal.zip *.war
RUN rm /tmp/geoportal.zip

#COPY tomcat-users.xml $CATALINA_HOME/conf

# Install harvester application
ENV GITURL=https://github.com/Esri/geoportal-server-harvester.git

WORKDIR /tmp
RUN git clone ${GITURL} harvester
WORKDIR /tmp/harvester
RUN mvn package -DskipTests

WORKDIR $CATALINA_HOME/webapps/

RUN cp /tmp/harvester/geoportal-application/geoportal-harvester-war/target/*.war harvester.war
