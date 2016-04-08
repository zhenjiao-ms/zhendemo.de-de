---
title: Get Import by File Path
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e31b2a38-9653-4a9c-9920-37ce2b116032
---
# Get Import by File Path
---

[Anforderung](#request) | [Antwort](#response) | **[Vorschauvorgang]**
<a name="top"/>

Der Vorgang **Get Import by File Path (Import nach Dateipfad abrufen)** gibt einen JSON-Textkörper für ein **Import**objekt mit seinen Eigenschaften zurück, einschließlich ID, Importquelle, **Berichten** und **Datasets**.
Schließen Sie den **Importdateipfad** in den filePath-Parameter der Abfragezeichenfolge des GET-URIs ein, um ein **Import**-Objekt zurückzugeben.

**Erforderlicher Bereich**: Metadata.View_Any
<a name="request"/>
##Anforderung

GET https://api.powerbi.com/beta/myorg/imports?filePath={filePath}

###Abfragezeichenfolgenparameter

<table>
  <tr>
    <td>
      <b>Name</b>
    </td>
    <td>
      <b>Beschreibung</b>
    </td>
  </tr>
  <tr>
    <td>filePath</td>
    <td>Der Pfad zur Importdatei.</td>
  </tr>
</table>

###Beispiele für die Anforderung

    GET http://api.powerBI.com/beta/myorg/imports?filePath=http://microsoft-my.sharepoint.com/personal/{user name}/shared%20with%20everyone/mydoc.xlsx

###Header

Inhaltstyp: application/json
Autorisierung: Träger eyJ0eX ... FWSXfwtQ   
<a name="response"/>

##Antwort

###Statuscode

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
    <td>200</td>
    <td>OK. Gibt die erfolgreiche Ausführung an. Das Dataset wird im Antworttext zurückgegeben.</td>
  </tr>
</table>

###Inhaltstyp

application/json

###Textschema

**Imports** haben die folgenden Eigenschaften:

- ID
- Name
- createdDateTime
- Importquelle (entweder Datei- oder URL-Pfad)
- Array von **Berichten**
- Array von **Datasets**
    - Ein **Dataset** hat die gleichen Eigenschaften, die von [GET Datasets](Get-Datasets.md) zurückgegeben werden.
    
    {
      "id" : "{import_guid}",
      "createdDateTime" : "{DateTime}",
      "updatedDateTime" : "{DateTime}",
      "importState" : "{string}",
      "error":
        {
            "code": "{string}",
            "message": "{message}",
            "target": "{target}",
            "details": [
                {
                "code": "{code}",
                "target": "{target}",
                "message": "{message}",
                }
            ]
        },
     "reports" : [
          {
              "id":"{GUID}",
              "name":"{string}"
          }  
      ],        
      "datasets" : [
          {
              "id":"{guid}",
              "name":"{import_name}"
          }  
      ]
    }

###Textbeispiel

    {
      "id" : "405BBEEB--30264301ED95",
      "createdDateTime" : "2015-03-13 15:51:30.937",
      "updatedDateTime" : "2015-03-13 15:51:30.937",
      "importState" : "Published", 
      "error":
        {
            "code": "NotImplemented",
            "message": "Requested feature is not implemented",
            "target": "query",
            "details": [
                {
                "code": "QueryParmNotSupported",
                "target": "$search",
                "message":  "Requested query option not supported",
                }
            ]
        },
     "reports" : [
          {
              "id":"A68F51BD--5EFF229013C7",
              "name":"forecast.xlsx"
          }  
      ],      
    "datasets" : [
          {
              "id":"A68F51BD--5EFF229013C7",
              "name":"forecast.xlsx"
          }  
      ]
    }


