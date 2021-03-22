### SELINUX 끄기
* 현재 상태에서만 보안 설정에 대한 활성화 및 비활성화를 설정 할 수 있다.시스템 재부팅 후 원래 상태로 돌아 온다. (휘발성)   
sestatus: 상태확인, 0: 비활성화, 1:활성화
```console
[root@localhost ~]# sestatus
[root@localhost ~]# setenforce 0
[root@localhost ~]# setenforce 1
```
* 설정 파일 변경(재부팅 필요)   
SELINUX=enforcing (사용함)   
SELINUX=perimssive (보안경고만 사용)   
SELINUX=disabled (사용안함)
```console
[root@localhost ~]# vi /etc/sysconfig/selinu
SELINUX=disabled
```
#### Apache 권한 설정
* http  제한 풀기
```console
[root@localhost ~]# semanage permissive -a httpd_t
```
* httpd의 읽기/쓰기 권한
```console
[root@localhost ~]# chcon -R --type=httpd_sys_rw_content_t /var/www/myweb
```
* The boolean domain_can_mmap_files was set incorrectly 오류 발생 시   
 -P 옵션을 줄 경우 리부팅시에도 적용됨
```console
[root@localhost ~]# setsebool -P domain_can_mmap_files 1
```
* httpd 의 network 연결을 허용하도록 설정(모든포트)   
 -P 옵션을 줄 경우 리부팅시에도 적용됨
```console
[root@localhost ~]# setsebool -P httpd_can_network_connect 1
```
* httpd 의 network 연결을 허용하도록 설정(특정포트)   
설정 후 apache 를 재구동
```console
[root@localhost ~]# semanage port -a -t http_port_t -p tcp 8009
[root@localhost ~]# systemctl restart httpd
```

