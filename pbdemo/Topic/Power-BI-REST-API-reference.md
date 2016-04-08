---
title: Referenz f&#252;r Power&#160;BI-REST-API
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e05cad22-434f-4d4c-90d7-459ea3b255af
---
# Referenz f&#252;r Power&#160;BI-REST-API
---

Die Power BI-REST-API ist eine REST-basierte API, die den programmgesteuerten Zugriff auf **Dashboard**-Ressourcen wie **Datasets**, **Tabellen** und **Zeilen** in Power BI ermöglicht. Power BI ist ein cloudbasierter Dienst, den Sie zum Erstellen einer benutzerdefinierten Dashboard-Anwendung nutzen können. Die API verfügt über die Konzepte, ein **Dataset** per Push-Vorgang an ein **Dashboard** zu übermitteln. Ein **Dataset** ist eine Sammlung von **Tabellen**. Eine **Tabelle** weist Zeilen und Spalten auf, die Daten enthalten.

Die Power BI-REST-API bietet die folgenden Vorgänge:

- [Dataset-Vorgänge](Dataset-operations.md): Abrufen und Erstellen von Datasets.
- [Tabellenvorgänge](Table-operations.md): Abrufen von Tabellen und Aktualisieren des Tabellenschemas.
- [Zeilenvorgänge](Row-operations.md): Hinzufügen von Zeilen und Löschen von Zeilen.
- [Gruppenvorgänge](Group-operations.md): Abrufen von Gruppen.
- [Importvorgänge](Import-operations.md): Erstellen von Importen, Abrufen von Importen, Abrufen von Importen nach GUID und Abrufen von Importen nach Dateipfad.
- [Dashboardvorgänge](Dashboard-operations.md): Dashboards abrufen und Kacheln abrufen.

Zum Ausführen von Vorgängen für Power BI-Ressourcen senden Sie HTTP-Anforderungen mit einer unterstützten Methode (GET, POST, PUT oder DELETE) an einen Endpunkt, der eine Ressourcensammlung oder eine bestimmte Ressource als Ziel hat. Für einen Power BI-Vorgang ist ein Zugriffstoken für Azure Active Directory (AD) erforderlich. Weitere Informationen zur Authentifizierung für den Power BI-Dienst finden Sie unter [Authentifizieren für Power BI-Dienst](Authenticate-to-Power-BI-service.md).

##Verwandte Themen

- [Einführung in die Power BI-REST-API](Introduction-to-Power-BI-REST-API.md)
- [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md)






