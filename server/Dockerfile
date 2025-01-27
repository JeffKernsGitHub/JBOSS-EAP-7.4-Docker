FROM fedora:latest

USER root

# Update the image
RUN dnf update -y
RUN dnf install unzip nano freetype fontconfig -y

# Set working directory
WORKDIR /opt

# Create jboss-eap system user  
RUN useradd --system jboss-eap

## Start JDK

# Copy JDK 17, jliked 'jre' in future
COPY serverjre-11.0.26.zip /opt/

# Extract JDK
RUN unzip serverjre-11.0.26.zip -d /opt/

# Change owner 
RUN chown -R root:jboss-eap /opt/serverjre-11.0.26

# Permission JDK
RUN chmod -R 777 /opt/serverjre-11.0.26

# Create soft link to alias as /opt/jdk11
RUN ln -s /opt/serverjre-11.0.26 /opt/jdk11

# Remove JDK zip file
RUN rm /opt/serverjre-11.0.26.zip

# Export JAVA_HOME for rest of script
#RUN export JAVA_HOME=/opt/jdk11

## End JDK

## Start Extra Config
# Add any extra configuration steps here
## End Extra Config

## Start JBOSS

# Copy JBOSS
COPY jboss-eap-7.4.zip /opt/

# Extract JBOSS
RUN unzip jboss-eap-7.4.zip -d /opt/

# Change owner 
RUN chown -R root:jboss-eap /opt/jboss-eap-7.4

# Permission JBOSS
RUN chmod -R 777 /opt/jboss-eap-7.4

# Delete zip
RUN rm jboss-eap-7.4.zip

# Create Management User - Removed as it should be part of baselined zip
# WORKDIR /opt/jboss-eap-7.4/bin
# RUN export PATH=$PATH:/opt/jdk11/bin
# RUN './add-user.sh -u mgmtuser1 -p password1! -cw'


COPY Lynx.zip /opt/

# Extract Lynx Config
RUN unzip Lynx.zip -d /opt/

# Change owner 
RUN chown -R root:jboss-eap /opt/Lynx

# Permission Lynx Config
RUN chmod -R 777 /opt/Lynx

# Delete zip
RUN rm Lynx.zip


 # drop back to the regular JBOSS user - good practice
USER jboss-eap

