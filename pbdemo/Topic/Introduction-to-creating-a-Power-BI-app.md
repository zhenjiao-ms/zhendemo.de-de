---
title: Einf&#252;hrung in das Erstellen einer Power BI-App
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09cf9c5a-517b-4e5a-856d-81c2aed8e032
robots: noindex,nofollow
---
# Einf&#252;hrung in das Erstellen einer Power BI-App
---

Mit der Power BI-REST-API können Sie Ihre eigene Unternehmenslösung erstellen, um Daten eines beliebigen Typs in Echtzeit in ein Power BI-Dashboard zu übertragen.
Ihre Dashboards werden bei Änderungen der Daten in Echtzeit aktualisiert.
Sie können zum Schreiben der App eine beliebige Technologie wie etwa .NET, JQuery oder Ruby verwenden.
Die Power BI-REST-API verwendet Industriestandards wie das OAuth2-Protokoll. Zudem beinhaltet sie REST-Vorgänge für die Echtzeitübertragung von Daten in ein Power BI-Dashboard.

###Nachfolgend sind die Voraussetzungen für das Erstellen einer Power BI-App angegeben.

| Voraussetzung| Beschreibung|
|-|-|
| [Azure Active Directory-Mandant](Create-an-Azure-Active-Directory-tenant.md)| Power BI-Apps sind in Azure Active Directory (Azure AD) integriert, um eine sichere Anmeldung und Autorisierung Ihrer App zu ermöglichen.|
| [Registrieren Ihrer Client-App](Register-a-client-app.md) oder [Registrieren Ihrer Web-App](Register-a-web-app.md)| Damit Ihre Anwendung auf die Power BI-REST-API zugreifen kann, müssen Sie sie in Azure Active Directory registrieren.|
| [Authentifizieren Ihrer App](Authenticate-to-Power-BI-service.md)| Power BI-Client-Apps verwenden Active Directory (AAD) zum Authentifizieren von Benutzern und zum Schutz von Anwendungen.Erfahren Sie, wie Sie [eine Client-App authentifizieren](Authenticate-a-client-app.md) oder [eine Web-App authentifizieren](Authenticate-a-web-app.md).|
| [Aufrufen von Power BI-REST-Vorgängen](Power-BI-REST-API-reference.md)| Mit der Power BI-REST-API können Sie Datasets erstellen und abrufen, Tabellenschemas abrufen und aktualisieren sowie Zeilen hinzufügen und löschen.<br/><br/> **Hinweis** Sie können jeden beliebigen Power BI REST-Vorgang für eine [Gruppe](Get-Groups.md) aufrufen.|
<a name="QuickStarts"/>
###Schnellstarts zum Aufrufen der einzelnen Power BI-REST-Vorgänge

| Schnellstartbeispiel| Beschreibung|
|-|-|
| [Beispiel für Create Dataset](Create-Dataset.md#example)| Hier sehen Sie, wie Sie ein Dataset erstellen.|
| [Beispiel für Get Datasets](Get-Datasets.md#example)| Hier sehen Sie, wie Sie ein Dataset abrufen. Dies umfasst auch das **Deserialisieren** der JSON-Antwort mit der **JavaScriptSerializer**-Klasse.|
| [Beispiel für Get Tables](Get-Tables.md#example)| Hier sehen Sie, wie Sie Dataset-Tabellen abrufen.Außerdem wird gezeigt, wie Sie eine Dataset-ID mithilfe einer LINQ-Abfrage abrufen und die JSON-Antwort mit der **JavaScriptSerializer**-Klasse **deserialisieren**.|
| [Beispiel für Update Table Schema](Update-Table-Schema.md#example)| Hier sehen Sie, wie Sie ein Tabellenschema aktualisieren.Außerdem wird gezeigt, wie Sie mithilfe einer LINQ-Abfrage eine Dataset-ID abrufen.|
| [Beispiel für Add Rows](Add-Rows.md#example)| Hier sehen Sie, wie Sie Zeilen zu einem Dataset hinzufügen.Außerdem wird gezeigt, wie Sie mithilfe einer LINQ-Abfrage eine Dataset-ID abrufen.|
| [Beispiel für Delete Rows](Delete-Rows.md#example)| Hier sehen Sie, wie Sie Zeilen aus einem Dataset löschen.Außerdem wird gezeigt, wie Sie mithilfe einer LINQ-Abfrage eine Dataset-ID abrufen.|
| [Beispiel für Get Groups](Get-Groups.md#example)| Hier sehen Sie, wie Sie Gruppen abrufen, denen der angemeldete Benutzer angehört.Außerdem wird gezeigt, wie Sie die JSON-Antwort mit der **JavaScriptSerializer**-Klasse **deserialisieren** .|
###Power BI-Beispiele

| Power BI-Beispiel| Beschreibung|
|-|-|
| [Beispiel für Client-App](Power-BI-client-app-sample.md)| Hier sehen Sie, wie Sie ein Zugriffstoken für eine Client-App abrufen und alle Power BI-REST-Vorgänge ausführen.|
| [Beispiel für ASP.NET-Web-App](Power-BI-web-app-sample.md)| Hier sehen Sie, wie Sie ein Zugriffstoken für eine Web-App abrufen und einen Power BI-REST-Vorgang ausführen.|


