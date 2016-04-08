---
title: Beispiel f&#252;r Power BI-Client-App
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e52924d-695b-4762-860e-9f8bb1e8b344
---
# Beispiel f&#252;r Power BI-Client-App
---

[Herunterladen des Client-App-Beispiels für erste Schritte mit .NET](http://go.microsoft.com/fwlink/?LinkId=619280) | [Code auf GitHub anzeigen](https://github.com/Microsoft/PowerBI-CSharp/tree/master/samples/consoleapp/getting-started-for-dotnet)

**Wichtig** Um das Beispiel für eine Client-App auszuführen, müssen Sie die Beispiel-App in **Azure Active Directory** (Azure AD) registrieren und eine **Client-ID** abrufen. Die **Client-ID** wird zur Authentifizierung bei ** Azure AD ** verwendet. Informationen dazu finden Sie unter [Ausführen des Beispiels für eine Client-App](#run).

Im Beispiel für eine Power BI-Client-App wird Folgendes veranschaulicht:

- Abrufen eines Zugriffstokens für Azure Active Directory zur Authentifizierung
- Erstellen eines Datasets
- Abrufen aller Datasets
- Hinzufügen von Zeilen zu einem Dataset und Löschen von Zeilen aus einer Datenbank
- Aktualisieren eines Tabellenschemas
- Abrufen von Tabellen
- Abrufen von Gruppen

**Hinweis** Im Beispiel für eine Client-App wird außerdem gezeigt, wie Sie Power BI-REST-Vorgänge für [Gruppen](Get-Groups.md) aufrufen.

**Ausführlichere Informationen zu dem Beispiel für eine Client-App sind unter dem Thema [Authentifizieren einer Client-App](Authenticate-a-client-app.md) angegeben, oder [zeigen Sie den Code auf GitHub an](https://github.com/Microsoft/PowerBI-CSharp/tree/master/samples/consoleapp/getting-started-for-dotnet).**

<a name="run"/>
##Ausführen des Beispiels für eine Client-App

Das Client-App-Beispiel ist eine Konsolenanwendung, die zeigt, wie Sie eine Power BI-Client-App bei Azure Active Directory authentifizieren und alle Power BI-REST-Vorgänge, einschließlich Gruppenvorgängen, aufrufen. Zum Ausführen des Beispiels ist Folgendes erforderlich:

1. [Herunterladen des Client-App-Beispiels für erste Schritte mit .NET von GitHub](https://github.com/Microsoft/PowerBI-CSharp/tree/master/samples/consoleapp/getting-started-for-dotnet)
2. [Registrieren des Client-App-Beispiels in Azure Active Directory](#register)
3. [Festlegen der Client-ID, die Sie beim Registrieren der Beispiel-App erhalten haben](#set)

<a name="register"/>
###Registrieren des Client-App-Beispiels in Azure Active Directory

Zum Aufrufen eines Power BI-REST-Vorgangs in einer Client-App brauchen Sie eine **Client-ID** für das Authentifizieren bei ** Azure AD **, damit die App auf Power BI-REST-Ressourcen zugreifen kann. Um eine **Client-ID** zu erhalten, registrieren Sie die Beispiel-App im **Windows Azure-Verwaltungsportal**. Bevor Sie die Beispiel-App registrieren, benötigen Sie die folgenden Einstellungen:

####Einstellungen für die Registrierung des Client-App-Beispiels

| Einstellung| Wert| Beschreibung
|-|-|-|
| Name| PowerBIClientSample| Dies kann ein beliebiger eindeutiger Name sein.|
| Umleitungs-URI| https://login.live.com/oauth20_desktop.srf| In Bezug auf eine Client-App bietet ein Umleitungs-URI Azure Active Directory ausführlichere Informationen über die zu authentifizierende Anwendung.Sie können diesen Umleitungs-URI für Ihre Client-App verwenden.|
| Berechtigungen| "Alle Datasets anzeigen", "Alle Datasets lesen und schreiben" und "Benutzergruppen anzeigen"| Weitere Informationen zu Power BI-Berechtigungen finden Sie unter [Power BI-Berechtigungen](Power-BI-permissions.md).|
####Registrieren des Beispiels für eine Client-App

Informationen zum Registrieren des Beispiels für eine Client-App finden Sie unter [Registrieren einer Client-App](Register-a-client-app.md). Verwenden Sie dabei die oben genannten Einstellungen. Nachdem Sie das Beispiel für eine Client-App registriert haben, generiert **Azure AD** die **Client-ID** für die App. Zum Ausführen des Beispiels müssen Sie die Variable **clientID** im Beispiel festlegen. Sie können die **Client-ID** von der Seite **Azure-Anwendungskonfiguration** abrufen. Informationen finden Sie unter [Abrufen einer Client-App-ID](Register-a-client-app.md#clientID).
<a name="set"/>
###Festlegen der Client-ID, die Sie beim Registrieren der Beispiel-App erhalten haben

Zum Ausführen des Beispiels müssen Sie die Variable **clientID** im Beispiel festlegen. Legen Sie in „Program.cs“ die Variable **clientID** folgendermaßen fest:

    private static string clientID = "{client id from Azure AD app registration}";

Nun können Sie das Beispiel für eine Client-App ausführen.

##Verwandte Themen

- [Registrieren einer Client-App](Register-a-client-app.md)
- [Authentifizieren einer Client-App](Authenticate-a-client-app.md)
- [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md)
- [Referenz für Power BI-REST-API](Power-BI-REST-API-reference.md)




