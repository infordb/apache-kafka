| Metricsets    | beta version | JMX(jolokia) | 설정 script                 |
| ------------- | ------------ | ------------ | ------------------------- |
| broker        | Y            | Y            | kafka-server-start.sh     |
| consumer      | Y            | Y            | kafka-console-consumer.sh |
| consumergroup | N            | N            | SASL 접속 (SCRAM 지원 안함)     |
| partition     | N            | N            | SASL 접속 (SCRAM 지원 안함)     |
| producer      | Y            | Y            | kafka-console-producer.sh |
