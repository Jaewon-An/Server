
### 4. tomcat-daemon 설치 및 자동실행 설정
```console
[root@localhost ~]# cd /home/jnet/tomcat9/bin
[root@localhost bin]# tar xvfz commons-daemon-native.tar.gz
[root@localhost bin]# cd commons-daemon-1.2.4-native-src/unix
[root@localhost unix]# ./support/buildconf.sh
[root@localhost native]#  ./configure
[root@localhost unix]# make
[root@localhost unix]# cp jsvc ../..
[root@localhost unix]# /home/jnet/tomcat9/bin
[root@localhost bin]# vi daemon.sh
test ".$TOMCAT_USER" = . && TOMCAT_USER=jnet
[root@localhost bin]# cp /home/jnet/tomcat9/bin/daemon.sh tomcat9
[root@localhost bin]# chmod a+x tomcat9
[root@localhost bin]# vi tomcat9
#추가 부분
#
# Tomcat startup file
#
# chkconfig: 2345 99 99
# description: Tomcat startup file
CATALINA_HOME=/home/jnet/tomcat9
[root@localhost bin]# cp /home/jnet/tomcat9/bin/tomcat9 /etc/rc.d/init.d/
[root@localhost bin]# cd /etc/rc.d/init.d/
[root@localhost bin]# chkconfig --add tomcat9

```
* make시 아래 경고 무시
```console
jsvc-unix.c:1311:20: warning: assignment to ‘__sighandler_t’ {aka ‘void (*)(int)’} from incompatible pointer type ‘void (*)(int,  siginfo_t *, void *)’ {aka ‘void (*)(int,  struct <anonymous> *, void *)’} [-Wincompatible-pointer-types]
     act.sa_handler = controller;
```

