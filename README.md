# apache-kafka 설치

## authentication & encryption

| 구분        | 인증\_암호화 방식      | authentication    | encryption               | authorization 대상 식별 |
| --------- | --------------- | ----------------- | ------------------------ | ------------------- |
| kafka     | SASL\_PLAINTEXT | SASL (SCRAM)      | none                     | SASL (ID)           |
| SASL\_SSL | SASL (SCRAM)    | SSL               | SASL (ID)                |
| SSL       | SSL             | SSL               | SSL (Distinguished Name) |
| PLAINTEXT | none            | none              |                          |
| zookeeper | SASL\_PLAINTEXT | SASL (DIGEST-MD5) | none                     | SASL (ID)           |
