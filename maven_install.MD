Step 1:
Download apache-maven-3.6.3-bin.tar.gz binary archive from the official link. 
Download maven packages https://maven.apache.org/download.cgi onto Jenkins server. In this case, I am using /opt/maven as my installation directory Link : https://maven.apache.org/download.cgi

     # Creating maven directory under /opt
     mkdir /opt/maven
     cd /opt/maven
     # downloading maven version 3.6.0
     wget http://mirrors.estointernet.in/apache/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz
     tar -xvzf apache-maven-3.6.1-bin.tar.gz

Step 2:
Open the Terminal and change the directory to /opt folder.

cd /opt/maven

Step 3:
Extract the apache-maven archive into the opt directory.

sudo tar -xvzf ~/Downloads/apache-maven-3.6.3-bin.tar.gz


Step 4:
Edit the /etc/environment file and add the following environment variable:

M2_HOME="/opt/maven"

Copy
also, append the bin directory to the PATH variable:

/opt/maven/bin

Copy

You can use nano to edit the file in the terminal itself. Execute the following command and modify the content as given below.

sudo nano /etc/environment

Copy
WARNING: Do not replace your environment file with the following content because you may already have different environment variables which are required by other applications to function properly. Notice the end of PATH variable and the M2_HOME variable.

PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/jdk-10.0.2/bin:/opt/apache-maven-3.6.3/bin"
JAVA_HOME="/usr/lib/jvm/jdk-10.0.2"
M2_HOME="/opt/maven"

Step 5:
Update the mvn command:

sudo update-alternatives --install "/usr/bin/mvn" "mvn" "/opt/maven/bin/mvn" 0
sudo update-alternatives --set mvn /opt/maven/bin/mvn


Step 6:
Add Bash completion to mvn so that you can complete complex Maven commands by hitting Tab multiple times.

sudo wget https://raw.github.com/dimaj/maven-bash-completion/master/bash_completion.bash --output-document /etc/bash_completion.d/mvn

Step 7:
Logout and login to the computer and check the Maven version using the following command.

mvn --version

1. Setup M2_HOME and M2 paths in .bash_profile of the user and add these to the path variable
   ```sh
   vi ~/.bash_profile
   vi ~/.profile
   M2_HOME=/opt/maven/apache-maven-3.6.1
   M2=$M2_HOME/bin
   PATH=<Existing_PATH>:$M2_HOME:$M2
   ```
#### Checkpoint 
1. logoff and login to check maven version
  
    ```sh
    mvn --version
    ```
So far we have completed the installation of maven software to support maven plugin on the jenkins console. Let's jump onto Jenkins to complete the remaining steps. 

### Setup maven on Jenkins console
1. Install maven plugin without restart  
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Invoker`
  - `Manage Jenkins` > `Jenkins Plugins` > `available` > `Maven Integration`

2. Configure maven path
  - `Manage Jenkins` > `Global Tool Configuration` > `Maven`
  
  clean install package






