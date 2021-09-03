
## AKHQ   
</br>

### download
https://github.com/tchiotludo/akhq

### config   
 application-test.yml
```
micronaut:
  server:
    port: 8088

akhq:
  connections:
    my-cluster-sasl:
      properties:
        bootstrap.servers: "$broker_ip1:9094, $broker_ip2:9094, $broker_ip3:9094"
        security.protocol: SASL_PLAINTEXT
        sasl.mechanism: SCRAM-SHA-256
        sasl.jaas.config: org.apache.kafka.common.security.scram.ScramLoginModule required username="client" password="client-secret";

      connect:
        - name: connect-1
          url: "http://$connect_rest_url:8083"
          basic-auth-username: connectadmin
          basic-auth-password: connectadmin
```









### Start Script
```
nohup java -Dmicronaut.config.files=/data/akhq-0.18.0/application-test.yml -jar /data/akhq-0.18.0/akhq.jar >> akhq.log 2>&1 &
```

### 인증 설정

```
micronaut:
  server:
    port: 8088
  security:
    enabled: true
    token:
      jwt:
        signatures:
          secret:
            generator:
              secret: secret01234567890ABCDEFGHIJKLMNO  ## 32자 이상 
akhq:
  connections:
    my-cluster-sasl:
      properties:
        bootstrap.servers: "70.60.31.201:9094, 70.60.31.202:9094, 70.60.31.203:9094"
        security.protocol: SASL_PLAINTEXT
        sasl.mechanism: SCRAM-SHA-256
        sasl.jaas.config: org.apache.kafka.common.security.scram.ScramLoginModule required username="client" password="client-secret";
      connect:
        - name: connect-1
          url: "http://70.60.31.201:8083"
          basic-auth-username: connectadmin
          basic-auth-password: connectadmin
  security:
    default-group: no-roles	
    basic-auth:
      - username: admin
        password: "5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8"
        passwordHash: SHA256
        groups:
        - admin
      - username: reader
        password: "5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8"
        groups:
        - reader
 ```
 
 *sha256 password 생성*
```
echo -n "password" | sha256sum	
```
