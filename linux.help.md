# Linux help
[1. Tls: failed to verify certificate: x509: certificate](#1-tls-failed-to-verify-certificate-x509-certificate) <br/>
[2. Mount window folder nfs to linux](#2-mount-window-folder-nfs-to-linux) <br/>
[3. tar and remove file log](#3-tar-and-remove-file-log) <br/>
[4. NTP client & /etc/chrony.conf](#4-ntp-client--etcchronyconf) <br/>
[5. Generate a public/private key pair with OpenSSH](#5-generate-a-publicprivate-key-pair-with-openssh) <br/>
[6. Remove package and delete all ](#6-remove-package-and-delete-all) <br/>
[7. encrypt any file or directory with Mcrypt ](#7-encrypt-any-file-or-directory-with-mcrypt)  <br/>

## 1. Tls: failed to verify certificate: x509: certificate 
### First create an empty json fil
```bash
cat << EOF > /etc/docker/daemon.json
{ }
EOF
```
### Run the following to add certs
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

## 2. Mount window folder nfs to linux
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
sudo timedatectl set-ntp true 
sudo timedatectl set-timezone Asia/Ho_Chi_Minh
# rm -f /etc/localtime
# ln -s /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime

sudo apt install chrony

[soadmin@A1B-PCIBBONDS-API1 ~]$ cat /etc/chrony.conf
# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (https://www.pool.ntp.org/join.html).
#pool 2.pool.ntp.org iburst
server DC-ADDS10.ocb.vn iburst
server DR-ADDS10.ocb.vn iburst
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

## 5. Generate a public/private key pair with OpenSSH
```bash
ssh-keygen -t ed25519 -C "your_comment_see_below"
```
example:
```bash
sudo mkdir -p /usr/bin/sshkey/
ssh-keygen -t ed25519 -C "pc.ssh"
```
Your private key in ~/.ssh/id_ed25519 \
Your public key in ~/.ssh/id_ed25519.pub

If you forgot to add a password to your private key or if you want to change the password later on, you can add a (new) password to your existing private key with:
```bash
ssh-keygen -p -f ~/.ssh/id_ed25519
```
Copy ssh key pub server remote:
```bash
ssh-copy-id -i [ssh-key-location] [username]@[server-ip-address]
ssh-copy-id -i /home/soadmin12/.ssh/id_ed25519.pub ocboper@10.98.42.41
```
SSH with private key:
```bash
ssh -i '/path/to/keyfile' username@server
ssh -i '/home/soadmin12/.ssh/id_ed25519' ocboper@10.98.42.41
```

## 6. Remove package and delete all  
```
sudo apt-get remove --purge packagename
```

## 7. encrypt any file or directory with Mcrypt
https://linuxconfig.org/how-to-easily-encrypt-any-file-or-directory-with-mcrypt-on-linux-system  <br/>
UBUNTU/DEBIAN
```
sudo apt install mcrypt -y
```
oathtool --totp="type-totp" --digits=<digit 6 or 8> <code>

zip -e OTP.PAM.zip OTP.PAM.sh
unzip <name-file>

mcrypt -a wake OTP.PAM.sh
mcrypt -d <name-file> 

## 8. ####
