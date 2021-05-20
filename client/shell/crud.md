## Basic CRUD


### 환경설정
```
export KAFKA_HOME=/data/kafka/kafka_2.13-2.7.0
export PATH=$PATH:$KAFKA_HOME/bin
export JAAS_DIR='/data/kafka/kafka_2.13-2.7.0/client_jass'
export BK_LIST='xx.xx.xx.201:9094, xx.xx.xx.202:9094, xx.xx.xx.203:9094'
```


### list topic
```
export KAFKA_OPTS="-Djava.security.auth.login.config=$JAAS_DIR/users_jaas.conf"
kafka-topics.sh \
--bootstrap-server $BK_LIST \
--command-config $JAAS_DIR/client_sasl_only.properties \
--list
```



### describe topic
```
export KAFKA_OPTS="-Djava.security.auth.login.config=$JAAS_DIR/users_jaas.conf"
kafka-topics.sh \
--bootstrap-server $BK_LIST \
--command-config $JAAS_DIR/client_sasl_only.properties \
--describe \
--topic mytopic
```


### delete topic
```
export KAFKA_OPTS="-Djava.security.auth.login.config=$JAAS_DIR/users_jaas.conf"
kafka-topics.sh \
--bootstrap-server $BK_LIST \
--command-config $JAAS_DIR/client_sasl_only.properties \
--delete \
--topic GameLog
```



### create topic 
```
export KAFKA_OPTS="-Djava.security.auth.login.config=$JAAS_DIR/users_jaas.conf"
kafka-topics.sh \
--bootstrap-server $BK_LIST \
--command-config $JAAS_DIR/client_sasl_only.properties \
--create \
--replication-factor 2 \
--partitions 1 \
--topic GameLog
```


### producer

```
kafka-console-producer.sh \
--broker-list $BK_LIST \
--producer.config $JAAS_DIR/client_sasl.properties \
--topic GameLog \
```


### consumer

#### use group.id
```
kafka-console-consumer.sh \
--bootstrap-server $BK_LIST \
--consumer.config $JAAS_DIR/client_sasl.properties \
--topic GameLog  \
--from-beginning \
--consumer-property group.id=mytopic-group
```



#### consumer  for test
```
kafka-console-consumer.sh \
--bootstrap-server $BK_LIST \
--consumer.config $JAAS_DIR/client_sasl.properties \
--topic connect-status \
--from-beginning
```



