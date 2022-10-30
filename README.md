# hive-installation
Hive installation with mysql metastore

## Install mysql
- sudo apt update
- sudo apt install mysql-server
- sudo systemctl start mysql.service
- sudo mysql
- ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
- exit

## Download & Install Hive

- Sebelum install hive, pastikan kamu telah melakukan instalasi hadoop, yang akan digunakan untuk menyimpan data melalui hdfs.
Download Hive :
`wget https://archive.apache.org/dist/hive/hive-2.3.9/apache-hive-2.3.9-bin.tar.gz`
    
- Uncompress file Apache Hive :
`tar -xvf apache-hive-2.3.9-bin.tar.gz`

## Konfigurasi Hive

- Lakukan update environtment variable pada file .bashrc, dengan menjalankan perintah `sudo vi .bashrc`
Tambahkan konfigurasi berikut ke dalam file .bashrc :

```bash
# Set HIVE_HOME
export HIVE_HOME=/home/(server name)/apache-hive-2.3.9-bin
export PATH=$PATH:/home/(server name)/apache-hive-2.3.9-bin/bin
```

- Jalankan perintah berikut untuk melakukan update pada environtment variable
`source ~/.bashrc`

Konfigurasi **hive-env.sh**

`cd apache-hive-2.3.9-bin/`

`sudo vim conf/hive-env.sh`

Atur parameter sesuai dengan berikut ini,

```bash
HADOOP_HOME=$HOME/hadoop-3.3.4
export HADOOP_HEAPSIZE=512
export HIVE_CONF_DIR=$HOME/apache-hive-2.3.9-bin/con
```

Konfigurasi **hive-site.xml**

`sudo vim conf/hive-site.xml`

Masukkan konfigurasi berikut ke dalam hive-site.xml

```bash
<configuration>
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://localhost:3306/metastore_db?createDatabaseIfNotExist=true&amp;useSSL=true</value>
        <description>metadata is stored in a MySQL server</description>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
        <description>MySQL JDBC driver class</description>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>root</value>
        <description>user name for connecting to mysql server</description>
    </property>
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>pasword</value>
        <description>password for connecting to mysql server</description>
    </property>
    <property>
        <name>datanucleus.schema.autoCreateAll</name>
        <value>true</value>
        <description>auto create schema</description>
</property>
<property>
        <name>hive.metastore.schema.verification</name>
        <value>false</value>
        <description>schema verification</description>
</property>

  <property>
      <name>hive.server2.thrift.port</name>
      <value>10001</value>
      <description>TCP port number to listen on, default 10000</description>
    </property>

    <property>
        <name>hive.server2.enable.doAs</name>
        <value>false</value>
        <description>
          Setting this property to true will have HiveServer2 execute
          Hive operations as the user making the calls to it.
        </description>
    </property>
</configuration>
```

note : jangan lupa masukan connector mysql to hive lib

## Menjalankan Apache Hive

- Lakukan inisiasi database yang akan digunakan oleh Hive.
`bin/schematool -dbType mysql -initSchema`
- Apabila berhasil, jalankan Hive dengan menggunakan perintah berikut
`hive`
