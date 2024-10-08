# Window help
[1. Switch Command Prompt and PowerShell on the Win+X Menu](#1-switch-command-prompt-and-powershell-on-the-winx-menu) <br/>
[2. CHECK permission user](#2-check-permission-user) <br/>
[3. System has not been booted with systemd as init system (PID 1)](#3-system-has-not-been-booted-with-systemd-as-init-system-pid-1) <br/>
[4. Check MD5 powerchell](#4-check-md5-file-zip) <br/>
[5. Remove entries in the Windows Remote Desktop Connection client](#5-remove-entries-in-the-windows-remote-desktop-connection-client) <br/>
## 1. Switch Command Prompt and PowerShell on the Win+X Menu
- Open Windows Settings by pressing Win+I. Select Personalization.
- From the Personalization applet, select Taskbar. 
- Select the option to Replace Command Prompt with Windows PowerShell in the menu when I right-click the start button or press Windows key+X. 
- Select OK, and then close Windows Settings. 

## 2. CHECK permission user
icacls folder/file name

## 3. System has not been booted with systemd as init system (PID 1).
System has not been booted with systemd as init system (PID 1). Can't operate. Failed to connect to bus: Host is down.
```
sudo -e /etc/wsl.conf
[boot]
systemd=true
```
```
wsl -t Ubuntu-redmine
wsl -d Ubuntu-redmine
```
```
useradd -m -G sudo -s /bin/bash "hoainh"
hoainh ALL=(ALL) NOPASSWD: ALL
%sudo  ALL=(ALL) NOPASSWD: ALL
```

## 4. Check md5 file zip
all file:
```
powershell "Get-FileHash -Algorithm MD5 -Path (Get-ChildItem "*" -Recurse) |Format-Table Algorithm,Hash,PATH "
```
zip:
```
powershell "Get-FileHash -Algorithm MD5 -Path (Get-ChildItem "*.zip" -Recurse) |Format-Table Algorithm,Hash,PATH "
```

## 5. Remove entries in the Windows Remote Desktop Connection client
```
>> start Registry Editor
>> HKEY_CURRENT_USER\Software\Microsoft\Terminal Server Client\Default
>> remove entries
```
