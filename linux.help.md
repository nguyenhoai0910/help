# tls: failed to verify certificate: x509: certificate 
## 1. first create an empty json fil
```bash
cat << EOF > /etc/docker/daemon.json
{ }
EOF
```
## 2. run the following to add certs
```bash
openssl s_client -showcerts -connect [registry_address]:[registry_port] < /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /etc/docker/certs.d/[registry_address]/ca.crt
```
example:
```log
root@hoainh:/home/hoainh/mssql/docker-compose-mssql# docker-compose up -d
Pulling sqldata (mcr.microsoft.com/mssql/server:)...
ERROR: Get "https://mcr.microsoft.com/v2/": tls: failed to verify certificate: x509: certificate signed by unknown authority
```
```bash
mkdir -p /etc/docker/certs.d/mcr.microsoft.com
openssl s_client -showcerts -connect mcr.microsoft.com:443 < /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /etc/docker/certs.d/mcr.microsoft.com/ca.crt
```