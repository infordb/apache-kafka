
## AKHQ   
</br>
   
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



