# Tomcat installation on EC2 instance

### Pre-requisites
1. EC2 instance with Java v1.8.x 

### Install Apache Tomcat
1. Download tomcat packages from  https://tomcat.apache.org/download-80.cgi onto /opt on EC2 instance
   ```sh 
   # Create tomcat directory
   mkdir /opt/tomcat
   cd /opt/tomcat
   wget https://mirrors.sonic.net/apache/tomcat/tomcat-8/v8.5.57/bin/apache-tomcat-8.5.57.tar.gz
   tar -xvzf apache-tomcat-8.5.57.tar.gz
   ```
1. give executing permissions to startup.sh and shutdown.sh which are under bin. 
   ```sh
   chmod +x /opt/apache-tomcat-8.5.35/bin/startup.sh 
   chmod +x /opt/tomcat/bin 

   shutdown.sh
   ```

1. create link files for tomcat startup.sh and shutdown.sh 
   ```sh
   ln -s /opt/tomcat/bin/startup.sh /usr/local/bin/tomcatup
   ln -s /opt/tomcat/bin/shutdown.sh /usr/local/bin/tomcatdown
   tomcatup
   ```
   
  To check tomcat Version:
  cd tomcat/lib 
java -cp catalina.jar org.apache.catalina.util.ServerInfo

  #### Check point :
access tomcat application from browser on port 8080  
 - http://<Public_IP>:8080

  Using unique ports for each application is a best practice in an environment. But tomcat and Jenkins runs on ports number 8080. Hence lets change tomcat port number to 8090. Change port number in conf/server.xml file under tomcat home
   ```sh
 cd /opt/tomcat/conf
# update port number in the "connecter port" field in server.xml
# restart tomcat after configuration update
tomcatdown
tomcatup
```
#### Check point :
Access tomcat application from browser on port 8090  
 - http://<Public_IP>:8090

1. now application is accessible on port 8090. but tomcat application doesnt allow to login from browser. changing a default parameter in context.xml does address this issue
   ```sh
   #search for context.xml
   find / -name context.xml
   ```
1. above command gives 3 context.xml files. comment (<!-- & -->) `Value ClassName` field on files which are under webapp directory. 
After that restart tomcat services to effect these changes
   ```sh 
   tomcatdown
   tomcatup
   ```
1. Update users information in the tomcat-users.xml file
goto tomcat home directory and comment 127.0.0 in /opt/tomcat/webapps/manager/META-INF/context.xml and vi /opt/tomcat/webapps/host-manager/META-INF/context.xml file

Add below roles in 

/opt/tomcat/conf/tomcat-users.xml


   ```sh
	<role rolename="manager-gui"/>
	<role rolename="manager-script"/>
	<role rolename="manager-jmx"/>
	<role rolename="manager-status"/>
	<user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/>
	<user username="testingworld" password="testingworld" roles="manager-script"/>
	<user username="tomcat" password="s3cret" roles="manager-gui"/>
   ```
1. Restart serivce and try to login to tomcat application from the browser. This time it should be Successful


Deploy War File in tomcat through Jenkins:

(1)In Jenkins Manage Plugin 
Install Deploy to container plugins
New Project : Deploy_On_Container Maven Project

Repository URL : https://github.com/AWSDemo2504/Hello-World.git

Goals and Options : clean install package

Post Build Actions : Deploy_on_container 
War Files : **/*.war
Tomcat 8.x : 
set jenkins credentials :
testingworld/testingworld
ex: tomcat url : http://localhost:8090/

Open the url in browser : http://13.232.6.15:8090/webapp/

