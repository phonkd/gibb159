# Modul 159
## Arbeitsblatt 01
### Exceltabelle
![KerberosTabelle](NullNUMMER24/images/KerberosTabelle.png)
### Was würde passieren, wenn der private Schlüssel des KDC bekannt wäre ?
1. **Falsche Identitäten:** Die Leute könnten gefälschte Tickets erstellen und sich als andere ausgeben, um auf Dinge zuzugreifen, für die sie keine Berechtigung haben.
2. **Geheimnisenthüllung:** Der Datenverkehr, der normalerweise geschützt ist, könnte entschlüsselt werden, was zu Informationsdiebstahl führen könnte.
3. **Vertrauensverlust:** Die Leute würden dem Netzwerk weniger vertrauen, weil sie nicht sicher wären, ob die Sicherheitsmechanismen noch funktionieren.
4. **Angriffe:** Die Angreifer könnten sich in den Datenverkehr einschalten und ihn manipulieren, um Schaden anzurichten.
### Warum kann der Client das TGT nicht entschlüsseln ?
Der Ticket Granting Ticket (TGT) ist verschlüsselt, um die Sicherheit zu gewährleisten. Der Client kann es nicht entschlüsseln, weil es mit dem geheimen Schlüssel des KDC verschlüsselt ist. Das macht es schwierig für Angreifer, die darin enthaltenen Informationen abzufangen oder zu ändern. Der Client nutzt das TGT, um bei anderen Diensten spezielle Tickets zu bekommen, die der Client dann entschlüsseln kann, um auf bestimmte Dinge zuzugreifen.
