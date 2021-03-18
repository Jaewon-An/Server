### 1. 톰캣 다운로드
  <https://tomcat.apache.org/download-90.cgi>

### 2. 톰캣설치
```console
# cd /home/jnet
# yum install java-1.8*
# wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.44/bin/apache-tomcat-9.0.44.tar.gz
# tar xzvf apache-tomcat-9.0.44.tar.gz
# ln -s apache-tomcat-9.0.44.tar.gz tomcat
```

### 3. tomcat-native 설치

``` console
# yum install gcc apr-1.4* apr-devel openssl*
# cd tomcat/bin
# tar xzvf tomcat-native.tar.gz
# cd tomcat-native-1.2.17-src/native
# configure
# make
# make install
```

### 4. 톰캣 설정
JSVC_OPTS=’-Xms4096m - Xmx4096m -XX:NewSize=2048m -XX:MaxNewSize=2048m’


### [참고사이트]
[centos 7.0에서 was(tomcat9) 설치하기!](https://sethlee.tistory.com/2)
