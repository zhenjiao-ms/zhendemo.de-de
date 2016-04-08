---
title: Power&#160;BI-Berechtigungen
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b19c5d1f-7d7a-4772-a9cd-66abad2bebe6
robots: noindex,nofollow
---
# Power&#160;BI-Berechtigungen
---

Mit Power BI-Berechtigungen kann eine Anwendung bestimmte Aktionen im Namen des Benutzers ausführen.
Berechtigungen sind erst gültig, wenn sie von einem Benutzer genehmigt wurden.

<table>
  <tr>
    <td>
      <b>Anzeigename</b>
    </td>
    <td>
      <b>Beschreibung</b>
    </td>
    <td>
      <b>Bereichswert</b>
    </td>
  </tr>
  <tr>
    <td>Alle Datasets anzeigen</td>
    <td>Die App kann alle Datasets für den angemeldeten Benutzer und Datasets, auf die der Benutzer Zugriff hat, anzeigen.</td>
    <td>Dataset.Read.All</td>
  </tr>
  <tr>
    <td>Alle Datasets lesen und schreiben</td>
    <td>Die App kann alle Datasets für den angemeldeten Benutzer und Datasets, auf die der Benutzer Zugriff hat, anzeigen und darin schreiben.</td>
    <td>Dataset.ReadWrite.All</td>
  </tr>
  <tr>
    <td>Benutzergruppen anzeigen</td>
    <td>Die App kann alle Gruppen anzeigen, denen der angemeldete Benutzer angehört.</td>
    <td>Group.Read.All</td>
  </tr>
</table>
Eine Anwendung kann bei der ersten Anmeldung auf der Seite eines Benutzers Berechtigungen anfordern. Die angeforderten Berechtigungen werden im Bereichsparameter des Aufrufs übergeben.
Wenn die Berechtigungen gewährt werden, wird ein Zugriffstoken an die App zurückgegeben, das für zukünftige API-Aufrufe verwendet werden kann.
Der Zugriff ist nur für eine bestimmte Anwendung möglich.

##Anfordern von Berechtigungen

Sie können die API aufrufen, um sich mit einem Benutzernamen und einem Kennwort zu authentifizieren und im Namen eines anderen Benutzers Aktionen ausführen. Zu diesem Zweck müssen jedoch Berechtigungen angefordert und vom Benutzer genehmigt werden. Daraufhin wird bei allen zukünftigen Aufrufen das zurückgegebene Zugriffstoken gesendet.
Bei diesem Vorgang befolgen wir das Standardprotokoll [OAuth 2.0](http://oauth.net/2/).
Während die tatsächliche Implementierung variieren kann, beinhaltet der OAuth-Fluss für Power BI die folgenden Elemente:

*   **Anmeldebenutzeroberfläche**: Über diese Benutzeroberfläche kann der Entwickler Berechtigungen anfordern.
    Der Benutzer muss sich hierfür anmelden, sofern er dies nicht bereits getan hat.
    Zudem muss der Benutzer die von der Anwendung angeforderten Berechtigungen genehmigen.
    Das Anmeldefenster sendet einen Zugriffscode oder eine Fehlermeldung an eine festgelegte Umleitungs-URL zurück.
    
    *   In Power BI sollte für systemeigene Anwendungen eine Standard-Umleitungs-URL angegeben werden.
*   **Autorisierungscode**: Autorisierungscodes werden nach der Anmeldung über URL-Parameter in der Umleitungs-URL für Webanwendungen zurückgegeben.
    Dass sie in Parametern enthalten sind, birgt ein gewisses Sicherheitsrisiko.
    Webanwendungen müssen den Autorisierungscode gegen ein Autorisierungstoken austauschen.
*   **Autorisierungstoken**: Werden verwendet, um API-Aufrufe im Namen eines anderen Benutzers zu authentifizieren.
    Sie sind auf eine bestimmte Anwendung begrenzt.
    Token haben eine feste Lebensdauer und müssen nach Ablauf aktualisiert werden.
*   **Aktualisierungstoken**: Token werden nach ihrem Ablauf aktualisiert.


