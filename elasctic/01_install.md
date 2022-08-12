### 0. 리눅스 설정

<hr/>

* /etc/security/limits.conf
  ```console
  # vi /etc/security/limits.conf
  jnet soft nofile 65536         
  jnet hard nofile 65536
  jnet soft nproc 65536         
  jnet hard nproc 65536
  jnet soft memlock unlimited         
  jnet hard memlock unlimited
  ```

  

* /etc/sysctl.conf
  ```console
  # vi /etc/sysctl.conf
  vm.max_map_count=262144
  
  # sysctl -p
  ```

  

### 1. 설치

<hr/>

[엘라스틱 다운로드 페이지](https://www.elastic.co/kr/downloads/elasticsearch)

```console
[jnet@localhost ~]$ mkdir ~/server/down
[jnet@localhost ~]$ cd ~/server/down
[jnet@localhost down]$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.3.3-linux-x86_64.tar.gz
[jnet@localhost down]$ tar -zxvf elasticsearch-8.3.3-linux-x86_64.tar.gz
[jnet@localhost down]$ cd ../elasticsearch8
[jnet@localhost elasticsearch8]$
```



### 2. 엘라스틱 설정

<hr/>

* elasticsearch.yml
  

  ```console
  # ======================== Elasticsearch Configuration =========================
  
  # ---------------------------------- Cluster -----------------------------------
  #
  # 클러스터들은 같은 클러스터의 이름을 가진 경우 묶을 수 있는데 여기서 지정을 할 수 있다. 
  #
  cluster.name: kolaseek-el
  
  # ------------------------------------ Node ------------------------------------
  #
  # 엘라스틱서치의 노드명을 설정한다. 노드명을 지정하지 않으면 임의로 지정하기 때문에 웬만해서는 짓는 것이 좋다.
  #
  node.name: node-1
  
  # ----------------------------------- Paths ------------------------------------
  #
  # 인덱스 경로를 지정한다. 설정하지 않는 경우 엘라스틱서치 폴더의 data 디렉토리에 인덱스를 생성하며, 멀티 경로일 경우 comma를 사용한다.
  #
  #path.data: /path/to/data
  
  #
  # 엘라스틱서치의 노드 및 클러스터에서 생성되는 로그의 저장 경로를 지정한다.
  #
  #path.logs: /path/to/logs
  
  
  # ----------------------------------- Memory -----------------------------------
  #
  # 엘라스틱서치가 선점한 메모리를 다른 Java 프로그램이 사용하지 못하도록 Lock을 하는 기능으로 True를 권장한다
  #
  bootstrap.memory_lock: true
  
  # ---------------------------------- Network -----------------------------------
  #
  # 지정된 IP만 엘라스틱 서치에 접근할 수 있도록 설정한다. 
  # 단일 아이피 지정
  #network.host: 192.168.0.1
  # 여러대 접근 지정
  #network.host: [192.168.0.1, 192.168.0.2, ... ]
  # 모든 아이피 허용
  network.host: 0.0.0.0
  
  #
  # 클라이언트와 통신할 Port 번호를 설정한다
  #
  http.port: 9200
  
  #
  # 엘라스틱 노드들 끼리 서로 통신하기 위한 포트를 설정한다. 디폴트는 9300 이다.
  #
  #transport.port: 9300
  
  
  # --------------------------------- Discovery ----------------------------------
  #
  # 활성화된 다른 서버를 찾는 옵션으로 같은 클러스터로 묶인 노드의 IP 혹은 도메인 주소명을 지정하면 된다
  # The default list of hosts is ["127.0.0.1", "[::1]"]
  #
  #discovery.seed_hosts: ["host1", "host2"]
  
  
  # 마스터로 선출될 수 있는 노드들을 지정한다. 
  #
  #cluster.initial_master_nodes: ["node-1", "node-2"]
  
  
  # ---------------------------------- Various -----------------------------------
  #
  # Allow wildcard deletion of indices:
  #
  #action.destructive_requires_name: false
  
  #----------------------- BEGIN SECURITY AUTO CONFIGURATION -----------------------
  #
  # The following settings, TLS certificates, and keys have been automatically      
  # generated to configure Elasticsearch security features on 11-08-2022 04:40:42
  #
  # --------------------------------------------------------------------------------
  
  # Enable security features
  xpack.security.enabled: false
  
  xpack.security.enrollment.enabled: false
  
  # Enable encryption for HTTP API client connections, such as Kibana, Logstash, and Agents
  xpack.security.http.ssl:
    enabled: false
    keystore.path: certs/http.p12
  
  # Enable encryption and mutual authentication between cluster nodes
  xpack.security.transport.ssl:
    enabled: false
    verification_mode: certificate
    keystore.path: certs/transport.p12
    truststore.path: certs/transport.p12
  #----------------------- END SECURITY AUTO CONFIGURATION -------------------------
  
  ```



### 3. JVM 설정(jvm.options.d)

<hr/>

```console
[jnet@localhost elasticsearch8]$ cd config/jvm.options.d
[jnet@localhost jvm.options.d]$ vi user.options
-Xms2g
-Xmx2g
[jnet@localhost jvm.options.d]$
```



### 4. 실행

<hr/>

* 기본 명령어
  ```console
  [jnet@localhost elasticsearch8]$ cd bin
  [jnet@localhost bin]$ elasticsearch
  ```

  

* startup.sh

  ```console
  #!/bin/bash
  
  ES_HOME=/home/jnet/server/elasticsearch8
  PID_DIR=$ES_HOME/bin
  $ES_HOME/bin/elasticsearch -d -s -p $PID_DIR/elasticsearch8.pid
  
  ```

* shutdown.sh
  ```console
  #!/bin/bash
  
  ES_HOME=/home/jnet/server/elasticsearch8
  PID_DIR=$ES_HOME/bin
  $ES_HOME/bin/elasticsearch -d -s -p $PID_DIR/elasticsearch8.pid
  
  ```

  

### 5. 서비스 등록(systemctl) - 미완성

<hr/>

* /lib/systemd/system/elasticsearch8.service 편집

  ```console
  # vi /lib/systemd/system/elasticsearch8.service
  
  [Unit]
  Description=Elasticsearch8
  Documentation=https://www.elastic.co/kr/products/elasticsearch
  Wants=network-online.target
  After=network-online.target
  
  [Service]
  RuntimeDirectory=elasticsearch8
  WorkingDirectory=/home/jnet/server/elasticsearch8/bin
  
  LimitMEMLOCK=unlimited
  LimitNOFILE=65535
  LimitNPROC=65536
  
  ExecStart=/home/jnet/server/elasticsearch8/bin/elasticsearch
  ExecReload=/home/jnet/server/elasticsearch8/bin/elasticsearch
  RestartSec=3
  
  User=jnet
  Group=jnet
  
  [Install]
  WantedBy=multi-user.target
  ```

* service 파일 수정사항 적용 및 부팅 시 elasticsearch 자동 실행 활성화
  ```console
  
  # systemctl daemon-reload
  # systemctl enable elasticsearch8
  # systemctl start elasticsearch8
  # systemctl stop elasticsearch8
  ```

  
