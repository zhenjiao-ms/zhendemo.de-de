---
title: Tabellenvorg&#228;nge (Table operations)
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ebd7c9ea-999c-4727-a7f6-fe145ad0ebd7
robots: nofollow
---
# Tabellenvorg&#228;nge (Table operations)
---

Ein **Tabellen**-Ressourcenpfad ist eine Sammlung von **Tabellen**-Objekten eines bestimmten **Datasets**.
Eine **Tabelle** weist **Zeilen** und **Spalten** auf, die Daten enthalten.
Eine **Spalte** weist einen Namen und einen **Datentyp** auf.

Die Power BI-REST-API bietet die folgenden **Tabellenvorgänge**:

- [Get Tables](Get-Tables.md): Gibt eine JSON-Liste von **Tabellen** für das angegebene **Dataset** zurück.
- [Update Table Schema](Update-Table-Schema.md): Aktualisiert das Schema einer vorhanden **Tabelle**.

###Tabellen-JSON

**Tabellen** verfügen über einen Namen und eine Sammlung von **Spaltenobjekten**.
Eine **Spalte** weist einen **Namen** und einen **Datentyp** auf.

    {
         "name": "table_name",
         "columns":[
            {
               "name": "column_name",
               "dataType": "data_type"
            },
        …
        ]
    }

###Siehe auch

- [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md)
- [Referenz für Power BI-REST-API](Power-BI-REST-API-reference.md)
- [Übersicht über Power BI-REST-API](Overview-of-Power-BI-REST-API.md)



