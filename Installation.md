# Tomcat Introduction
Tomcat is a application deployment server, it is typically used for .war deployments and .ear deployments randomly. 
Tomcat is a middleware components, allows to deploy multiple .war files using context. 
# Tomcat Installation
## Installation pre-requisites
- JRE (Java Runtime environment) is the pre-requisite for Tomcat, however JDK comes with JRE.
- To install tomcat, we need a deployment server on which tomcat will be running
## Building Deployment server
- In current deployment, Cent OS is used.
- Tomcat9 with JDK 17 will be installed
## Installing Java 1.8 on the Cent OS 
cd /opt
wget -c --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.rpm
sudo yum install java -y 
## Tomcat Installation 
sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.84/bin/apache-tomcat-9.0.84.zip
sudo yum install unzip
unzip apac...
## verify the folder structure in tomcat
'bin - all the operation scripts will be present in the bin folder (statup, shutdown and catalina)
'conf - server.xml and tomcat-users.xml
'libs - This will consists of all the libraries, any application developed in java will consists of jar files and library will store and jar files.
'Logs - tomcat logs, when we start the tomcat, it will have manager.log, hostmanager.log, catalina.out, localhost.log.
'webapps - By default, there will be 5 apps will be present in the folder, we can deploy any customer application here
'Temp - It consists of temporary files 
'work - once we deploy the application in the webapps folder, those apps will be generating some files (ex: jar, pom.xml, property), these are stored in work directory.
## providing executable permissions on the bin folder
ls -la 
sudo chmod +x *
## starting tomcat process 
sh startup.sh
'''bash
# verify the java process is running or not
ps -ef | grep -i java*
# run the startup.sh from any where in the instance, we need to create a link
ln -s /opt/apache/bin/startup.sh /usr/bin/startTomcat
ln -s /opt/apache/bin/shutdown.sh /usr/bin/stopTomcat
'''
# Access the tomcat from the webbrowser, default port is 8080
http:<hostname/publicIP>:8080
## Access denied (403) on manager tab on tomcat homepage
on the homepage, click on 'manager' and 'hostmanager'
To address the issue, we need to modify the context.xml file. file is located in META_INA  
'''bash
vi opt/apache/webapp/manager/META_INF/context.xml
# Comment below lines 
<!--
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
-->
'''
repeat the same with host-manager as well. This allow the remote IP address to access manager and host-manager applications, this leave us with another problem to solve, it is authentication. lets look into it.
## credentials to login to manager
* we need to perform the following steps for authentication:
* important files /opt/apache/conf/tomcat-users.xml, uses to add uses and 
'''bash
# go to tomcat-users.xml to add users to login to manager and hostmanager
vi /opt/apache/conf/tomcat-users.xml
# add below lines at the bottom
<user username="rajesh" password="tomcat", roles="manager-gui,admin-gui,admin-script,manager-script, manager-status,manager-jmx"/>
'''
* when you login using the credentials, you will be able to see the list of application, with no new customer applications, you would be able to see the default applications.
## change the default port number
* Go to server.xml
'''bash
# go to the server.xml 
sudo vi /opt/apache/conf/server.xml
# search for communication port :/communication 
''' 
