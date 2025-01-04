# JBOSS EAP 7.4 Docker

Project to Create a RedHat JBOSS EAP Container


# Multiple Container Study

On client:

```
jeffkerns@cyberdeck:~$ docker commit -p a62935864154166b8a0dd5355963f90f20e6b5947fd3350595fc7258eb2d7155 my_jboss0 
sha256:7735c958b789f66b97cad00c1e267799e957d42ec224838431ef5b3937be0841
jeffkerns@cyberdeck:~$ docker commit -p 046f257f20e1cdda1b4fc87689a9a6654a5d090e04ef8a18044ae437f0f3c29a my_jboss1
sha256:303fc985cb53e35212ce212d31e95d9b516c12b1eb11aa712d63e85772775561
jeffkerns@cyberdeck:~$ docker save -o my_jboss0.tar my_jboss0
jeffkerns@cyberdeck:~$ 
jeffkerns@cyberdeck:~$ ls
'Calibre Library'   Desktop   Development   Documents   Downloads   Dropbox   Infocom   Music   my_jboss0.tar   Pictures   Public   Templates   Videos
jeffkerns@cyberdeck:~$ ls -l my_jboss0.tar 
-rw------- 1 jeffkerns jeffkerns 3246739456 Jan  4 13:01 my_jboss0.tar
jeffkerns@cyberdeck:~$ docker save -o my_jboss1.tar my_jboss1
jeffkerns@cyberdeck:~$ scp my_jboss* root@192.168.124.231:/root
root@192.168.124.231's password: 
my_jboss0.tar                                                                                                                                            100% 3096MB 406.5MB/s   00:07    
my_jboss1.tar                                                                                                                                            100% 3096MB 559.2MB/s   00:05    
jeffkerns@cyberdeck:~$ 
```

On server:
```
# create fresh log volumes 
docker volume create jboss-eap-log0
docker volume create jboss-eap-log1

# Load Images - used two separate to outline that different JBOSSes can be used.
docker load -i my_jboss0.tar
docker load -i my_jboss1.tar

# create/start containers
docker run --name my_jboss0 -d -e JAVA_HOME=/opt/jdk11 -e JBOSS_HOME=/opt/jboss-eap-7.4 -p 8080:8080 -p 9990:9990 -v jboss-eap-log0:/opt/jboss-eap-7.4/standalone/log jbosseap7:latest /opt/jboss-eap-7.4/bin/standalone.sh -b 0.0.0.0 -bmanagement 0.0.0.0
docker run --name my_jboss1 -d -e JAVA_HOME=/opt/jdk11 -e JBOSS_HOME=/opt/jboss-eap-7.4 -p 8081:8080 -p 9991:9990 -v jboss-eap-log1:/opt/jboss-eap-7.4/standalone/log jbosseap7:latest /opt/jboss-eap-7.4/bin/standalone.sh -b 0.0.0.0 -bmanagement 0.0.0.0


```