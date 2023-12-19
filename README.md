# Cybersecurity in einer Web-Applikation
## Einführung
In diesem Portfoliobeitrag schreibe ich über Sicherheitsmassnahmen in einer Web-Applikation. Ich zeige verschiedene konzepte auf und zeige auch Beispiele aus Sicht eines Angreifers wie Lücken im Programm genutzt werden können um Unfug zu treiben. Natürlich werden dazu auch Methoden gezeigt um diese Lücken zu schliessen. Der ganze Beitrag ist in 5 Abschnitte aufgeteilt und beschäftigt sich jeweils mit einem bestimmten Thema. 

## Handlungsziel 1
### Phishing

Phishing ist eine betrügerische Technik, bei der versucht wird, durch das Vortäuschen vertrauenswürdiger Quellen sensible Informationen wie Passwörter oder Kreditkartendaten von ahnungslosen Personen zu stehlen.

**Gegenmassnahmen**
* Nicht auf Links oder Anhänge in verdächtigen Nachrichten klicken
* Vorsichtiger Umgang mit E-Mails von unbekannten Absendern
* Überprüfung von komischen Endungen der E-Mail Adresse

### Ransomware

Ransomware ist eine schädliche Software, die die Dateien eines Computers verschlüsselt und dann Lösegeld von ihrem Besitzer verlangt, um die Daten wieder freizugeben.

**Gegenmassnahmen**
* Betriebssysteme und Software auf dem neusten Stand halten.
* Regelmässige Backups machen

### Malware

Malware ist schädliche Software, die entwickelt wurde, um deinem Computer oder Gerät zu schaden, Daten zu stehlen oder unerwünschte Aktionen auszuführen, ohne dass du es merkst.

**Gegenmassnahmen**
* Nur Dateien aus sicheren Quellen herunterladen
* Betriebssyteme und Software auf dem neusten Stand halten.
* Antivirenprogramm benutzen

### DDoS-Angriffe

DDoS (Distributed Denial of Service) ist eine Angriffsmethode, bei der eine Vielzahl von Computern gleichzeitig eine Website oder einen Dienst überlastet, um ihn unzugänglich zu machen.

**Gegenmassnahmen**
* Netzverkehr filtern
* Gute Netzwerksicherheit und Überwachung verwenden

### Kritische Beurteilung
Ich denke, ich habe dieses Handlungsziel erreicht, da ich Bedrohungen und deren Gegenmassnahmen aufzeigen konnte.
  
## Handlungsziel 2
Durch SQL Injection kann man sehr einfach in der Insecure App sich als Administrator einloggen ohne das Nötige Passwort zu kennen. Mit den Eingaben des folgenden Bildes kann man sich als Administrator einloggen.

![Beispiel in der App](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/26f14051-2a19-416d-b5ae-0470ce42ec31)

Um dies zu verhindern müssen wir den jetzigen Code hier:

![Beispiel im Code](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/1f346ef9-1d8e-490a-b177-7c8a87464ba9)

Wie folgt ändern um ihn vor SQL Injection zu schützen: 

![Biespiel des überarbeiteten Codes](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/7d01d65c-0b08-4d67-a947-94b474344d4e)

Vor der Änderung haben wir mit der Funktion "FromSqlRaw" gearbeitet, was gleich ist wie eine Abfrage in einer Datenbank umgebung, was daher sehr viele Möglichkeiten bietet und auch das Risiko damit erhöht.

### Kritische Beurteilung
Ich denke, ich habe dieses Handlungsziel erreicht, da ich anhand eines Beispiels SQL-Injection verhindern konnte in meinem Projekt.

## Handlungsziel 3

### Unterschied Authentifizierung und Autorisierung

Authentifizierung beantwortet die Frage "Wer bist du?" und validiert die Identität einer Person oder eines Systems. Autorisierung definiert, welche Aktionen oder Ressourcen einem authentifizierten Benutzer gestattet sind und beantwortet die Frage "Was darfst du tun?" - also welche Berechtigungen dieser Benutzer hat. In Kombination gewährleisten Authentifizierung und Autorisierung einen sicheren Zugriff auf Systeme und Daten.

### Implementierte Authentifizierung

In der Insecure App haben wir zur Authentifizierung haben wir zum einen seit der ersten Version eine einfache Authentifizierung mit einem Passwort. Das bedeutet wenn jemand dein Passwort kennt kann er auf alle deine Daten in der Insecure App zugreifen. Daher haben wir eine 2-Faktor-Authentifizierung eingerichtet basierend auf dem Google-Authenticator. Wenn der Nutzer also die 2FA aktiviert hat auf seinem Konto reicht das Passwort alleine nicht mehr um sich einzuloggen. Der Nutzer muss jedes mal beim Einloggen einen, von der Google-Authenticator App generierten, neuen 6-stelligen Code eingeben.

![Login mit 2-Faktor-Authentifizierung](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/8a164fa4-d5e3-4b7a-8404-0a4c511ab782)

Möchte ein Nutzer die 2FA bei sich einrichten, so muss er zu "Enable 2FA" navigieren und auf den Button mit der Aufschrift "Enable" klicken. Sobald auf den Button geklickt wurde wird ihm ein QR code generiert, den er mit seiner Authenticator App scannen kann. Nach dem Scannen ist das Konto in der App hinterlegt. 

![2FA QR-Code](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/7fbc4282-c7f4-4aa8-a7c8-a69131141310)

### Implementierte Autorisierung

In der Insecure App wird die Autorisierung mit 2 Rollen geregelt, zum einen der Admin, der alles machen darf und dem User, der nur begrenzte Möglichkeiten besitzt. So wird zum Beispiel im folgenden Beispiel bei der Löschung eines Eintrages überprüft, ob ein Admin den Eintrag löschen möchte oder der Autor des Eintrages selbst. Ist es keiner der beiden Fällen ist es dem Nutzer nicht erlaubt diese Aktion zu vollstrecken. 

```C#
if (!_userService.IsAdmin() && _userService.GetUserId() != news.AuthorId)
{
    return Forbid();
}
```
![Autorisierung im Code](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/83a2eb5b-9239-4f58-8df2-c7c546152cd0)

### Kritische Beurteilung
Ich denke, ich habe dieses Handlungsziel erreicht, da ich erklärt habe wie Authentifizierung und Autorisierung und meiner Applikation funktioniert.

## Handlungsziel 4

In den meisten Fällen, in denen Konten gehackt werden, liegt es nicht an Sicherheitslücken des Systems, sondern an den Menschen. Nutzt jemand das Passwort "Hallo123" ist dies nicht sehr sicher, da es ein sehr verbreitetes Passwort ist, nicht sehr lange ist und ebenfalls aus einem klaren Wort besteht. Stattdessen sollte man ein möglichst langes Passwort verwenden mit zufälligen Zeichenfolgen bestehend aus Kleinbuchstaben, Grossbuchstaben, Zahlen und Sonderzeichen. Jedes Passwort ist Knackbar, die Frage ist nur wie lange man dafür braucht.
In der Insecure App haben wir eine kleine Funktion erstellt, welche den Nutzer ermutigen soll ein sicheres Passwort zu wählen. 

![Code der Passwortüberprüfung](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/4b99391e-895a-4fab-85ec-20a7c97b44dd)

Ebenfalls sollte der Key für den JWT auch nicht in unserem öffentlichen Github Repository sichtbar sein, weshalb wir diesen im Secret Manager-Tool von ASP.NET ablegen sollten. diese Files werden nicht in das Repository geladen. In unserem veröffentlichten Code steht nur eine Referenz zu diesem Secret, womit aussenstehnde aber nichts anfangen Können. Dies Funktioniert nach dem gleichen prinzip wie die ".env" Files.

### Kritische Beurteilung
Ich denke, ich habe dieses Handlungsziel erreicht, da ich aufgezeigt habe wie in meiner Applikation überprüft wird, wie sicher das Passwort ist.

## Handlungsziel 5

Durch das Hinzufügen von Logging-Nachrichten können Entwickler und Administratoren essenzielle Ereignisse in der Anwendung überwachen. Diese Protokolle sind besonders nützlich beim Debuggen, der Überwachung der Anwendungsleistung und der Identifizierung von Problemen oder Sicherheitsvorfällen. Sie bieten Einblicke in das Geschehen der Anwendung und erleichtern das Tracking von Aktivitäten im System.
In der Insecure App haben wir zum Beispiel Logs erstellt beim Login, wenn es ein Fehler gibt beim Login, aber auch wenn sich ein Nutzer erfolgreich eingeloggt hat.

![Logging im Code](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/2189b08d-c8c5-4118-a119-67b892411939)

Diese Logs werden demnach ausgegeben bei einer Fehlversuch und einem Erfolgreichen Login ausgegeben in der Konsole

![Fehlversuch beim Login](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/34696ae1-9962-4d12-bbf5-28e591a8799d)

![Erfolgreiches Login](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/0cac6c14-0ef8-425c-8a2a-759a43a59686)

### Kritische Beurteilung
Ich denke, ich habe dieses Handlungsziel erreicht, da ich aufzeige wo in meinem Programm Logs augegeben werden und weshalb man diese verwendet.

## Selbsteinschätzung
Ich habe das Gefühl, dass ich mit diesem Beitrag eine gute Arbeit geleistet habe und mein gelerntes Wissen gut aufzeigen konnte. Auch meine Leistung während des Modules schätze ich als gut ein, da ich zu fast jedem Zeitpunkt verstanden habe was ich bearbeite und das ohne grosse Schwierigkeiten. 
