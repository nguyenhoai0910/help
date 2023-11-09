# 1.Tls: failed to verify certificate: x509: certificate 
## First create an empty json fil
```bash
cat << EOF > /etc/docker/daemon.json
{ }
EOF
```
## Run the following to add certs
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
```bash
mkdir -p /etc/docker/certs.d/www.scootersoftware.com
openssl s_client -showcerts -connect www.scootersoftware.com:443 < /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /etc/docker/certs.d/www.scootersoftware.com/ca.crt
```

# 2. Mount window folder nfs to linux
```bash
sudo mount -t cifs //10.96.59.18/trulyonline/CTQT_Pilot . -o username=trulyonline
```
enterpassword:
 
