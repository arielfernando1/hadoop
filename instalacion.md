# Hadoop (2.7.3) Cluster with multiple nodes on CentOS 7
<!-- Port	Node
2220	Master
2221	Node1
2223 	Node2 -->
<!-- # Port forward
333	 > 80
6666 > 8042
5555 > 8088
4040 > 50070 -->
We will install Hadoop 2.7.3 on CentOS 7. We will create a cluster with 3 nodes. One master and two slaves. We will use VirtualBox to create the virtual machines. We will use CentOS 7 as the operating system.
## Set hostnames
> Before setting hostnames make sure you set the static IP addresses for the nodes.
```bash
# on master
sudo hostnamectl set-hostname master
# on node1
sudo hostnamectl set-hostname node1
# on node2
sudo hostnamectl set-hostname node2
# on node N
sudo hostnamectl set-hostname nodeN
```

## Create hadoop user on each node
```bash
sudo useradd -d /home/hadop hadoop
sudo passwd hadoop
# Generate ssh key
ssh-keygen -t rsa
ssh-copy-id [hostname]
```


## Configure network
192.168.56.2 master

192.168.56.3 node1

192.168.56.4 node2
```bash
# on master
sudo nano /etc/hosts
```
> Add the hostnames and IP addresses of all nodes to the /etc/hosts file.
```bash
192.168.56.3 node1
192.168.56.4 node2
```

##  Install JDK
```bash
sudo wget https://javadl.oracle.com/webapps/download/GetFile/1.8.0_321-b07/df5ad55fdd604472a86a45a217032c7d/linux-i586/jdk-8u321-linux-x64.rpm
rpm -Uvh jdk-8u321-linux-x64.rpm
java -version
```
> Repeat on all nodes

# Copy SSH keys from slave to master nodes
```bash
# For master
ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@node1
ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@node2
# For node1
ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@master
ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@node2
# For node2
ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@master
ssh-copy-id -i ~/.ssh/id_rsa.pub hadoop@node1
```

# Copy hadoop src
cd /opt
scp -p hadoop-2.7.3.tar.gz hadoop@node2:/home/hadoop/ 
tar -zxf hadoop-2.7.3.tar.gz
mv hadoop-2.7.3 hadoop
# Copy master configs to slaves
scp -p /home/hadoop/hadoop/etc/hadoop/* hadoop@node2:/home/hadoop/hadoop/etc/hadoop/
# Hadoop ENV
nano $HADOOP_HOME/etc/hadoop/hadoop-env.sh
# Java Home
/usr/java/jdk1.8.0_321-amd64/bin/java
# Refresh nodes
hdfs dfsadmin -refreshNodes
## Create directory
```bash
# on master, node1, node2
su - root
mkdir -p /home/hadoop/volume/namenode
mkdir -p /home/hadoop/volume/datanode
chown -R hadoop:hadoop /home/hadoop/volume/
hdfs namenode -format
```
## Start Hadoop
```bash
# on master
start-dfs.sh
start-yarn.sh
```
## Check Hadoop
```bash
# on master
jps
hdfs dfsadmin -report
```

## HDFS commands
```bash
hdfs dfs -ls /
hdfs dfs -mkdir /user
hdfs dfs -copyFromLocal /home/hadoop/hadoop-2.7.3/etc/hadoop /user
```

### Other commands

> Uninstall Java
```bash
# Uninstall java
sudo yum remove jdk
```

