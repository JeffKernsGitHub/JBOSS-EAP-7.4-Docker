FROM fedora:latest

USER root

# Update the image
RUN dnf update -y
RUN dnf install unzip freetype nano -y

# Set working directory
WORKDIR /opt

# Create jboss-eap system user  
RUN useradd --system jboss-eap

## Start JDK

# Copy JDK 17, jliked 'jre' in future
COPY jdk-11.0.26_linux-x64_bin.zip /opt/

# Extract JDK
RUN unzip jdk-11.0.26_linux-x64_bin.zip -d /opt/

# Change owner 
RUN chown -R root:jboss-eap /opt/jdk-11.0.26_linux-x64_bin

# Permission JDK
RUN chmod -R 777 /opt/jdk-11.0.26_linux-x64_bin

# Create soft link to alias as /opt/jdk11
RUN ln -s /opt/jdk-11.0.25_linux-x64_bin/jdk-11.0.26 /opt/jdk11

# Remove JDK zip file
RUN rm /opt/jdk-11.0.26_linux-x64_bin.zip

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

 # drop back to the regular JBOSS user - good practice
USER jboss-eap

