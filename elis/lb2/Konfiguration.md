***

## Vorbereitung des Domain Controllers / KDC

### Samba Domain Controller installieren

VM: **vmLS1**

>[!tip] 
>Work from `vmLP1` via ssh so copy and paste works.
#### Netzwerk Konfigurieren

1. (a) wurde übersprungen weil [netplan nicht geändert werden sollte](vm%20warning#^4cc64a)
2. (B) anpassen von [`/etc/hosts`](hosts)
3. (C) anpassen von [`/etc/hostname`](hostname)
4. `reboot`

## updates&samba installation
vm: **vmLS1**

```bash
sudo apt update
sudo apt -y upgrade
apt -y install acl attr build-essential libacl1-dev libattr1-dev
apt -y install libblkid-dev libgnutls28-dev libreadline-dev python-dev
apt -y install python-dnspython gdb pkg-config libpopt-dev libldap2-dev
apt -y install libbsd-dev attr krb5-user docbook-xsl libcups2-dev acl ntp ntpdate
apt -y install net-tools git winbind libpam0g-dev dnsutils lsof
mv /etc/samba/smb.conf /etc/samba/smb.conf.orig
```

>[!answer] When asked for stuff from kebreros enter
>```
>vmls1.sam159.iet-gibb.ch
>```
>>[!tip] when accidently skipping the question, just run `sudo dpkg-reconfigure krb5-config
`

