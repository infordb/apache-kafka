
## jaas 파일 생성


### 환경 설정
```
export JAAS_DIR='/root/test'
export JAAS_BK_USER='client'
export JAAS_BK_PW='client-secret'
export JAAS_ZK_USER='kafka'
export JAAS_ZK_PW='kafka-secret'

## scrame 인 경우
#export LoginModule='org.apache.kafka.common.security.scram.ScramLoginModule required'
#export mechanism='SCRAM-SHA-256'

## plain 경우 
export LoginModule='org.apache.kafka.common.security.plain.PlainLoginModule required'
export mechanism='PLAIN'
```

###  property

```

cat << EOF > $JAAS_DIR/client_sasl.properties
security.protocol=SASL_PLAINTEXT
sasl.mechanism=SCRAM-SHA-256
sasl.jaas.config="$LoginModule" \
    username="$JAAS_BK_USER" \
    password="$JAAS_BK_PW";
EOF



cat << EOF > $JAAS_DIR/client_sasl_only.properties
security.protocol=SASL_PLAINTEXT
sasl.mechanism=$mechanism
EOF
```


### jaas
```
cat << EOF > $JAAS_DIR/users_jaas.conf
KafkaClient {
  $LoginModule
  username="$JAAS_BK_USER"
  password="$JAAS_BK_PW";
};
Client {
   org.apache.zookeeper.server.auth.DigestLoginModule required
   username="$JAAS_ZK_USER"
   password="$JAAS_ZK_PW";
};
EOF



cat << EOF > $JAAS_DIR/users_jaas_kafka.conf
KafkaClient {
  $LoginModule
  username="$JAAS_BK_USER"
  password="$JAAS_BK_PW";
};
EOF


cat << EOF > $JAAS_DIR/users_jaas_zookeeper.conf
Client {
   org.apache.zookeeper.server.auth.DigestLoginModule required
   username="$JAAS_ZK_USER"
   password="JAAS_ZK_PW";
};
EOF

```
