---
title: Zeilenvorg&#228;nge (Row operations)
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cf65ea37-e8c7-4b9c-8536-8e60e8555350
---
# Zeilenvorg&#228;nge (Row operations)
---

Ein **Zeilen**ressourcenpfad ist eine Auflistung von Zeilen für eine **Tabelle**.

Die Power BI-REST-API bietet die folgenden **Zeilen**vorgänge:

- [Add Rows (Zeilen hinzufügen)](Add-Rows.md): Fügt **Zeilen** zu einer **Tabelle** in einem **Dataset** hinzu.
- [Delete Rows](Delete-Rows.md): Löscht **Zeilen** aus einer **Tabelle** in einem **Dataset**.

###Zeilen-JSON

**Zeilen** umfassen eine Sammlung von **Spalten**namen und Werten.

    {
        "rows":
            [
                {"column_name1": value, "column_name2": value, "column_name3": value, ...}
            ]
    }

### Siehe auch
- [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md)
- [Referenz für Power BI-REST-API](Power-BI-REST-API-reference.md)
- [Übersicht über Power BI-REST-API](Overview-of-Power-BI-REST-API.md)



