# LB_183_Portfolio_VontobelJan

## Handlungsziel 1
In der Insecure App können Nutzer ohne die nötigen Rechte zu besitzen alle Beiträge bearbeiten, sobald sie die ID eines Eintrages kennen. Der Nutzer kann zwar nicht direkt auf der Insecure App Seite die Daten verändern, aber ganz einfach mit Tools wie Swagger oder Postman. Auf diese Weise können die Rollen sehr einfach umgangen werden und könnten auch weggelassen werden, wenn dies nicht behoben wird. In der Theorie könnte ein Nutzer alle Beträge löschen und damit einen grossen Schaden anrichten.
![Code Beispiel](https://github.com/SpogotenauPOGGERS/JanVontobel_LB_183/assets/89130699/0ba7d5df-4d50-4df9-8e7d-37cd4d8ece18)
In dieser Patchanfrage müsste zusätzlich noch überprüft werden, ob der Nutzer die nötige Berrechtigung dazu hat.
