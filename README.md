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


