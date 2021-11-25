## Secure your MongoDB connections - SSL/TLS

### 1. Creating own SSL CA to dump our self-signed certificate (client)

Create the private root key and self-sign certificate
```
$openssl genrsa -out rootCA.key 2048
$openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.pem

Country Name (2 letter code) [AU]:TH
State or Province Name (full name) [Some-State]:Bangkok
Locality Name (eg, city) []:Bangkok
Organization Name (eg, company) [Internet Widgits Pty Ltd]:SCK
Organizational Unit Name (eg, section) []:DEV
Common Name (e.g. server FQDN or YOUR name) []:Data
Email Address []:xxx@xxx.com
```

### Create A Certificated
```
$openssl genrsa -out mongodb.key 2048
$openssl req -new -key mongodb.key -out mongodb.csr
$openssl x509 -req -in mongodb.csr -CA rootCA.pem -CAkey rootCA.key -CAcreateserial -out mongodb.crt -days 500 -sha256
```

Create pem file
```
$cat mongodb.key mongodb.crt > mongodb.pem
```

### 2. Start server with TLS
```
$mkdir -p data/rs1
$mongod -f node1.conf
```

### 3. Connect to server
```
$mongo --tls --tlsCAFile rootCA.pem --tlsCertificateKeyFile mongodb.pem --port 27001 --sslAllowInvalidHostnames
```

