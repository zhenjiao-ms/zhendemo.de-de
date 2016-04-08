---
title: Einschr&#228;nkungen f&#252;r Power&#160;BI-REST-API
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3cdde6a-0df8-4341-bace-0f24e57a8487
---
# Einschr&#228;nkungen f&#252;r Power&#160;BI-REST-API
---

Für die Power BI-REST-API gelten die folgenden Einschränkungen.

**Bereitstellen von Zeilen mit POST**

*   Max. 10.000 Zeilen pro einzelner POST-Zeilenanforderung
*   1.000.000 Zeilen pro Stunde und Dataset hinzugefügt
*   Max. 5 ausstehende POST-Zeilenanforderungen pro Dataset
*   120 POST-Zeilenanforderungen pro Minute und Dataset
*   Max. 200.000 Zeilen pro Tabelle im FIFO-Dataset gespeichert
*   Max. 5.000.000 Zeilen pro Tabelle in einem Dataset ohne Aufbewahrungsrichtlinie gespeichert
*   32.766 Zeichen pro Wert für Zeichenfolgenspalten in POST-Zeilenvorgängen

**POST-Zeilenvorgänge pro Power BI-Plan**

*   Dataset von einem Benutzer mit kostenlosem Dienstplan erstellt: 10.000 Zeilen pro Stunde und Dataset hinzugefügt
*   Dataset von einem Benutzer mit kostenpflichtigem Dienstplan erstellt: 1.000.000 Zeilen pro Stunde und Dataset hinzugefügt
*   Wenn ein Benutzer diesen Grenzwert überschreitet, scheitern alle weiteren API-Aufrufe mit den folgenden Details:
    
    *   HTTP-Statuscode: 429 Zu viele Anfragen
    *   Antwort-Header: Retry-After + {Anzahl der Sekunden, bis das Kontingent zurückgesetzt wird}
    *   Ein OData-Fehler mit folgender Meldung: "Sie haben die maximalen Zeilen pro Stunde für dieses Dataset überschritten.
        Um mehr Zeilen pro Stunde zu verschieben, führen Sie ein Upgrade Ihres Kontos auf Power BI Pro aus oder wiederholen Sie den Pushvorgang später."


