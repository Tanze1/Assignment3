# PBDAssignment3
Install Hadoop

Step 1 - 
Download and extract Hadoop
Download Hadoop from their official website and unzip the file. We'll be using Hadoop 3.2.1.
https://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-3.2.1/hadoop-3.2.1.tar.gz
Hadoop is portable so you can store it on an external hard drive. For the purpose of documentation, I will extract it to C:/Users/Anthony/Documents/cp-master.

If there are permission errors, run your unzipping program as administrator and unzip again.

Install Hadoop native IO binary
Clone or download the winutils repository 
https://github.com/cdarlint/winutils
and copy the contents of hadoop-3.2.1/bin into the extracted location of the Hadoop binary package. In our example, it will be C:\Users\Anthony\Documents\cp-master\hadoop-3.2.1\bin

Install Java JDK
Java JDK is required to run Hadoop, so if you haven't installed it, install it.

Oracle requires you sign up and login to download it. I suggest you find an alternative resource to download it from for example here (JDK 8u261)
https://enos.itcollege.ee/~jpoial/allalaadimised/jdk8/
This resource might not exist forever, so Google 'jdk version download'.

Run the installation file and the default installation directory will be C:\Program Files\Java\jdk1.8.0_261.

After installation, open up CMD or Powershell and confirm Java is intalled:

$ java -version
java version "1.8.0_261"
Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode)

Step 2 - 
Configure environment variables
Open the Start Menu and type in 'environment' and press enter. A new window with System Properties should open up. Click the Environment Variables button near the bottom right.

JAVA_HOME environment variable
From step 3, find the location of where you installed Java. In this example, the default directory is C:\Program Files\Java\jdk1.8.0_261
Create a new User variable with the variable name as JAVA_HOME and the value as C:\Program Files\Java\jdk1.8.0_261

HADOOP_HOME environment variable
From step 1, copy the directory you extracted the Hadoop binaries to. In this example, the directory is C:\Users\Anthony\Documents\cp-master\hadoop-3.2.1
Create a new User variable with the variable name as HADOOP_HOME and the value as C:\Users\Anthony\Documents\cp-master\hadoop-3.2.1

PATH environment variable
We'll now need to add the bin folders to the PATH environment variable.

Click Path then Edit
Click New on the top right
Add C:\Users\Anthony\Documents\cp-master\hadoop-3.2.1\bin
Add C:\Program Files\Java\jdk1.8.0_261\bin


Step 3 -
Hadoop environment
Hadoop complains about the directory if the JAVA_HOME directory has spaces. In the default installation directory, Program Files has a space which is problematic. To fix this, open the %HADOOP_HOME%\etc\hadoop\hadoop-env.cmd and change the JAVA_HOME line to the following:

set JAVA_HOME=C:\PROGRA~1\Java\jdk1.8.0_261
After setting those environment variables, you reopen CMD or Powershell and verify that the hadoop command is available:

$ hadoop -version
java version "1.8.0_261"
Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode) 

Configure core site
Edit core-site.xml and replace the configuration element with the following:

<configuration>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://0.0.0.0:19000</value>
  </property>
</configuration>

Configure HDFS
Create two folders, one for the namenode directory and another for the data directory. The following are the two created folders in this example:

C:\Users\Anthony\Documents\cp-master\hadoop-3.2.1\data\dfs\namespace_logs
C:\Users\Anthony\Documents\cp-master\hadoop-3.2.1\data\dfs\data
Edit hdfs-site.xml and replace the configuration element with the following:

<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <!-- <value>file:///DIRECTORY 1 HERE</value> -->
    <value>file:///C:/Users/Anthony/Documents/cp-master/hadoop-3.2.1/data/dfs/namespace_logs</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <!-- <value>file:///DIRECTORY 2 HERE</value> -->
    <value>file:///C:/Users/Anthony/Documents/cp-master/hadoop-3.2.1/data/dfs/data</value>
  </property>
</configuration>

Configure MapReduce and YARN site
Edit mapred-site.xml and replace the configuration element with the following:

<configuration>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
  <property> 
    <name>mapreduce.application.classpath</name>
    <value>%HADOOP_HOME%/share/hadoop/mapreduce/*,%HADOOP_HOME%/share/hadoop/mapreduce/lib/*,%HADOOP_HOME%/share/hadoop/common/*,%HADOOP_HOME%/share/hadoop/common/lib/*,%HADOOP_HOME%/share/hadoop/yarn/*,%HADOOP_HOME%/share/hadoop/yarn/lib/*,%HADOOP_HOME%/share/hadoop/hdfs/*,%HADOOP_HOME%/share/hadoop/hdfs/lib/*</value>
  </property>
</configuration>

Edit yarn-site.xml and replace the configuration element with the following:

<configuration>
  <property>
    <name>yarn.resourcemanager.hostname</name>
    <value>localhost</value>
  </property>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
  <property>
    <name>yarn.nodemanager.env-whitelist</name>
    <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
  </property>
</configuration>
