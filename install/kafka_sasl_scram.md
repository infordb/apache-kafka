## kafka 설치  
SASL (SCRAM) 구성


### 1. log(data) directory 생성

mkdir /data/kafka/kafka_2.13-2.7.0/kafka-data


### 2. config file 수정 

server1번 예시임  
broker.id / ip 는 각 서버별로 설정함 

```
broker.id=1
zookeeper.connect=xx.xx.xx.201:2181, xx.xx.xx.202:2181, xx.xx.xx.203:2181/mykafka
listeners=SASL_PLAINTEXT://xx.xx.xx.201:9094
advertised.listeners=SASL_PLAINTEXT://xx.xx.xx.201:9094

log.dir=/data/kafka/kafka_2.13-2.7.0/kafka-data

## add
allow.everyone.if.no.acl.found=false
authorizer.class.name=kafka.security.auth.SimpleAclAuthorizer
super.users=User:kafkabroker;User:client
security.inter.broker.protocol=SASL_PLAINTEXT
sasl.enabled.mechanisms=SCRAM-SHA-256
sasl.mechanism.inter.broker.protocol=SCRAM-SHA-256
zookeeper.set.acl=true
```

####
3. kafka jaas 파일 생성  
####

cat /data/kafka/kafka_2.13-2.7.0/server_jaas/server_kafka_jaas.conf

KafkaServer {
   org.apache.kafka.common.security.scram.ScramLoginModule required
   username="kafkabroker"
   password="kafkabroker-secret";
};
Client {
   org.apache.zookeeper.server.auth.DigestLoginModule required
   username="kafka"
   password="kafka-secret";
};


####
4. jmx enable   
####

vi /data/kafka/kafka_2.13-2.7.0/bin/kafka-server-start.sh

## JMX
export JMX_PORT=9999
## Server JaaS
export KAFKA_OPTS='-Djava.security.auth.login.config=/data/kafka/kafka_2.13-2.7.0/server_jaas/server_kafka_jaas.conf'




######################
# broker 계정 생성  
######################
broker 계정이 생성 안된 상태이기 때문에 zookeeper 접속을 통해 생성 해야 한다. 

client
kafkabroker


##zookeeper start
/data/kafka/kafka_2.13-2.7.0/bin/zookeeper-server-start.sh -daemon /data/kafka/kafka_2.13-2.7.0/config/zookeeper.properties


## 
zookeeper root node 생성 
##
zookeeper-shell.sh 70.60.31.201:2181
create /mykafka
setAcl /mykafka sasl:kafka:cdrwa,world:anyone:r



## zookeeper에게 요청 
export KAFKA_OPTS="-Djava.security.auth.login.config=/data/kafka/kafka_2.13-2.7.0/client_jass/users_jaas_zookeeper.conf"
/data/kafka/kafka_2.13-2.7.0/bin/kafka-configs.sh \
--zookeeper 70.60.31.201:2181/mykafka \
--alter \
--add-config 'SCRAM-SHA-256=[iterations=8192,password=kafkabroker-secret],SCRAM-SHA-512=[password=kafkabroker-secret]' \
--entity-type users \
--entity-name kafkabroker


export KAFKA_OPTS="-Djava.security.auth.login.config=/data/kafka/kafka_2.13-2.7.0/client_jass/users_jaas_zookeeper.conf"
## zookeeper에게 요청 
/data/kafka/kafka_2.13-2.7.0/bin/kafka-configs.sh \
--zookeeper 70.60.31.201:2181/mykafka \
--alter \
--add-config 'SCRAM-SHA-256=[iterations=8192,password=client-secret],SCRAM-SHA-512=[password=client-secret]' \
--entity-type users \
--entity-name client



ls /mykafka/config/users
[client, kafkabroker]

