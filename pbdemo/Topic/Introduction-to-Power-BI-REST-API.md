---
title: Einf&#252;hrung in die Power&#160;BI-REST-API
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9e115235-3b5a-4dcd-9526-bf8142c6c1f4
robots: noindex,nofollow
---
# Einf&#252;hrung in die Power&#160;BI-REST-API
---

Die Power BI-Entwicklererfahrung (Preview) ist eine REST-API zum Übertragen von Datenquellen in Power BI.
Wenn eine Anwendung einem Dataset Zeilen hinzufügt, werden die Kacheln auf dem Dashboard automatisch mit den neuen Daten aktualisiert.

In früheren Versionen von Power BI waren alle Modelle und Berichte mit einer Datenquelle verbunden, von der sie Daten beziehen.
Die Daten mussten regelmäßig aktualisiert werden, um neue Daten zu sehen.
Mit der Power BI-REST-API überträgt Power BI Daten, sobald diese verfügbar sind. Auf diesen Daten basierende Berichte werden in Echtzeit aktualisiert.
Somit entfällt die Wartezeit des Pull-Modells. Zudem wird sichergestellt, dass sich die Daten stets auf dem aktuellen Stand befinden.

##Was kann ich mit der Power BI-REST-API tun?

- [Erstellen eines Datasets](Create-Dataset.md)
- [Hinzufügen von Zeilen](Add-Rows.md) zu oder [Löschen von Zeilen](Delete-Rows.md) aus einem Dataset, um eine Kachel automatisch mit neuen Daten zu aktualisieren
- [Abrufen von Datasets](Get-Datasets.md)
- [Abrufen von Tabellen](Get-Tables.md)
- [Aktualisieren eines Tabellenschemas](Update-Table-Schema.md)
- [Abrufen von Gruppen](Get-Groups.md)

##Power BI-REST und JSON

Mit REST stellt Power BI die Power BI-Web-API dar und mit JSON werden Objekte in Power BI beschrieben.

REST ist eine der gängigeren Architekturen für Web-Programmierprojekte. Sie zeichnet sich durch ihre hohe Bedienfreundlichkeit und die Unterstützung vorhandener Webprotokolle wie HTTP aus.


JSON (JavaScript Object Notation) ist ein offenes Standardformat, das lesbaren Text verwendet, um Datenobjekte zu übertragen, die aus Attribut/Wert-Paaren bestehen.
Es dient in erster Linie zum Übertragen von Daten zwischen einem Server und einer Webanwendung, beispielsweise in einer REST-Implementierung.
Sie beschreiben Objekte in Power BI mit JSON.

- Weitere Informationen zu REST finden Sie auf [Wikipedia](http://en.wikipedia.org/wiki/Representational_state_transfer).
- Weitere Informationen zu JSON finden Sie unter [Einführung in JSON](http://json.org/).

**Einfaches JSON-Datenformat**

    {
        "name": "SalesMarketing",
        "tables": [
            {
                "name": "Product",
                "columns": [
                {
                    "name": "ProductID",
                    "dataType": "int"
                },
                {
                    "name": "Manufacturer",
                    "dataType": "string"
                },
                {
                    "name": "Category",
                    "dataType": "string"
                },
                {
                    "name": "Segment",
                    "dataType": "string"
                },
                {
                    "name": "Product",
                    "dataType": "string"
                },
                {
                    "name": "IsCompete",
                    "dataType": "bool"
                }
                ]
            }
        ]
    }

##Power BI-REST-URLs

Power BI-REST-Ressourcen werden für die folgenden Power BI-Funktionen zur Verfügung gestellt:

**Stammressourcen oder -objekte**

    http(s)://api.powerbi.com/<Tenant>/<root>

**Beispiel**

    https://api.powerbi.com/v1.0/myorg

**Sammlung von Ressourcen oder Objekten**

    http(s)://api.PowerBI.com/<Tenant>/<collection>/<key>

**Beispiel**

    https://api.powerbi.com/v1.0/myorg/datasets

**Ein Objekt**

    https://api.powerbi.com/v1.0/myorg/datasets/{dataset_guid}/tables/Product/rows

**Gruppen**

    https://api.powerbi.com/v1.0/myorg/groups/{group_guid}/datasets/{dataset_guid}

**Vorgänge (wenn nicht von Verb unterstützt)**

    http(s)://api.powerbi.com/<Tenant>/<root>/<Operation>

Dabei gilt Folgendes:

- **Mandant**: Der Mandant für diese Power BI-Instanz.
    Als Verknüpfung kann der Wert "Myorg" angegeben werden.
    In diesem Fall verwenden Sie den Mandanten, der in dem mit der Anforderung gesendeten Authentifizierungstoken bereitgestellt wird.

- **Stamm**: Ein Stammobjekt wie etwa Benutzer, Tabelle oder Kachel.

- **Sammlung**: Der Name der Sammlung wie etwa Benutzer, Tabellen oder Kacheln.

- **Schlüssel**: Der eindeutige Bezeichner eines Objekts in einer Sammlung.

- **Vorgang**: Der auszuführende Vorgang wie etwa Aktualisieren oder Zusammenführen.

###Verben

Vorgänge werden von Verben unterstützt, die konsistente Funktionalität für unterschiedliche Endpunkte bieten.


**Verben für Power BI-REST**

<table>
  <tr>
    <td>
      <strong>Verb</strong>
    </td>
    <td>
      <strong>Beschreibung </strong>
    </td>
  </tr>
  <tr>
    <td>GET</td>
    <td>
      <p>Ruft den Wert eines Objekts oder einer Sammlung aus dem angegebenen Inhaltstyp des Aufrufers ab, sofern zutreffend: </p>
      <ul>
        <li>application/xml  </li>
        <li>application/json</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>PUT</td>
    <td>Ersetzt ein Objekt oder erstellt bei Bedarf ein neues benanntes Objekt. </td>
  </tr>
  <tr>
    <td>DELETE</td>
    <td>Löscht ein Objekt.</td>
  </tr>
  <tr>
    <td>POST</td>
    <td>Erstellt ein neues unbenanntes Objekt oder wird an ein vorhandenes Objekt angefügt. Gibt die Position des erstellten Objekts an.  </td>
  </tr>
</table>

##Verwandte Themen

- [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md)
- [Authentifizieren mit Power BI](http://go.microsoft.com/fwlink/?LinkId=519359)
- [Registrieren einer App](http://go.microsoft.com/fwlink/?LinkId=519361)
- [Begriffe der Power BI-API](https://powerbi.microsoft.com/en-us/api-terms)




