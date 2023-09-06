***
>[!warning]
>All VM's must have the same Gateway set to `192.168.110.2`

>[!info] IPS:

| VM    | IP             | Gateway       |
| ----- | -------------- | ------------- |
| VMLS1 | 192.168.110.61 | 192.168.110.2 |
| VMLS2 | 192.168.110.62 | 192.168.110.2 |
| VMLP1 | 192.168.110.30 | 192.168.110.2 |
| VMWP1 | 192.168.110.10 | 192.168.110.2 |
 

>[!warning] IPs in dokument
>Use the IP's from above and not the IP's from the document.
>The ones in the document are not for smartlearn online.
>

>[!warning] Netplan
>Do not adjust any netplan stuff even when in document

^4cc64a

>[!warning] RechnerNamen:
>`vmLS1.sam159.iet-gibb.ch`


>[!warning] Search domain im netplan file
>`sam159.iet-gibb.ch


## Howto

### vmLP1

1. Change Gateway in `vmLP1`
	1. Open ubuntu network settings
	2. Click on cog
	3. adjust gateway to `192.168.110.2`
	4. Save
	5. Turn off lan connection
	6. Turn lan back on
2. Remove goofy ssh thing: `ssh-keygen -f "/home/vmadmin/.ssh/known_hosts" -R "192.168.110.62"`
>[!success] Ready to use ssh (vmLP1)

### vmLS1


## Seite 04 fehler beheben

```bash
rm /etc/resolv.conf
```

`editor /etc/resolv.conf`

```inhalt
nameserver 192.168.110.61
search sam159.iet-gibb.ch
```
