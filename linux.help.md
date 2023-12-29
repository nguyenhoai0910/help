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
 
## 3. tar and remove file log
```bash
tar -cf files.tar my_directory --remove-files
```
example:
```log
tar -cf monitor.sh.202310.tar monitor.sh.202310* --remove-files
-f, --file=ARCHIVE         use archive file or device ARCHIVE
```
## 4. NTP client & /etc/chrony.conf
Config:
```bash
[soadmin@A1B-PCIBBONDS-API1 ~]$ cat /etc/chrony.conf
# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (https://www.pool.ntp.org/join.html).
#pool 2.pool.ntp.org iburst
server DC-ADDS10.ocb.vn iburst
server DR-ADDS10.ocb.vn iburst

# Use NTP servers from DHCP.
sourcedir /run/chrony-dhcp
```

Command line check:
```bash
chronyc sources -v
chronyc sourcestats
chronyc tracking
timedatectl
hwclock --systohc           // Set đồng bộ thời gian cho đồng hồ của BIOS (Đồng hồ phần cứng) 
```


```bash
[soadmin@A1B-PCIBBONDS-API1 ~]$ chronyc sources -v

  .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
 / .- Source state '*' = current best, '+' = combined, '-' = not combined,
| /             'x' = may be in error, '~' = too variable, '?' = unusable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^* ip-10-96-150-10.ap-south>     3   6   377    36  +1796us[+1816us] +/-   57ms
^+ ip-10-97-150-10.ap-south>     4   6   377    33  -1598us[-1598us] +/-  104ms

[soadmin@A1B-PCIBBONDS-API1 ~]$ chronyc sourcestats
Name/IP Address            NP  NR  Span  Frequency  Freq Skew  Offset  Std Dev
==============================================================================
ip-10-96-150-10.ap-south>   9   5   516     +0.864      3.819  +1369us   357us
ip-10-97-150-10.ap-south>  14  11   655     -0.122      1.011  -2424us   185us


[soadmin@A1B-PCIBBONDS-API1 ~]$ chronyc tracking
Reference ID    : 0A60960A (ip-10-96-150-10.ap-southeast-1.compute.internal)
Stratum         : 4
Ref time (UTC)  : Fri Dec 29 04:43:51 2023
System time     : 0.000154608 seconds fast of NTP time
Last offset     : +0.000020182 seconds
RMS offset      : 0.003842033 seconds
Frequency       : 46.232 ppm fast
Residual freq   : +0.028 ppm
Skew            : 2.849 ppm
Root delay      : 0.057150137 seconds
Root dispersion : 0.027695538 seconds
Update interval : 64.8 seconds
Leap status     : Normal
```