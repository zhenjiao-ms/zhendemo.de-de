---
title: Get Imports
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee917f6-37a0-4061-8a82-4fd90ac85bb4
---
# Get Imports
---

[Anforderung](#request) | [Antwort](#response)
<a name="top"/> | **[Vorschauvorgang]**

Der Vorgang **Get Imports (Imports abrufen)** gibt eine JSON-Liste aller **Import**objekte mit diesen Eigenschaften zurück, einschließlich ID, Importquelle, **Berichten** und **Datasets**.


**Erforderlicher Bereich**: Metadata.View_Any
<a name="request"/>
##Anforderung

GET https://api.powerbi.com/beta/myorg/imports

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
    "value":[
        {
          "id" : "{guid}",
          "createdDateTime" : "{DateTime}",
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
      ]    
    }
    

###Textbeispiel

    {
    "value":[
        {
          "id" : "405BBEEB--30264301ED95",
          "createdDateTime" : "2015-03-13 15:51:30.937",
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
      ]    
    }  


