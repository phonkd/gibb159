#  $\mathfrak{\color{lime}{\huge Kerberos}}$
$\mathfrak{\color{lime}{Lernziele:}}$ 
- Beschreiben, was unter einem Directory Service verstanden wird und auf welchen Komponenten dieser Service basiert. Sie können auch Elemente einer nativen Kerberos-Infrastruktur in einer Microsoft ADDS-Infrastruktur identifizieren und benennen.  
  $\mathfrak{\color{red}{Antwort:}}$ 
  - Ein Directory Service ist wie ein Netzwerkverzeichnis, in dem Informationen über Computer und Benutzer gespeichert werden. In einer nativen Kerberos-Infrastruktur wie Microsoft Active Directory (ADDS) sind die Hauptkomponenten:
   - **Verzeichnisdatenbank**: Hier werden Informationen über Benutzer und Ressourcen gespeichert.
   - **Active Directory-Domänencontroller (DC)**: Diese Server hosten die Datenbank und bieten Dienste wie Authentifizierung und Autorisierung an.
   - **LDAP**: Ein Protokoll zur Kommunikation mit der Datenbank. 
   - **Kerberos**: Ein Sicherheitsprotokoll für die sichere Authentifizierung von Benutzern und Diensten.
   - **DNS**: Ein System zur Auflösung von Netzwerknamen in IP-Adressen.
   - **GPOs**: Richtlinien zur Verwaltung von Einstellungen und Berechtigungen.
   - **OUs**: Container zur Organisation von Benutzern und Ressourcen im Verzeichnis.

- Verständnis Authentisierung – Authorisierung. Sie können diese beiden Begriffe mit Beispielen klar definieren. 
   $\mathfrak{\color{red}{Antwort:}}$
   - **Authentifizierung** --> Die Authentifizierung ist der Prozess, bei dem die Identität einer Entität überprüft wird. Dieser Prozess ist enscheidend, um sicherzustellen, dass nur autorisierte Benutzer auf bestimmte Ressourcen zugreifen können. _Beispiel:_ Angenommen, du meldest dich bei einem E-Mail-Konto an. Die EIngabe deines Usernamen und Passwort ist ein Authentitfizerungsprozess.  
   - **Autorisierung** --> Die Autorisierung bezieht sich auf den Prozess der Festlegung von Berechtigungen und Zugrifsrechten für authentifizierte Benutzer oder Entiäten. _Beispiel:_ Du bist in einem Firmennetz angemeldet. Die Autorisierung bestimmt nun, welche Aktionen du innerhalb des Netzwerkes ausführen darfst. 
   - **Zusammengefasst**:
	   - _Authentifizierung_ ist der Prozess der Identitätsprüfung.
	   - _Autorisierung_ ist der Prozess der Festlegung von Zugriffsrechten und Berechtigungen nach erfolgreicher Authentifizierung.

- Rolle von Kerberos in einem Directory Service beschreiben können 
   $\mathfrak{\color{red}{Antwort:}}$ 
   - **Authentifizierung** --> die Hauptrolle von Kerberos in einem Directory Service besteht darin die Authentifizierung von Benutzern zu ermöglichen.
   - **Sign-On** (SSO) --> Kerberos unterstützt das Konzept des SSO. Nach der ersten erfolgreichen Anmeldung erhält der Benutzer ein Ticket für den Zugriff auf verschiedene Dienste im AD. Nach der ersten Authentifizierung erhält der Client ein Ticket Granting Ticket (TGT). Dieses TGT kann dann gesendet werden, damit der User SSO verwenden kann. 
   - **Autorisierung** --> Nach einer erfolgreichen Benutzer Authentifizierung, ermöglicht Kerberos die Autorisierung von Benutzern. 
   - **Sicher** --> Kerberos bietet ein hohes Mass an Sicherheit, da es die Kommunikation zwischen Benutzern und dem AD. Schützt vor potenziellen Angriffen wie Man-in-the-Middle-Angriffen und Abhören des Datenverkehrs.

- Begriffe innerhalb von Kerberos benennen und erklären können
   $\mathfrak{\color{red}{Antwort:}}$
   - **Principal** --> Ein Principal ist eine Entität, die sich bei Kerberos authentifiziert. Es kann sich dabei als User, einen Dienst oder Maschiene handeln. 
   - **Realm** --> Ein Realm ist eine logische Organisationseinheit in einem Kerberos-System. Es kann sich um eine Domäne, eine Abteilung oder eine Art von Sicherheitsdomäne handeln.
   - **Ticket** --> Ein Ticket ist eine verschlüsselte Nachricht, die die Identität des Benutzers und Informationen über die gewünschte Dienste enthält.
   - **Key Distribition Center** (**KDC**) --> Der Key Destribution Center ist der zentrale Dienst, der Authentifizierungsserver sowie den Ticket Granting Server enthält.

- Elementare notwendige Komponenten einer Kerberos-Infrastruktur benennen und erklären 
   $\mathfrak{\color{red}{Antwort:}}$
   - **Principal** 
   - **KDC**
   - **Authentication Server** (**AS**) --> Der AS ist für die Autheentifizierung von Benutzern zuständig.
   - **Ticket Granting Server** (**TGS**) --> Der TGS ist für die Ausgabe von Tickets für Dienste verantwortlich.
   - REALM
   - **KDC-Datenbank** --> In dieser DB werden die Identitäten der Principals und deren verschlüsselten Kennwörter oder Schlüssel abgelegt. 
   - **Tickets**

- Wie werden Netzwerkdienste mit Kerberos integriert? Sie können das allgemein erklären und im Detail anhand einer Linux- und kerberisiserten Authentifizierung. 
   $\mathfrak{\color{red}{Antwort:}}$
   - **Serverseitige Integration** --> Der Server eines Netzwerkdienstes muss so konfiguriert werden, dass er Kerberos-Tickets akzeptiert. Der Dienst muss auch so konfiguriert sein, dass er die Tickets entschlüsseln und Benutzeridentitäten überprüfen kann.
   - **Clientseitige Integration** --> Die Clientseitige, die auf Netzwerkdienste zugreifen, müssen so konfiguriert sein, dass sie Kerberos-Tickets anfordern und verwenden.
   - **Ticketverwaltung** --> Sowohl Server als auch Client müssen die Tickets verwalten, indem sie sicherstellen, dass sie rechtzeitig erneuert werden un dass abgelaufene Tickets verworfen werden. 

- Sie kennen den Zusammenhang zwischen SSO und Kerberos und können erklären, welche Mechanismen ein SSO ermöglichen. 
   $\mathfrak{\color{red}{Antwort:}}$
   - Kerberos das Single Sign-On, indem es sicherstellt, dass du dich einmalig authentifizierst und dann Zugriff auf verschiedene Dienste erhältst, ohne wiederholt Anmeldeinformationen eingeben zu müssen. Es ist, als ob du einen magischen Schlüssel für das gesamte Netzwerk hast.

- Sie wissen, wo Kerberos seine Daten abspeichert und kennen 2 Orte, wo sich diese Daten befinden können. 
   $\mathfrak{\color{red}{Antwort:}}$
   - **Domänencontrollern** --> Sie speichern die Kerberos-Daten für aller Benutzer und Computern in einem Netzwerk.
   - **Auf dem Client** --> Wenn du dich bei deinem Computer anmeldest, speichert dein Computer einige Kerberos-Daten lokal.

- Sie können das Kerberos-Protokoll im Detail erklären, kennen die Bedeutung von temporären Schlüsseln und Langzeitschlüssel. Sie wissen beispielsweise genau was passiert, nachdem sie die Enter-Taste bei der Domain-Anmeldung unter Windows gedrückt haben.  
   $\mathfrak{\color{red}{Antwort:}}$
   - **Schritt 1: Anmelden an deinem Computer**
     Stell dir vor, du möchtest deinen Computer nutzen. Du gibst deinen Benutzernamen und dein Passwort ein und drückst Enter, um dich anzumelden. Jetzt beginnt die Magie von Kerberos.
   - **Schritt 2: Die Ticketanforderung**
     Dein Computer (der als Client bezeichnet wird) sagt: "Hey, ich möchte gerne auf Dateien und andere Dinge im Netzwerk zugreifen, aber ich brauche Hilfe von Kerberos, um sicherzustellen, dass ich wirklich ich bin."
   - **Schritt 3: Der Ticket Granting Server (TGS**)
     Kerberos hat einen besonderen Server, den Ticket Granting Server (TGS). Der TGS ist wie ein Türsteher für das Netzwerk. Er prüft, ob du wirklich du bist. Der TGS sagt: "Okay, ich glaube dir. Hier ist ein spezielles Ticket für dich, ein Ticket Granting Ticket (TGT). Das ist wie dein Eintrittspass für das ganze Netzwerk."
   - **Schritt 4: Der TGS-Anforderung**
     Jetzt, da du dein TGT hast, möchtest du auf eine bestimmte Datei oder einen anderen Dienst zugreifen, zum Beispiel einen Drucker. Du fragst den TGS: "Kann ich bitte auf den Drucker zugreifen?"
   - **Schritt 5: Das Service Ticket**
     Der TGS sagt: "Klar, hier ist ein Service Ticket für den Drucker. Damit kannst du darauf zugreifen. Aber ich werde auch ein geheimes Schlüsselpaar mitgeben, einen temporären Schlüssel, um sicherzustellen, dass nur du den Drucker benutzen kannst."
   - **Schritt 6: Der Zugriff auf den Dienst**
     Jetzt gehst du zum Drucker. Du zeigst dein Service Ticket und den temporären Schlüssel vor. Der Drucker sagt: "Ich erkenne dein Ticket und deinen Schlüssel. Du darfst drucken!"

- Durch ihre detaillierten Kenntnisse des Kerberos-Protokolls können sie auch Beschreibungen von Hackerangriffen einordnen und erklären und mögliche Gefahren für die Sicherheit abschätzen. 
   $\mathfrak{\color{red}{Antwort:}}$
   - **Kerberos-Ticket-Weiterleitung (Pass-the-Ticket-Angriff):** Ein Angreifer, der Zugriff auf ein Kerberos-Ticket erlangt hat, kann dieses Ticket ohne Berechtigung an einen anderen Dienst weiterleiten und sich als ein anderer Benutzer ausgeben. Dies kann dazu führen, dass der Angreifer auf Ressourcen zugreifen kann, für die er keine Berechtigung hat.
   - **Kerberos-Golden Ticket-Angriff:** Ein Angreifer mit ausreichenden Berechtigungen kann ein gefälschtes Kerberos-Ticket erstellen, das ihm uneingeschränkten Zugriff auf das gesamte Netzwerk gewährt. Dies kann schwerwiegende Sicherheitsverletzungen zur Folge haben, da der Angreifer vollständige Kontrolle über das Netzwerk erlangt.
   - **Kerberos-Silber Ticket-Angriff:** Hierbei handelt es sich um eine Variation des Golden Ticket-Angriffs, bei dem ein Angreifer ein gefälschtes Ticket erstellt, um auf bestimmte Ressourcen oder Dienste zuzugreifen. Dies ist weniger umfassend als ein Golden Ticket, aber immer noch gefährlich.
   - **Brute-Force-Angriffe auf Passwörter:** Wenn ein Angreifer Zugriff auf die Kerberos-Datenbank erhält, kann er versuchen, Passwörter durch Brute-Force-Angriffe zu erraten. Dies ist ein zeitaufwändiger Prozess, aber wenn erfolgreiche Passwörter gefunden werden, kann der Angreifer umfassenden Zugriff erhalten.
   - **Kerberos-Pre-Authentication-Angriff:** Einige Kerberos-Implementierungen ermöglichen es Benutzern, sich ohne Pre-Authentication anzumelden, was bedeutet, dass sie kein Passwort eingeben müssen, um ein Ticket zu erhalten. Ein Angreifer kann diese Schwachstelle ausnutzen, um Tickets für andere Benutzer zu stehlen und auf Ressourcen zuzugreifen.

- Sie können auch einen «theoretischen» Hackerangriff selber konzipieren, indem sie von halbwegs realistischen Annahmen ausgehen. Im Sinne von: wenn ich das weiss, dann könnte ich …  
   $\mathfrak{\color{red}{Antwort:}}$
   **Hypothetischer Angriff: Kerberos Ticket Sniffing**
	Annahmen:
	1. Der Angreifer hat Zugang zum internen Netzwerk, entweder physisch oder über eine Kompromittierung eines internen Systems.
	2. Der Angreifer hat Kenntnisse über die Netzwerktopologie und den Kerberos-Verkehr.
	Ablauf:
	1. Der Angreifer positioniert sich im internen Netzwerk und verwendet Sniffer-Software, um den Kerberos-Netzwerkverkehr abzuhören.
	2. Ein legitimer Benutzer meldet sich an seinem Computer an und erhält ein Ticket Granting Ticket (TGT) von der Authentifizierung im Kerberos-System.
	3. Der Benutzer möchte auf eine Dateifreigabe zugreifen. Er fordert ein Service Ticket an und verwendet das TGT.
	4. Der Service Ticket wird verschlüsselt an den Benutzer gesendet. Der Angreifer fängt den verschlüsselten Datenverkehr ab.
	5. Der Angreifer versucht, den verschlüsselten Service Ticket zu knacken oder auf andere Weise zu entschlüsseln. Wenn dies gelingt, erhält er Zugriff auf die geschützte Ressource, auf die der Benutzer zugreifen wollte.

- Die Fachbegriffe innerhalb des Kerberos-Protokolls kennen sie. Beispielsweise können sie genau erklären, was ein Service Key ist und was ein Service-Session-Key ist und welche Bedeutung diese Keys für die Sicherheit haben. 
   $\mathfrak{\color{red}{Antwort:}}$
   - **Service Key (Dienstsitzungsschlüssel):**
	Stell dir vor, du möchtest in einen Geheimclub gehen. Der Service Key ist wie ein besonderer Schlüssel, den du benötigst, um die Tür zu öffnen. Dieser Schlüssel gehört dem Club und nur er kann damit die Tür öffnen. In der Welt von Kerberos ist der Service Key ein geheimer Schlüssel, den ein Dienst (wie ein Server oder eine Anwendung) hat. Wenn du auf diesen Dienst zugreifen möchtest, musst du den Service Key verwenden, um sicherzustellen, dass du eingelassen wirst. Dieser Schlüssel ist stabil und langfristig, ändert sich nicht oft und bleibt geheim.
  - **Service Session Key (Dienst-Sitzungsschlüssel):**
	Jetzt, nachdem du die Tür geöffnet hast und im Geheimclub bist, möchtest du sicherstellen, dass niemand hört, was du und deine Freunde miteinander sprechen. Der Service Session Key ist wie ein spezieller, geheimer Code, den du und der Club verwenden, um eure Gespräche zu verschlüsseln. Niemand kann eure Unterhaltung verstehen, außer euch beiden. In der Welt von Kerberos ist der Service Session Key ein temporärer Schlüssel, der speziell für deine aktuelle Verbindung zu einem Dienst erstellt wird. Er ist kurzlebig und ändert sich jedes Mal, wenn du dich mit diesem Dienst verbindest. Das macht deine Kommunikation sicher und geheim.[]()
	Zusammengefasst, der Service Key ist wie der Schlüssel zur Tür eines Clubs, während der Service Session Key wie ein geheimer Code für private Gespräche innerhalb des Clubs ist. Beide sind wichtig für die Sicherheit, da sie sicherstellen, dass nur autorisierte Benutzer auf Dienste zugreifen und sicher miteinander kommunizieren können.

- Sie kennen Befehle um Kerberos zu administrieren und wissen auch, was man in Kerberos administrieren kann und muss. 
   $\mathfrak{\color{red}{Antwort:}}$
   - **Kerberos-Verwaltungsbefehle:**
	   - _kinit (Kerberos Initialisierung)_ --> Du gibst deinen Benutzernamen und dein Passwort ein, und es erstellt ein Ticket Granting Ticket (TGT), dass dir erlaubt, auf verschiedene Dienste zuzugreifen. 
	   - _klist (Kerberos-Liste)_ --> Kann zum anzeigen der Tickets verwendet werden. Es zeigt, welche Tickets du hast und wann sie ablaufen.
	   - _kdestroy (Kerberos-Zerstörung)_ --> Es löscht alle Tickets die du erhalten hast.
   - **Was in Kerberos administriert werden kann und muss:**
	   - _Benutzerkonten_ --> Für Benutzerkonten können Benutzernamen und Passwörter festgelegt werden. Diese Infos werden in der Kerberos-Datenbank gespeichert.
	   - _Dienstprinzipien_ --> Du musst sicherstellen, dass nur autorisierte Dienste Zugriff erhalten. 
	   - _Sicherheitsrichtlinien_ -->Du musst festlegen, wie lange Tickets gültig sind, wie stark Passwörter sein müssen und noch weitere Sicherheitsanforderungen.
	   - _Tickets_ --> Du musst sicherstellen, dass die Tickets sicher übertragen und gespeichert werden. 

- Sie wissen, was benötigt wird, um einen Kerberos-Administrationsservice zu betreiben
   $\mathfrak{\color{red}{Antwort:}}$
   Um einen Kerberos-Administrationsservice zu betreiben wird folgendes benötigt: 
   - **Computer und Netzwerk** --> Du benötigst einen Computer sowie ein Netzwerk indem ein Kerberos-Administrationservice betrieben werden kann. 
   - _Kerberos Software_ --> Die Software muss installiert und konfiguriert werden.
   - _Datenbank_ --> Es wird eine Datenbank benötigt wo Benutzerkonten, Dienstprinzipien und Sicherheitskonzepte gespeichert werden.
   - _Sicherheitsmassnahmen_ --> Bei Kerberos müssen Sicherheitsmassnahmen wie Verschlüsselung und Überwachung eingerichtet werden.
   
https://youtube.com/watch?v=qW361k3-BtU&si=M6n9rzFlvuq5s7lq

