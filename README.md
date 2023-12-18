# LB_183_Portfolio_VontobelJan

## Handlungsziel 1
In der Insecure App können Nutzer ohne die nötigen Rechte zu besitzen alle Beiträge bearbeiten, sobald sie die ID eines Eintrages kennen. Der Nutzer kann zwar nicht direkt auf der Insecure App Seite die Daten verändern, aber ganz einfach mit Tools wie Swagger oder Postman. Auf diese Weise können die Rollen sehr einfach umgangen werden und könnten auch weggelassen werden, wenn dies nicht behoben wird. In der Theorie könnte ein Nutzer alle Beträge löschen und damit einen grossen Schaden anrichten.

![Code Beispiel](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/0ba7d5df-4d50-4df9-8e7d-37cd4d8ece18)
In dieser Patchanfrage müsste zusätzlich noch überprüft werden, ob der Nutzer die nötige Berrechtigung dazu hat.

## Handlungsziel 2
Durch SQL Injection kann man sehr einfach in der Insecure App sich als Administrator einloggen ohne das Nötige Passwort zu kennen. Mit den Eingaben des folgenden Bildes kann man sich als Administrator einloggen.

![Beispiel in der App](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/26f14051-2a19-416d-b5ae-0470ce42ec31)

Um dies zu verhindern müssen wir den jetzigen Code hier:

![Beispiel im Code](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/1f346ef9-1d8e-490a-b177-7c8a87464ba9)

Wie folgt ändern um ihn vor SQL Injection zu schützen: 

![Biespiel des überarbeiteten Codes](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/7d01d65c-0b08-4d67-a947-94b474344d4e)

Vor der Änderung haben wir mit der Funktion "FromSqlRaw" gearbeitet, was gleich ist wie eine Abfrage in einer Datenbank umgebung, was daher sehr viele Möglichkeiten bietet und auch das Risiko damit erhöht.

## Handlungsziel 3

### Unterschied Authentifizierung und Autorisierung

Authentifizierung beantwortet die Frage "Wer bist du?" und validiert die Identität einer Person oder eines Systems. Autorisierung definiert, welche Aktionen oder Ressourcen einem authentifizierten Benutzer gestattet sind und beantwortet die Frage "Was darfst du tun?" - also welche Berechtigungen dieser Benutzer hat. In Kombination gewährleisten Authentifizierung und Autorisierung einen sicheren Zugriff auf Systeme und Daten.

### Implementierte Authentifizierung

In der Insecure App haben wir zur Authentifizierung haben wir zum einen seit der ersten Version eine einfache Authentifizierung mit einem Passwort. Das bedeutet wenn jemand dein Passwort kennt kann er auf alle deine Daten in der Insecure App zugreifen. Daher haben wir eine 2-Faktor-Authentifizierung eingerichtet basierend auf dem Google-Authenticator. Wenn der Nutzer also die 2FA aktiviert hat auf seinem Konto reicht das Passwort alleine nicht mehr um sich einzuloggen. Der Nutzer muss jedes mal beim Einloggen einen, von der Google-Authenticator App generierten, neuen 6-stelligen Code eingeben.

![Login mit 2-Faktor-Authentifizierung](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/8a164fa4-d5e3-4b7a-8404-0a4c511ab782)

Möchte ein Nutzer die 2FA bei sich einrichten, so muss er zu "Enable 2FA" navigieren und auf den Button mit der Aufschrift "Enable" klicken. Sobald auf den Button geklickt wurde wird ihm ein QR code generiert, den er mit seiner Authenticator App scannen kann. Nach dem Scannen ist das Konto in der App hinterlegt. 

![2FA QR-Code](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/7fbc4282-c7f4-4aa8-a7c8-a69131141310)

### Implementierte Autorisierung

In der Insecure App wird die Autorisierung mit 2 Rollen geregelt, zum einen der Admin, der alles machen darf und dem User, der nurz begrenzte Möglichkeiten besitzt. So wird zum Beispiel im folgenden Beispiel bei der Löschung eines Eintrages überprüft, ob ein Admin den Eintrag löschen möchte oder der Autor des Eintrages selbst. Ist es keiner der beiden Fällen ist es dem Nutzer nicht erlaubt diese Aktion zu vollstrecken. 

```C#
if (!_userService.IsAdmin() && _userService.GetUserId() != news.AuthorId)
{
    return Forbid();
}
```
![Autorisierung im Code](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/83a2eb5b-9239-4f58-8df2-c7c546152cd0)

## Handlungsziel 4

In den meisten Fällen, in denen Konten gehackt werden, liegt es nicht an Sicherheitslücken des Systems, sondern an den Menschen. Nutzt jemand das Passwort "Hallo123" ist dies nicht sehr sicher, da es ein sehr verbreitetes Passwort ist, nicht sehr lange ist und ebenfalls aus einem klaren Wort besteht. Stattdessen sollte man ein möglichst langes Passwort verwenden mit zufälligen Zeichenfolgen bestehend aus Kleinbuchstaben, Grossbuchstaben, Zahlen und Sonderzeichen. Jedes Passwort ist Knackbar, die Frage ist nur wie lange man dafür braucht.
In der Insecure App haben wir eine kleine Funktion erstellt, welche den Nutzer ermutigen soll ein sicheres Passwort zu wählen. 

![Code der Passwortüberprüfung](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/4b99391e-895a-4fab-85ec-20a7c97b44dd)

Ebenfalls sollte der Key für den JWT auch nicht in unserem öffentlichen Github Repository sichtbar sein, weshalb wir diesen im Secret Manager-Tool von ASP.NET ablegen sollten. diese Files werden nicht in das Repository geladen. In unserem veröffentlichten Code steht nur eine Referenz zu diesem Secret, womit aussenstehnde aber nichts anfangen Können. Dies Funktioniert nach dem gleichen prinzip wie die ".env" Files.

## Handlungsziel 5
