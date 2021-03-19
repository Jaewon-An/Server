##### org.apache.catalina.core.AprLifecycleListener.lifecycleEvent SSLEngine을 초기화하지 못했습니다.
AprLifecycleListener.lifecycleEvent Failed to initialize the SSLEngine.   
openssl 재설치 후 tomcat-native 재설치   
yum install openssl*


/etc/rc.d/init.d/tomcat-daemon: line 242: /home/jnet/tomcat9/bin/jsvc: 허가 거부
selinux 문제
