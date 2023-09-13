***
[M159Thema2-AB01](elis/lb2/kw37/M159Thema2-AB01.pdf)
## Vorbereitung des Domain Controllers / KDC

### Samba Domain Controller installieren

VM: **vmLS1**

>[!tip] 
>Work from `vmLP1` via ssh so copy and paste works.

>[!warning]
>Because name resolution does not work, you need to ssh with the ip `192.168.110.61`.
>For that to work run `ssh-keygen -f "/home/vmadmin/.ssh/known_hosts" -R "192.168.110.61"`
#### Netzwerk Konfigurieren

>[!error] Watch so the correct vm is used unlike me.

1. (a) wurde übersprungen weil [netplan nicht geändert werden sollte](vm%20warning#^4cc64a)
2. (B) anpassen von [`/etc/hosts`](hosts)
3. (C) anpassen von [`/etc/hostname`](hostname)
4. (D) datei [/etc/netplan/00-eth0](00-eth0.yaml) bearbeiten
   `netplan apply`
5. `reboot`


## updates
vm: **vmLS1**

```bash
sudo apt update
sudo apt -y upgrade
```

## Installation von packages:

```bash
sudo apt install samba smbclient heimdal-clients
```

```bash
sudo apt install acl attr build-essential libacl1-dev libattr1-dev
```

```bash
sudo apt install libblkid-dev libgnutls28-dev libreadline-dev python-dev
```

```bash
sudo apt install python-dnspython gdb pkg-config libpopt-dev libldap2-dev
```

```bash
sudo apt install libbsd-dev attr krb5-user docbook-xsl libcups2-dev acl ntp ntpdate
```

```bash
sudo apt install net-tools git winbind libpam0g-dev dnsutils lsof
```


>[!answer] When asked for stuff from kebreros enter
>```
>vmls1.sam159.iet-gibb.ch
>```
>>[!tip] when accidently skipping the question, just run `sudo dpkg-reconfigure krb5-config
`


### copy old config file:

```bash
mv /etc/samba/smb.conf /etc/samba/smb.conf.orig
```


### Samba als kdc aufsetzen:

>[!error] Bitte nicht 15 minuten verschwenden und als root ausführen.

```bash
samba-tool domain provision
```

>[!info] Realm default wert nehmen (kopiere eingeklammerten text)

>[!info] Bei Domain auch Default wert aus klammern kopieren.

>[!info] Server Role auch default eingeklammertes kopieren

>[!Iinfo]
>Bei `DNS backend` gib `SAMBA_INTENRAL` an.

>[!info] bei `DNS forwarder IP address` gib `8.8.8.8` an.

>[!info] Password
>`Pa$$w0rd`


>[!success] output
>![](Pasted%20image%2020230913120720.png)


### Service stoppen

```bash
systemctl disable systemd-resolved
systemctl stop systemd-resolved
```

### Config smb.conf kontrollieren:

![](Pasted%20image%2020230913120902.png)

### autostart von samba:

```bash
systemctl unmask samba-ad-dc 
systemctl enable samba-ad-dc
systemctl start samba-ad-dc
reboot
```

```bash
systemctl status samba-ad-dc
```

