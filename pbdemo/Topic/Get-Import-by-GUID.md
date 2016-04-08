---
title: Get Import by GUID
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d71ff2b0-1b9b-4f57-b39f-52da11dba554
---
# Get Import by GUID
---

[Anforderung](#request) | [Antwort](#response) | **[Vorschauvorgang]**
<a name="top"/>

Der Vorgang **Get Import by GUID (Import nach GUID abrufen)** gibt einen JSON-Textkörper für ein **Import**-Objekt mit seinen Eigenschaften zurück, einschließlich ID, Importquelle, **Berichten** und **Datasets**.
Schließen Sie die **Import-GUID** in den GET-URI ein, um ein **Import**-Objekt zurückzugeben.

**Erforderlicher Bereich**: Metadata.View_Any
<a name="request"/>
##Anforderung

GET https://api.powerbi.com/beta/myorg/imports/{Import_GUID}

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
      "id" : "{Import_GUID}",
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
              "id":"{GUID}",
              "name":"{string}"
          }  
        ]
      }

###Textbeispiel

    {
      "id" : "405BBEEB--30264301ED95",
      "createdDateTime" : "2015-03-13 15:51:30.937",
      "updatedDateTime" : "2015-03-13 15:51:30.937",
      "importState" : "Published", -Need to get Enum values
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
          "id":"1662698F-045D-4F2D-AFD1-556BEC8F4413",
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


