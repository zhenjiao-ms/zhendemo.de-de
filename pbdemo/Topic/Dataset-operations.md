---
title: Dataset-Vorg&#228;nge (Dataset operations)
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d25be11a-ee96-47eb-b808-c070145329fb
---
# Dataset-Vorg&#228;nge (Dataset operations)
---

Ein **Dataset**-Ressourcenpfad ist eine Sammlung von **Tabellen** im Besitz eines signierten Power BI-Benutzers.
Eine **Tabelle** weist **Zeilen** und **Spalten** auf, die Daten enthalten.
Mit der Power BI-Rest-API können Sie neue Datasets erstellen und in diesen Datasets Daten zu den Tabellen hinzufügen.
Ein Dashboard, das diese Datasets verwendet, aktualisiert automatisch, sobald neue Daten verfügbar sind.

Die Power BI-REST-API bietet die folgenden **Dataset**-Vorgänge:
- [Get Datasets](Get-Datasets.md): Gibt eine JSON-Liste aller **Dataset**-Objekte zurück, einschließlich eines Namens und einer ID.
- [Create Dataset](Create-Dataset.md): Erstellt ein neues **Dataset**-Objekt aus einer JSON-Schemadefinition.

###Dataset-JSON

Ein **Dataset** verfügt über eine Sammlung von **Tabellenobjekten**.

    {
       "name": "dataset_name",
       "tables":[
        …
        ]
    }

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

##Siehe auch

- [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md)
- [Referenz für Power BI-REST-API](Power-BI-REST-API-reference.md)
- [Übersicht über Power BI-REST-API](Overview-of-Power-BI-REST-API.md)



