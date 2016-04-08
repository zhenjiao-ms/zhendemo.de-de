---
title: Beispiel f&#252;r Power BI-Web-App
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d8402b48-3f71-4c42-996d-7a5310dbb480
---
# Beispiel f&#252;r Power BI-Web-App
---

[Web-App-Beispiel herunterladen](http://go.microsoft.com/fwlink/?LinkId=619279) | Code auf GitHub anzeigen: [Default.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619431) | [Redirect.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619432)

**Wichtig** Um das Beispiel für eine Web-App auszuführen, müssen Sie das Web-App-Beispiel in **Azure Active Directory** (Azure AD) registrieren und eine **Client-ID** sowie einen **geheimen Clientschlüssel** abrufen.
Die **Client-ID** und der **geheime Clientschlüssel** werden zur Authentifizierung bei ** Azure AD ** verwendet.
Informationen dazu finden Sie unter [Ausführen des Beispiels für eine Web-App](#run).
<a name="run"/>
##Ausführen des Beispiels für eine Web-App

Das Beispiel für eine Web-App zeigt, wie Sie eine Power BI-Web-App bei **Azure Active Directory** authentifizieren und einen Power BI-REST-Aufruf zum Abrufen aller Datasets ausführen.


**Ausführlichere Informationen zu dem Beispiel für eine Web-App sind unter dem Thema [Authentifizieren einer Web-App](Authenticate-a-web-app.md) angegeben, oder zeigen Sie den Code auf GitHub an: [Default.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619431)|[Redirect.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619432)**

Zum Ausführen des Beispiels ist Folgendes erforderlich:

1. [Herunterladen des Web-App-Beispiels](http://go.microsoft.com/fwlink/?LinkId=619279)
2. [Registrieren des Web-App-Beispiels in Azure Active Directory](#register)
3. [Festlegen der Client-ID und des geheimen Clientschlüssels, die Sie beim Registrieren der Beispiel-App erhalten haben](#set)
4. [Umleitungs-URL auf den Umleitungs-URL für Ihre Webb-App festlegen](#redirect)

<a name="register"/>
###Registrieren des Web-App-Beispiels in Azure Active Directory

Zum Aufrufen eines Power BI-REST-Vorgangs in einer Web-App brauchen Sie eine **Client-ID** und einen **geheimen Clientschlüssel** für das Authentifizieren bei ** Azure AD **, damit die App auf Power BI-REST-Ressourcen zugreifen kann.
Um eine **Client-ID** und einen **geheimen Clientschlüssel** zu erhalten, registrieren Sie die Beispiel-App im **Windows Azure-Verwaltungsportal**.
Bevor Sie die Beispiel-App registrieren, benötigen Sie die folgenden Einstellungen:

####Einstellungen für die Registrierung des Web-App-Beispiels

| Einstellung| Wert| Beschreibung
|-|-|-|
| Name| PowerBIWebSample| Dies kann ein beliebiger eindeutiger Name sein.|
| Anmelde-URL| http://localhost:13526| Dies ist Ihre Web-App-URL.|
| App-ID-URI| https://{Ihre Mandanten-ID}.onmicrosoft.com/PowerBIWebSample| Dies ist Ihre Azure-Mandanten-URL gefolgt vom Namen Ihrer App.Beispiel: https://IhrMandant.onmicrosoft.com/IhreWebApp|
| Berechtigungen| Alle Datasets anzeigen| Da mit dem Web-App-Beispiel nur Datasets abgerufen werden, benötigen Sie nur die Berechtigung **Alle Datasets anzeigen**.Weitere Informationen zu Power BI-Berechtigungen finden Sie unter [Power BI-Berechtigungen](Power-BI-permissions.md).|
####Registrieren des Beispiels für eine Web-App

Informationen zum Registrieren des Beispiels für eine Web-App finden Sie unter [Registrieren einer Web-App](Register-a-web-app.md). Verwenden Sie dabei die oben genannten Einstellungen.
Nachdem Sie das Beispiel für eine Web-App registriert haben, generiert **Azure AD** die **Client-ID** für die App.
Für den **geheimen Clientschlüssel** müssen Sie die **Dauer** festlegen.
Nachdem Sie die App im **Azure-Verwaltungsportal** gespeichert haben, generiert Azure AD einen **geheimen Clientschlüssel**.
Erstellen Sie unbedingt eine Kopie des Schlüssels, da er ansonsten für zukünftige Navigationsvorgänge auf der App-Konfigurationsseite nicht verfügbar ist.

Um das Beispiel auszuführen, müssen Sie die Eigenschaften **ClientID** und **ClientSecret** in **web.config** festlegen.
Sie können die **Client-ID** von der Seite **Azure-Anwendungskonfiguration** abrufen.
Informationen finden Sie unter [Abrufen einer Web-App-ID](Register-a-web-app.md#clientID).
Erstellen Sie unbedingt eine Kopie des **geheimen Clientschlüssels**, da er ansonsten für zukünftige Navigationsvorgänge auf der Konfigurationsseite nicht verfügbar ist.
<a name="set"/>
###Festlegen der Client-ID und des geheimen Clientschlüssels, die Sie beim Registrieren der Beispiel-App erhalten haben

Um das Web-App-Beispiel auszuführen, müssen Sie die Eigenschaften **ClientID** und **ClientSecret** in **web.config** wie folgt festlegen:

      <applicationSettings>
        <PBIWebApp.Properties.Settings>
          <setting name="ClientID" serializeAs="String">
            <value>{Client ID from Azure AD app registration}</value>
          </setting>
          <setting name="ClientSecretKey" serializeAs="String">
            <value>{Client Secret Key from Azure AD app registration}</value>      
      </setting>
        </PBIWebApp.Properties.Settings>
      </applicationSettings>

<a name="redirect"/>
###Umleitungs-URL auf den Umleitungs-URL für Ihre Webb-App festlegen

Im Beispiel wird für die Ausführung auf „localhost“ konfiguriert.
Wenn Sie eine Web-App entwickeln, stellen Sie jedoch sicher, dass Sie die **Umleitungs-URL** in der Datei „web.config“ auf die Web-App-URL ändern.
Beispielsweise „http://{web_app_name}.azurewebsites.net/Redirect“.

          <setting name="RedirectUrl" serializeAs="String">
            <value>http://{web_app_url}/Redirect</value>
          </setting>

<br/>
Nun können Sie das Beispiel für eine Web-App ausführen.

##Verwandte Themen

- [Registrieren einer Web-App](Register-a-web-app.md)
- [Authentifizieren einer Web-App](Authenticate-a-web-app.md)
- [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md)
- [Referenz für Power BI-REST-API](Power-BI-REST-API-reference.md)




