
### 1. JAVA 설치(root 권한)
```console
[root@localhost ~]# yum install java-1.8*
[root@localhost ~]# vi /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
```

### 2. Tomcat 설치(jnet 계정 사전생성필요)
```console
[root@localhost ~]# cd /home/jnet
[root@localhost jnet]# wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.44/bin/apache-tomcat-9.0.44.tar.gz
[root@localhost jnet]# tar xzvf apache-tomcat-9.0.44.tar.gz
[root@localhost jnet]# mv apache-tomcat-9.0.44 tomcat9
```

### 3. Tomcat 설정파일 생성
```console
[root@localhost ~]# cd /home/jnet
[root@localhost jnet]# wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.44/bin/apache-tomcat-9.0.44.tar.gz
[root@localhost jnet]# tar xzvf apache-tomcat-9.0.44.tar.gz
[root@localhost jnet]# mv apache-tomcat-9.0.44 tomcat9
```
### 3. tomcat-native 설치(root 권한필요)
``` console
[root@localhost ~]# yum install gcc make redhat-rpm-config apr* openssl*
[root@localhost ~]# cd /home/jnet/tomcat9/bin
[root@localhost bin]# tar xzvf tomcat-native.tar.gz
[root@localhost bin]#  cd tomcat-native-1.2.26-src/native
[root@localhost native]#  ./configure
[root@localhost native]# make && make install
[root@localhost native]## vi /etc/profile
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/apr/lib
```

### 4. Tomcat 설정파일 생성
```console
[root@localhost ~]# cd /home/jnet/tomcat9/bin
[root@localhost bin]#  vi setenv.sh
#!/bin/bash
CATALINA_OPTS="$CATALINA_OPTS -server -Dfile.encoding=UTF8 -Duser.timezone=GMT+9"
CATALINA_OPTS="$CATALINA_OPTS -Xms4096m -Xmx4096m -XX:NewSize=2048m -XX:MaxNewSize=2048m"
[root@localhost bin]## chmod a+x setenv.sh
```

### 5. 권한설정 및 테스트
```console
[root@localhost ~]# cd /home/jnet/
[root@localhost ~]# chown jnet:jnet -R /home/jnet/tomcat9
[root@localhost ~]# su - jnet
[jnet@localhost ~]$ cd /home/jnet/tomcat9/bin
[jnet@localhost ~]$ ./daemon.sh start
