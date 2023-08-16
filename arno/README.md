# m159
## Samba installieren (Arbeitsblatt 1)
Samba kann auf Ubuntu mit `apt install samba smbclient heimdal-clients` installiert werden. Es wird danach der Kerberos und Administrative Server abgefragt. Dieser ist in unserem Fall vmls1.sam159.iet-gibb.ch.


Weitere Abhängigkeiten können mit 
```apt install acl attr build-essential libacl1-dev libattr1-dev
apt install libblkid-dev libgnutls28-dev libreadline-dev python-dev
apt install python-dnspython gdb pkg-config libpopt-dev libldap2-dev
apt install libbsd-dev attr krb5-user docbook-xsl libcups2-dev acl ntp ntpdate
apt install net-tools git winbind libpam0g-dev dnsutils lsof
```
installiert werden.
Um das Originale Samba Config File zu sichern kann der Befehl `mv /etc/samba/smb.conf /etc/samba/smb.conf.orig` verwendet werden.

Samba Domain erstellen mit `samba-tool domain provision`

Systemd DNS-Resolver deaktivieren, da jetzt Samba diesen Job übernimmt `sudo systemctl disable systemd-resolved --now`

Samba-Service mit
```
systemctl unmask samba-ad-dc
systemctl enable --now samba-ad-dc
killall smbd
killall winbindd
```
starten.

Aus irgend einem Grund ist der Dienst beim starten immer mit der Fehlermeldung, dass smbd und winbindd schon läuft gecrasht. Nachdem diese Prozesse gekillt wurden, konnte der Dienst gestartet werden.

Mit `ln -sf /var/lib/samba/private/krb5.conf /etc/krb5.conf` die krb5.conf von Samba nach /etc Symlinken.
```
[realms]
 SAM159.IET-GIBB.CH = {
  kdc = vmls1.sam159.iet-gibb.ch
  default_domain = sam159.iet-gibb.ch
}
```
in `/etc/krb5.conf` ergänzen.

Damit ich `samba_dnsupdate --verbose` ausführen konnte, musste ich aus irgend einem Grund die IP 127.0.0.53 zu 127.0.0.1 im File `/etc/resolv.conf` erstetzen.

Reverse DNS Zone erstellen 
```
samba-tool dns zonecreate vmls1 220.168.192.in-addr.arpa -Uadministrator
samba-tool dns add 192.168.220.10 220.168.192.in-addr.arpa 10 PTR vmls1.sam159.iet-gibb.ch -Uadministrator
```
Mit `smbclient -L localhost - Uadministrator` die Verbindung testen

### Aufgaben
* Ticket für den User administrator lösen: `kinit administrator`
* Verbindumg mit `smbclient -L vmls1 -k` testen. Der Parameter -k bewirkt laut Manual
```
-k|--kerberos
           Try to authenticate with kerberos. Only useful in an Active Directory environment.
```
das die Kerberos Authentizierung verwendet wird.
* Der Credential-Cache kann mit `klist` ausgegeben werden. Er sieht wie folgt aus:
```
Ticket cache: FILE:/tmp/krb5cc_0
Default principal: administrator@SAM159.IET-GIBB.CH

Valid starting       Expires              Service principal
11/04/2021 14:16:10  11/05/2021 00:16:10  krbtgt/SAM159.IET-GIBB.CH@SAM159.IET-GIBB.CH
	renew until 11/05/2021 14:16:06
11/04/2021 14:17:04  11/05/2021 00:16:10  cifs/vmls1@SAM159.IET-GIBB.CH
```
* `smbclient -L localhost -k` funktioniert nicht, da localhost kein Kerberos Principal ist. Siehe Fehlermeldung
```
Kerberos auth with 'administrator@SAM159.IET-GIBB.CH' (SAM159\root) to access 'localhost' not possible
session setup failed: NT_STATUS_ACCESS_DENIED
```
* Passwort komplexität deaktivieren
```
samba-tool domain passwordsettings set --complexity=off
samba-tool domain passwordsettings set --history-lengt=0
samba-tool domain passwordsettings set --min-pwd-age=0
samba-tool domain passwordsettings set --max-pwd-age=0
samba-tool user setexpiry administrator --noexpiry
```
* A Records mit samba-tool hinzufügen
```
samba-tool dns add vmls1.sam159.iet-gibb.ch sam159.iet-gibb.ch vmls2 A 192.168.220.11 -Uadministrator
samba-tool dns add vmls1.sam159.iet-gibb.ch sam159.iet-gibb.ch vmlp1 A 192.168.210.30 -Uadministrator
```
* PTR Records hinzufügen
```
samba-tool dns add vmls1.sam159.iet-gibb.ch 220.168.192.in-addr.arpa vmls2 PTR 11 -Uadministrator
```
* `samba-tool fsmo show` Zeigt laut Manual die Rollen an. Der Output sieht wie folgt aus:
```
SchemaMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
InfrastructureMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
RidAllocationMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
PdcEmulationMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
DomainNamingMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
DomainDnsZonesMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
ForestDnsZonesMasterRole owner: CN=NTDS Settings,CN=VMLS1,CN=Servers,CN=Default-First-Site-Name,CN=Sites,CN=Configuration,DC=sam159,DC=iet-gibb,DC=ch
```
## Arbeitsblatt 2 (LAM Manager)
LAM (LDAP Account Manager) installieren

sudo apt-get install smbldap-tools
sudo apt install ldap-account-manager

Im Browser LAM aufrufen
http://192.168.220.10/lam/
User: Administrator
PW: SmL12345**

Im Loginfenster rechts oben auf LAM Configuration klicken -> edit server profiles -> manage Server Profiles
Eingabe Add profile
Profile Name: sam159Domain
Password: sml12345

![image](https://user-images.githubusercontent.com/50365065/142415469-fb979f7c-acf9-42aa-909e-de78c4a8f649.png)
![image](https://user-images.githubusercontent.com/50365065/142415485-d0854346-7183-49c1-bd1b-f3cee65b6653.png)
![image](https://user-images.githubusercontent.com/50365065/142415499-af67074d-2e2f-4894-b188-49ca7ca8b94f.png)

In smb.conf datei noch :ldap server require strong auth = no eintragen, da man kein komplexes Passwort nehmen muss.

Wenn man eingeloggt ist, kann man Tree view oben rechts gehen und dann hat man eine Übersicht über die AD
Objekte

![image](https://user-images.githubusercontent.com/50365065/142415565-be35f7c6-6776-4699-a840-979f35ffd939.png)
![image](https://user-images.githubusercontent.com/50365065/142415601-e17218ec-ced3-45a2-9d3c-c34b38dd2816.png)
![image](https://user-images.githubusercontent.com/50365065/142415609-ed9223c9-8179-40c9-b18a-53b78ddebaf7.png)

Ticket für den User arno lösen
![image](https://user-images.githubusercontent.com/50365065/142419706-470bedf7-b112-4279-b4b4-f76fdcec904e.png)


![image](https://user-images.githubusercontent.com/50365065/142419481-4b188575-80e0-4f2e-8814-fc2ca8dccb73.png)


Rollen auf dem Server in den RSAT Tools
![image](https://user-images.githubusercontent.com/50365065/142415685-6a1d9de5-6b6b-4d5e-a3b2-8822241cb913.png)
![image](https://user-images.githubusercontent.com/50365065/142415698-dff62d85-b529-44ca-aa76-2610735ea2bc.png)

Users Kontrolle

Gleiche Informationen

DNS Kontrolle

Selbe Informationen
RSAT Tools -DNS – For/rev

LDAP Search

Apt install apt install ldap-utils

ldapsearch -x -LLL -H ldap://vmls1.sam159.iet-gibb.ch -b dc=sam159,dc=iet-gibb,dc=ch -D CN=administrator,CN=Users,DC=sam159,DC=iet-gibb,DC=ch -w SmL12345** '(cn=*)'

Überprüfung SID in Systeme:
LAM: tree view -> user anklicken -> objectSID
Win: Server manager starten -> users und computer -> user eigenschaften -> object SID

SID vom User arno
![image](https://user-images.githubusercontent.com/50365065/142420425-9383f727-52e4-42f4-a7bd-47a71a8c0e0b.png)

## Arbeitsblatt 3
Samba mit `apt install samba samba-common-bin smbclient heimdal-clients libpam-heimdal` installieren.

![image](https://user-images.githubusercontent.com/50365065/142423600-6e6bbc31-4018-47cd-b371-de99546ba863.png)

Als Kerberos Server muss SAM159.IET-GIBB.CH angegeben werden. 

`apt install libnss-winbind libpam-winbind` installieren.

/etc/smb/smb.conf

![image](https://user-images.githubusercontent.com/50365065/142424964-34c7c3ae-8268-4ef1-8925-9d491eb7c62a.png)

VMLS2 der Domain joinen `net ads join -Uadministrator`. Der DNS Fehler ist normal, da die VM bereits im DNS eingetragen ist.

User und Gruppen mit `wbinfo -u` resp. `wbinfo -g` auflisten

![image](https://user-images.githubusercontent.com/50365065/142426165-189efaf7-2ec2-45a2-b226-dc90679563c0.png)

```
passwd:         compat systemd winbind
group:          compat systemd winbind
shadow:         compat winbind
```
in /etc/nssswitch.conf anpassen
### Aufgaben
Das Wechseln auf den Admin User funktioniert nicht sauber, da das Home von ihm nicht existiert.

![image](https://user-images.githubusercontent.com/50365065/142427112-f2f9bd31-9076-424a-aadb-37092bcf32f3.png)

Mit dem Befehl `id` sehe ich die IDs des Users und seiner Gruppen.

`S-1-5-21-3036488515-4217794594-2754236771-1107` ist die SID vom User gibb, welcher über die RSAT tools erstellt wurde unter Linux. Die UID ist `1001107` und die GID `1000513`. Die UID unter Linux besteht aus 100 und dem letzen Teil der SID.

## Arbeitsblatt 4
In diesem Arbeitsblatt wird die Config von vmLS2 von einem Config File auf die Registry umgestellt. Die Registry hat den Vorteil, dass sie eine Datenbank ist und somit von mehr als einem User gleichzeitig bearbeitet werden kann. Zudem wird nur die Änderung und nicht die ganze Config über das Netzwerk übertragen. 

Meiner Meinung nach spielt dies heute jedoch nicht mehr wirklich eine Rolle, da eigentlich alle Clients mit 100 Mbit/s, wenn nicht 1000 Mbit/s angebunden sind. Als Linux Admin finde ich ein Config File deutlich praktisches als eine Registry, da die Registry nur von Windows wirklich praktisch verwaltet werden kann, und nicht immer ein Windows Client zur verfügung steht. Zudem habe ich die Samba Config via Registry noch nie in Praxis im einsatz gesehen.

Auf die Registry zugreifen: `net registry enumerate HKLM` `net registry enumerate HKLM\\software\\` `net registry enumerate HKLM\\ software\\samba`

Von vmLS1 über das Netzwerk auf die Registry zugreifen: `net rpc registry enumerate HKLM\\software\\samba\\ -Uadministrator -I`

Von vmLS1 über das Netzwerk via Kerberos Authentisierung auf die Registry zugreifen: `kinit administrator` und `net rpc registry enumerate HKLM\\software\\samba\\ -k -S vmLS2.sam159.iet-gibb .ch`

Von Windows auf die Registry zugreifen: 
1) Auf vmWP1 die die Registry via regedit.exe starten
2) Datei -> Mit Netzwerkregistrierung verbinden
3) vmls2.sam159.iet-gibb.ch eingeben und bestätigen

![image](https://user-images.githubusercontent.com/50365065/143446302-4d50c91d-41d1-43c8-bb42-ba78d7ea5d99.png)

Registry von vmLS2 via Windows regedit angezeigt.

Samba Config Datei in die Registry importieren: `net conf import /etc/samba/smb.conf`

Überprüfen, ob import geklappt hat mit `net conf list`.

### Aufgaben

#### Aufgabe 1
Von Windows auf die Registry zugreifen: 
1) Auf vmWP1 die die Registry via regedit.exe starten
2) Datei -> Mit Netzwerkregistrierung verbinden
3) vmls2.sam159.iet-gibb.ch eingeben und bestätigen

![image](https://user-images.githubusercontent.com/50365065/143446302-4d50c91d-41d1-43c8-bb42-ba78d7ea5d99.png)

#### Aufgabe 2

Im smb.conf File alles löschen und `config backend = registry` eintragen. Danach Samba mit `systemctl restart samba-ad-dc` neustarten

#### Aufgabe 3
Der Test, dass die Config via Registry verwaltet wird, hat funktioniert. Beim Befehl `testparm` sollte die zweite Zeile `lp_load_ex: changing to config backend registry` sein. Dies zeigt an, dass die Config via Registry ausgelesen wird.

## Arbeitsblatt 5
Ordner für Registry Share erstellen: `mkdir -p /daten/reg-share`

Share erstellen: `net conf addshare reg-share /daten/reg-share writeable=y guest_ok=n "Share in der Registry"`

Registry nach /dev/stdout exportieren (Standard Terminal Output) `net registry export hklm\\software\\samba /dev/stdout`

Erstellten Share anzeigen: 
```
root@vmLS2:~# smbclient -L vmls2 -Uadministrator
lp_load_ex: changing to config backend registry
Enter SAM159\administrator's password: 

	Sharename       Type      Comment
	---------       ----      -------
	IPC$            IPC       IPC Service (Samba 4.13.14-Ubuntu)
	reg-share       Disk      Share in der Registry
SMB1 disabled -- no workgroup available
```

#### Aufgaben

1) Nein, ich kann nicht schreiben. Dies liegt daran, dass guest ok auf no gesetzt ist. Da ich mich nicht speziell als User eingeloggt habe, habe ich keine Schreibrechte.

![image](https://user-images.githubusercontent.com/50365065/145401394-9352de2e-4b25-49e1-9709-eae6ff00d4a8.png)

2) Das Home Directory kann beim Profil vom User festgelegt werden ![image](https://user-images.githubusercontent.com/50365065/143450125-25bd6c1a-b0fc-403b-b219-8c29e5a6eb9f.png)
3) Die Berechtigungen sind folgendermassen gesetzt: `drwxrwx---+ 4 administrator domain users  4.0K Nov 25 14:43 arno
`
4) Ja, das Home wurde gemountet ![image](https://user-images.githubusercontent.com/50365065/143452280-b3174032-0316-47bb-8f00-75de02c9281f.png)
5) Die ACLs vom Homedirectory vom User arno sehen folgenermassen aus:

```
# file: .
# owner: administrator
# group: domain\040users
user::rwx
user:domain\040users:---
user:arno:rwx
group::---
group:BUILTIN\\administrators:rwx
group:administrator:rwx
group:domain\040users:---
group:arno:rwx
mask::rwx
other::---
default:user::rwx
default:user:administrator:rwx
default:user:arno:rwx
default:group::---
default:group:BUILTIN\\administrators:rwx
default:group:domain\040users:---
default:group:arno:rwx
default:mask::rwx
default:other::---
```

Heimatverzeichnis unter Windows:

![image](https://user-images.githubusercontent.com/50365065/145402496-b1973b01-53f4-46f6-9f07-480e54bbc54d.png)

## Arbeitsblatt 6
Admin-Share einrichten

![image](https://user-images.githubusercontent.com/50365065/145403936-d36e21a9-d350-48b0-8f85-c24723d5755e.png)

Rechte des Admin-Shares vergeben und abfragen

![image](https://user-images.githubusercontent.com/50365065/145404000-f3f05f5d-6812-4f83-90ba-676eb57bafda.png)

### Aufgaben
1) Das Privlege SeDiskOperatorPrivilege bedeutet, dass User die dies haben, Shares konfigurieren können. `Only users and groups having the SeDiskOperatorPrivilege privilege granted can configure share permissions.`. Quelle: https://wiki.samba.org/index.php/Setting_up_a_Share_Using_Windows_ACLs
2) Die ACLs des PUBLIC Shares auf dem Samba Server sehen wie folgt aus:
```
# file: .
# owner: administrator
# group: domain\040users
user::rwx
user:root:rwx
user:domain\040admins:rwx
user:domain\040users:rwx
group::rwx
group:administrator:rwx
group:domain\040admins:rwx
group:domain\040users:rwx
mask::rwx
other::r-x
default:user::rwx
default:user:root:rwx
default:user:administrator:rwx
default:user:domain\040admins:rwx
default:user:domain\040users:rwx
default:group::r-x
default:group:domain\040admins:rwx
default:group:domain\040users:rwx
default:mask::rwx
default:other::r-x
```
3) Der Parameter `hide unreadable = yes` bewirkt, dass Shares und Ordner nicht angezeigt werden, welche nicht lesbar sind. Dieser Parameter kann verwendet werden, um Shares von User zu verstecken, auf die sie sowieso keinen Zugriff haben.

#### Vorgehen
1) Gruppen im GUI erstellen. Dazu kann via RSAT-Tools die Gruppen GL, PRODUKTION UND VERWALTUNG erstellt werden.
2) Die User können ebenfalls einfach im RSAT erstellt werden
3) Doppelklick auf die User -> Mitglied von -> Entsprechende Gruppe hinzufügen
4) Im admin-share ein Verzeichnis namens ABTEILUNGEN erstellen
5) Im Verzeichnis ABTEILUNGEN die drei Verzeichnisse VERWALTUNG, PRODUKTION UND PUBLIC erstellen
6) In der Registry einen neuen Key namens sh-abteilungen erstellen. Die Fehlermeldung kann weggeklickt werden, danach muss F5 gedrückt werden und der neue, überflüssige Schlüssel wieder gelöscht werden


## Arbeitsblatt 7


