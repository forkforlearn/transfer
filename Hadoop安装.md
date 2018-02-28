## Hadoop安装

* ### 建立Hadoop用户  
    > 有没有必要为hadoop用户增加管理员权限呢，先加上吧

    ```bash
    sudo useradd -m hadoop -s /bin/bash
    sudo passwd hadoop
    sudo adduser hadoop sudo
    ```

    ​


* ### 免密码登录
    > 先登录到本机,用hadoop用户，然后退出

    ```bash
    su hadoop
    ssh localhost
    ```

    > 退出后，运行ssh-keygen的时候，一切直接回车

    ```bash
    exit
    cd ~/.ssh/
    ssh-keygen -t rsa
    cat ./id_rsa.pub >> ./authorized_keys
    ```

    ​

*  ###安装Hadoop和Spark

    > 先下载hadoop和spark

    ```bash
    wget https://mirrors.cnnic.cn/apache/hadoop/common/hadoop-2.7.5/hadoop-2.7.5.tar.gz
    wget http://mirror.dsrg.utoronto.ca/apache/spark/spark-2.2.1/spark-2.2.1-bin-hadoop2.7.tgz
    tar zfvx haddop.tar.gz
    sudo mv hadoop /usr/local/
    su hadoop
    vim ~/.bashrc
    vim /etc/environment
    ```

    > 在environment与.bashrc里添加以下内容

    ```bash
    export HADOOP_HOME=/usr/local/hadoop
    export PATH=$PATH:$HADOOP_HOME/bin
    export PATH=$PATH:$HADOOP_HOME/sbin
    export HADOOP_MAPRED_HOME=$HADOOP_HOME
    export HADOOP_COMMON_HOME=$HADOOP_HOME
    export HADOOP_HDFS_HOME=$HADOOP_HOME
    export YARN_MAPRED_HOME=$HADOOP_HOME
    export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
    export HADOOP_OPTS="-DJava.library.path=$HADOOP_HOME/lib"
    export HADOOP_LIBRARY_PATH=$HADOOP_HOME/lib/native:$JAVA_LIBRARY_PATH
    ```

    > 修改hadoop配置:hadoop-env.sh

    ```bash
    export JAVA_HOME=/usr/lib/jvm/java-8-oracle
    ```

    >  修改/etc/hosts和slaves

    ```bash
    vim /etc/hosts
    namenode1 172.16.100.140
    datanode1 172.16.100.141
    datanode2 172.16.100.142
    vim /usr/local/hadoop/etc/hadoop/slaves
    datanode1
    datanode2
    ```

    ​

    ​

    > 修改core-site.xml，添加变量

    ```bash
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://namenode1:9000</value>
        </property>
        <property>
            <name>hadoop.tmp.dir</name>
            <value>file:/usr/local/hadoop/tmp</value>
            <description>Abase for other temporary directories.</description>
        </property>
    ```

    ​

    ​