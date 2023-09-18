***

[VM Warning](VM%20Warning.md)

# Umgebung:

>[!tip] Via SSH wird von der `VMLP1` auf alle anderen benötigten VMS zugegriffen.

>[!warning]
>Da zu beginn dns nicht funktioniert, muss ssh mit den IP-Adressen gebraucht werden (falls eine VM eine andere IP hat, muss die IP angepasst werden):
>
>| VM    | IP                |
| ----- | -------------- |
| ----- | -------------- |
| VMLS1 | 192.168.110.61 |
| VMLS2 | 192.168.110.62 |
| VMLP1 | 192.168.110.30 |
| VMWP1 | 192.168.110.10 |
>Bei `ssh vmadmin@192.168.110.6x` kommt eine Fehlermeldung.
>Um das Problem zu lösen kann einfach der vorgeschlagene command verwendet werden:
>![](Pasted%20image%2020230918155205.png)

# Überprüfen der Gateway und DNS Server einstellungen:

## VMLS1

>[!info] Stimmte bereits
>![](Pasted%20image%2020230918155329.png)

>[!warning] Hostname muss be `vmls1` angepasst werden:
Der namen des rechners muss `vmLS1.sam159.iet-gibb.ch` sein.
>`sudo echo "vmLS1.sam159.iet-gibb.ch" > /etc/hostname`

>[!warning] Search domain anpassen
>`sudo nano /etc/netplan/00-eth0.yaml`
>![](Pasted%20image%2020230918163058.png)

## VMLS2

>[!info] auch hier stimmte es bereits
>![](Pasted%20image%2020230918155558.png)

## VMLP1

>[!error] Hier müssen DNS und gateway angepasst werden:
>![](Pasted%20image%2020230918155656.png)


1. Einstellungen für den Netzwerkadapter in gnome settings öffnen:
	![](Pasted%20image%2020230918155754.png)
2. Unter `IPV4` die Felder `DNS` und `Gateway` anpassen:
	![](Pasted%20image%2020230918155918.png)
3. `anwenden`
4. Damit die Änderungen übernommen werden muss der adapter wia Schalter neugestartet werden.

## VMWP1

1. `win` + `r` 
2. `ncpa.cpl` + enter
3. ![](Pasted%20image%2020230918162608.png)
