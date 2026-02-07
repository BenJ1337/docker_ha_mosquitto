# Mosquitto

https://github.com/BenJ1337/docker_ha_home-assistant-bootstrap

## Authentication methods
https://mosquitto.org/documentation/authentication-methods/

### Password-based Authentication
- [mosquitto_passwd](https://mosquitto.org/man/mosquitto_passwd-1.html)

1. Add a new user
```
# docker run -it --rm  -v ./:/mosquitto/config/ eclipse-mosquitto:2.0 mosquitto_passwd -b /mosquitto/config/passwd.txt "user1" "pa55w0rd"
# chmod o-r passwd.txt
# chown root:root passwd.txt
```

2. mosquitto.conf
```
password_file /mosquitto/config/passwd.txt
```

3. docker-compose.yaml
```yaml
services:
  ha-msqtt:
    volumes:
      - ${PASSWD_DIR:-.}/passwd.txt:/mosquitto/config/passwd.txt:ro
```

4. Custom path to passwd.txt (Optional)
./.env
```
PASSWD_DIR=/path/to/passwd.txt
```

### Certificate-based Authentication
1. Create a Certificate Authority (Optional)
```bash
openssl genrsa -out ./ca.key.pem 4096
openssl req -x509 -new -nodes -key ca.key.pem -sha256 -days 3650 -out ca.crt.pem
```

2. Create a Client Certificate
```bash
openssl genrsa -out mTLS/client.key.pem 4096
openssl req -new -key mTLS/client.key.pem -out mTLS/client.csr -subj "/CN=TestClient/C=DE/ST=Bayern/L=Landshut/O=None"
openssl x509 -req -in mTLS/client.csr -CA ca.crt.pem -CAkey ca.key.pem -CAcreateserial -out mTLS/client.crt.pem -days 365 -sha2561~openssl genrsa -out mTLS/client.key.pem 4096
openssl req -new -key mTLS/client.key.pem -out mTLS/client.csr -subj "/CN=TestClient/C=DE/ST=Bayern/L=Landshut/O=None"
openssl x509 -req -in mTLS/client.csr -CA ca.crt.pem -CAkey ca.key.pem -CAcreateserial -out mTLS/client.crt.pem -days 365 -sha256
```

3. mosquitto.conf
```
require_certificate true
use_identity_as_username true
cafile /certs/ca.crt.pem
```

4. docker-compose.yaml
```
services:
  ha-msqtt:
    volumes:
      - ${CERT_DIR:-.}/certs/:/certs/:ro
```

5. Custom path to client key pair (Optional)
./.env
```
CERT_DIR=/path/to/client/key-pair/
```
