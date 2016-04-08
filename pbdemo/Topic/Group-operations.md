---
title: Gruppenvorg&#228;nge (Group operations)
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 87c5bab7-de5d-44cd-908a-e187e26abb9a
---
# Gruppenvorg&#228;nge (Group operations)
---

Eine Power BI-App kann Inhalte im persönlichen Arbeitsbereich des Benutzers bereitstellen und dieselben REST-API-Aufrufe an alle Gruppen durchführen, auf die der Benutzer Zugriff hat.
Senden Sie die POST-Anforderung weiterhin an https://api.powerbi.com/v1.0/myorg/datasets, um ein Dataset im persönlichen Arbeitsbereich des Benutzers bereitzustellen.
Wenn Sie dieses Dataset der Gruppe zur Verfügung stellen wollen, in der der Benutzer Mitglied ist, senden Sie die POST-Anforderung an https://api.powerbi.com/v1.0/myorg/groups/{Gruppen-ID}/datasets.


Um die Gruppen-ID zum Einfügen in die URL abzurufen, rufen Sie den Vorgang **Get Groups** auf.

    GET https://api.powerbi.com/v1.0/myorg/groups

Die Power BI-REST-API bietet die folgenden **Gruppen**vorgänge:

- [Get Groups](Get-Groups.md) Gibt eine JSON-Liste aller **Gruppen**objekte zurück.

###Siehe auch

- [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md)
- [Referenz für Power BI-REST-API](Power-BI-REST-API-reference.md)
- [Übersicht über Power BI-REST-API](Overview-of-Power-BI-REST-API.md)



