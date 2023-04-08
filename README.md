# cloudera-docker-compose
Setting up a single node Cloudera Hadoop on cloud with quickstart image, docker and docker compose

## Getting Things Ready
First, it is necessary to have Docker and Docker compose running on your OS.

### Installing Docker
### Installing Docker Compose

### Setting up Single Node Cloudera Hadoop with Docker Compose  
* Get the docker-compose.yaml file from https://github.com/tatsster/cloudera-docker-compose/blob/main/cloudera/docker-compose.yaml and store it in a proper folder  
* Change directory to the folder where docker-compose.yaml exists  
* Execute following command and wait for docker and docker compose do the things for you (If you dont want to see the trace simply execute the command on detach mode)  
``` docker-compose up ``` 
* In the end, you see following final lines that means your Single Node Cloudera Hadoop is ready to use  
![alt text](https://github.com/emirkorkmaz/cloudera-quickstart-docker-compose/blob/master/misc/images/installation_done.png "Installation Done!")  
* Try to each and use Hue, http://IP:8888. User name and password is cloudera/cloudera  
![alt text](https://github.com/emirkorkmaz/cloudera-quickstart-docker-compose/blob/master/misc/images/hue_login.png "Hue Login")  

### Start up Cloudera Manager
Having installation completed, Cloudera Manager is still disabled and it is better to enable it. Here is the simple guide for enabling Cloudera Manager  

* List the quickstart container and attach to it  
``` docker ps ```   
``` docker attach container-id ```  
* Execute following command in container for enabling Cloudera Manager. It will take couple of minutes to be completed  
``` /home/cloudera/cloudera-manager --express ```  
![alt text](https://github.com/emirkorkmaz/cloudera-quickstart-docker-compose/blob/master/misc/images/cm_installed.png "CM Installed")  
* When CM is being started, it stops all services. Hence it is necessary to restart them all over Cloudera Manager
![alt text](https://github.com/emirkorkmaz/cloudera-quickstart-docker-compose/blob/master/misc/images/cm_start.png "CM Installed")  

and We're done! Single node Cloudera Hadoop is ready for use

![alt text](https://github.com/emirkorkmaz/cloudera-quickstart-docker-compose/blob/master/misc/images/cm_services_started.png "We're Done")  

### Install Python packages
#### Update CentOS 6 repo
* Open CentOS-Base repo file at `/etc/yum.repos.d/CentOS-Base.repo`
* Replace content with this [CentOS-Base](https://github.com/tatsster/cloudera-docker-compose/blob/main/centos-repo/CentOS-Base.repo)
* Run following commands
```
yum clean all

yum upgrade --disablerepo=* --enablerepo=base
```
* Use HTTPS in CentOS-Base 
  - Comment all lines `http://linuxsoft.cern.ch`
  - Uncomment all lines `https://vault.centos.org`
* Update EPEL repo
```
yum install wget --disablerepo=cloudera-*

wget https://archives.fedoraproject.org/pub/archive/epel/6/x86_64/epel-release-6-8.noarch.rpm

rpm -Uvh ./epel-release-6*.rpm

rm epel-release-6-8.noarch.rpm
```
* Disable all Cloudera repo
```
curl https://raw.githubusercontent.com/tatsster/cloudera-docker-compose/main/centos-repo/cloudera-accumulo.repo -o /etc/yum.repos.d/cloudera-accumulo.repo

curl https://raw.githubusercontent.com/tatsster/cloudera-docker-compose/main/centos-repo/cloudera-cdh5.repo -o /etc/yum.repos.d/cloudera-cdh5.repo

curl https://raw.githubusercontent.com/tatsster/cloudera-docker-compose/main/centos-repo/cloudera-kafka.repo -o /etc/yum.repos.d/cloudera-kafka.repo

curl https://raw.githubusercontent.com/tatsster/cloudera-docker-compose/main/centos-repo/cloudera-manager.repo -o /etc/yum.repos.d/cloudera-manager.repo
```

#### Install python3 & packages
```
yum install python34 python34-pip

pip3 install --upgrade pip mrjob
```
