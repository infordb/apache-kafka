
## metricbeat 구성 

The Broker, Producer, Consumer metricsets require Jolokia to fetch JMX metrics

| Metricsets    | beta version | JMX(jolokia) | 설정 script                 |
| ------------- | ------------ | ------------ | ------------------------- |
| broker        | Y            | Y            | kafka-server-start.sh     |
| consumer      | Y            | Y            | kafka-console-consumer.sh |
| consumergroup | N            | N            | SASL 접속 (SCRAM 지원 안함)     |
| partition     | N            | N            | SASL 접속 (SCRAM 지원 안함)     |
| producer      | Y            | Y            | kafka-console-producer.sh |



### broker 설정 예시
kafka-server-start.sh 
```
## JMX
export JMX_PORT=9999

## KAFKA_JMX_OPTS
export KAFKA_JMX_OPTS="
-Djava.security.auth.login.config=/data/kafka/kafka_2.13-2.7.0/server_jaas/server_kafka_jaas.conf \
-javaagent:/data/kafka/kafka_2.13-2.7.0/jolokia-1.6.2/agents/jolokia-jvm.jar=port=8778,host=localhost \
-Dcom.sun.management.jmxremote=true \
-Dcom.sun.management.jmxremote.authenticate=false \
-Dcom.sun.management.jmxremote.ssl=false \
-Djava.rmi.server.hostname=localhost \
-Dcom.sun.management.jmxremote.host=localhost \
-Dcom.sun.management.jmxremote.port=9999 \
-Dcom.sun.management.jmxremote.rmi.port=9999 \
-Djava.net.preferIPv4Stack=true"
```
