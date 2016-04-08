---
title: Create Import
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55c9125f-969b-4cbf-b97c-e1eafc466aef
---
# Create Import
---

[Anforderung](#request) | [Antwort](#response) | **[Vorschauvorgang]**
<a name="top"/>

Der Vorgang **Create Import (Imports erstellen)** erstellt Inhalte wahlweise aus einer PBIX-, einer Excel-, einer Paketdatei oder einem Dateipfad in OneDrive Pro und gibt die ID für den erstellten **Import** zurück.
Dadurch können Sie ein Inhaltspaket, wie etwa eine PBIX- oder Excel-Datei in einen Power BI-Arbeitsbereich importieren.
Die **Imports**-API teilt das PBIX-Paket in einzelne Pakete auf, die Datasets und Berichte beinhalten.
Anschließend wird ein Metadatenlink zwischen dem eigentlichen Importvorgang und den aus ihm erstellten Objekten bereitgestellt.

**Hinweis** Der Vorgang bewirkt das gleiche Verhalten wie beim Importieren einer Excel-Datei mithilfe des Bildschirms zum Herstellen von Verbindungen in Power BI.

**Erforderlicher Bereich**: Content.Create 
<a name="request"/>
##Anforderung

POST https://api.powerbi.com/beta/myorg/imports

###Abfragezeichenfolgenparameter

<table><tr><td><b>Name</b></td><td><b>Typ</b></td><td><b>Beschreibung</b></td><td><b>Werte</b></td></tr><tr><td>datasetDisplayName</td><td>Zeichenfolge</td><td>Wenn Sie eine PBIX- oder Excel-Datei verwenden, überschreibt das Einstellen dieses Parameters den Standardnamen des Datasets.</td><td>Ein Dataset-Name.</td></tr><tr><td>nameConflict</td><td>Zeichenfolge</td><td>Bestimmt, was geschehen soll, wenn ein Dataset mit dem gleichen Namen vorhanden ist.</td><td>-Ignorieren (Standard) <br/>
- Abbrechen <br/>
- Überschreiben</td></tr></table>

###Header

Inhaltstyp: multipart/form-data 
Autorisierung: Träger eyJ0eX ... FWSXfwtQ

###Anforderungstext

-   Beim Importieren aus einer Datei enthält der Textkörper eine PBIX- oder Excel-Datei, [die als Formulardaten kodiert](http://www.w3.org/TR/html401/interact/forms.html) ist.

-   Beim Importieren von OneDrive Pro enthält der Textkörper JSON-Eigenschaften, die einen relativen Pfad zur URL der Datei beinhalten.
- Der „filePath“ verwendet den [Pfad der OneDrive-Datei-API](https://msdn.microsoft.com/office/office365/APi/files-rest-operations#FileResource).


Beispiel:

```
    filePath = shared%20with%20everyone/Denver%20Data.xlsx 
```

###Textschema

Eine JSON-Liste von **Datasets** und Eigenschaften, einschließlich Name und ID.
Ferner wird eine ID für den **Import** zurückgegeben.

Ein **Import** weist einen **filePath** auf.

    {
      "filePath": "shared with everyone/FileName.xlsx"
    }

<a name="response"/>
##Antwort

###Antwort-Header

- Location-Header, der auf den Speicherort der Importstatusinformationen verweist.

- Retry-After-Header, der die Anzahl der Sekunden angibt, die der Client bis zur nächsten Anforderung warten soll

###Erfolgsstatuscode

<table>
  <tr>
    <td>
      <b>Code</b>
    </td>
    <td>
      <b>Beschreibung</b>
    </td>
  </tr>
  <tr>
    <td>201</td>
    <td>Erstellt. Die Anforderung wurde erfüllt und ein neuer Import erstellt.</td>
  </tr>
  <tr>
    <td>202</td>
    <td>Akzeptiert, wenn der Import noch ausgeführt wird.</td>
  </tr>
</table>

###Fehlerstatuscode

<table>
  <tr>
    <td>
      <b>Code</b>
    </td>
    <td>
      <b>Beschreibung</b>
    </td>
  </tr>
  <tr>
    <td>409</td>
    <td>Die importierte Datei ist bereits vorhanden.</td>
  </tr>
</table>

###Inhaltstyp

application/json


###Textschema

Eine Antwort vom Typ **Create Import** weist das folgende Schema auf:

    {"id": "{import_guid"}


