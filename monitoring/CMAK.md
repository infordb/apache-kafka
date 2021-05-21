## Cluster Manager for Apache Kafka

### 이슈

1. 2.4.0 까지 공식지원 / 2.7.0 에서도 정상 동작은 함
2. kafka jaas 설정은 있으나, zookeeper jaas 설정음 없음   
   https://github.com/yahoo/CMAK/issues/467
3. Topic 생성 시 zookeeper와 통신함, zookeeper 통해서 토픽 생성 시 zoo node에 권한 설정 안됨


### zookeeper jaas 설정 임시
cmak start할때 zookeeper jaas설정 넘기면 kafka 멀티 클러스터 관리를 못함  
(cluster 마다 zookeeper 설정이 다를 수 있음으로.)

cat /path/users_jaas_zookeeper.conf
```
Client {
   org.apache.zookeeper.server.auth.DigestLoginModule required
   username="kafka"
   password="kafka-secret";
};
```

start command
```
nohup /data/cmak/cmak-3.0.0.5/bin/cmak \
-Dconfig.file=/data/cmak/cmak-3.0.0.5/conf/application.conf \
-Dhttp.port=8080 \
-Djava.security.auth.login.config=/path/users_jaas_zookeeper.conf &
```
 
### Topic 생성 이슈



* cmka로 토픽을 생성하면 kafka level에서 권한이 모두 풀려서 생성됨
* zookeeper shell에서 생성해도 동일함, kafka tool에서만 jaas로 로그인했을때 권한 설정함 
```
getAcl /mykafka/config/topics/temp-new
'world,'anyone
: cdrwa
```

* topic.sh tool로 생성 시 zoo node 권한 설정 됨
```
getAcl /mykafka/config/topics/GameLog
'sasl,'kafka
: cdrwa
'world,'anyone
: r
```
### kafka jaas 설정 예시
![image](https://user-images.githubusercontent.com/10610884/119089425-12737b80-ba45-11eb-9182-60de3495400d.png)

