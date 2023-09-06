# Zu beachten
- Gateway und DNS bei allen Rechnern: 192.168.110.2. Korrigieren bei vmLP1, vmLS1 und vmLS2
- Rechnernamen (FQDN):
  vmLS1.sam159.iet-gibb.ch
- Search Doamin im netplan-yaml: sam159.iet-gibb.ch
# Mein Setup
Ich habe Tailscale auf meinem Client, sowie auf dem vmLP1 installiert. Somit muss ich VMWareworkstation nicht mehr verwenden. 
# Arbeitsblatt 1
## Lernziele
- Verwaltung eines ADDS mit Samba
- Praktisches Fallbeispiel ist die Basis für eine realitätsnahe Konfiguration
- Verständnis für LDAP
- Kenntnis, wie Windows und Linux Clients in einer Domain eingebunden werden
- Berechtigungen definieren und in einer Domain umsetzten. 
## 2 Arbeitsumgebung
In diesem Auftrag verwenden wir SAMBA. Ab der Version 4.X kann SAMBA als Implementierung des Microsoft ADDS unter Linux verstanden werden. SAMBA ist daher eine echte Alternative zum Microsoft Server. SAMBA wird auch häufig kommerziell für NAS verwendet. 
### Samba als Domain Controller:
- Ab Version 4 kann Samba als ADDC eingesetzt werden
- LDAP als AD backend ist integriert
- Heimdahl Kerberos wird vom KDC für die Authentisierung verwendet. 
### Samba als Domain Member:
- Stellt File- und Printservices zur Verfügung
- Domain Users werden gegenüber dem DC bem Login authentifiziert. 
### Laborumgebung mit Smartlearn
Die Laborumgebung besteht aus dem Realm: **SAM159.IET-GIBB.ch**. 
Folgende VM's werden dafür verwendet:
- vmLS1 --> Domain Controller / KDC, DNS Server, LDAP-Server
- vmLS2 --> Domain Member Server
- vmLP1 --> Domain Member Client
## Vorbereitung des Domain Controllers / KDC
### Samba Doamin Controller vmLS1 installieren
#### 1. Netzwerk konfigurieren
Dieser ganze Punkt kann übersprungen werden, da SmartLearn online verwendet wird. 
#### 2. Rechner Updaten
```Bash
sudo apt update && sudo apt upgrade
```
#### 3. Samba installieren
Die packete können so installiert werden:
```Bash
apt install samba smbclient heimdal-clients
apt install acl attr build-essential libacl1-dev libattr1-dev
apt install libblkid-dev libgnutls28-dev libreadline-dev python-dev
apt install python-dnspython gdb pkg-config libpopt-dev libldap2-dev
apt install libbsd-dev attr krb5-user docbook-xsl libcups2-dev acl ntp ntpdate
apt install net-tools git winbind libpam0g-dev dnsutils lsof
```
