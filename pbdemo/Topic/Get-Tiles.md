---
title: Get Tiles
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59df1d7d-dd08-49aa-bf02-8c6938f70f3a
---
# Get Tiles
---

<a name="top"/>
[Anforderung](#request) | [Antwort](#response)

Der Vorgang **Get Tiles (Kacheln abrufen)** gibt eine JSON-Liste aller **Kachel**-Objekte zurück, einschließlich einer ID, eines Titels und einer embedUrl oder einer JSON-Liste aus einer **Gruppe**.

**Erforderlicher Bereich**: Tile.Read.All

Informationen zum Festlegen des Berechtigungsbereichs finden Sie unter [Registrieren einer Client-App](https://msdn.microsoft.com/en-US/library/dn877542.aspx) oder [Registrieren einer Web-App](https://msdn.microsoft.com/en-us/library/dn985955.aspx).
<a name="request"/>
##Anforderung

GET https://api.powerbi.com/beta/myorg/{dashboard_id}/tiles

###URI-Parameter

| Name| Beschreibung| Datentyp|
|-|-|-|
| dashboard_id| GUID des zu verwendenden <b>Dashboards</b>.Sie können die **Dashboard**-ID mithilfe des Vorgangs [Get Dashboards](Get-Dashboards.md) abrufen.| Zeichenfolge|

###Gruppen

Gruppen sind eine Sammlung einheitlicher **Azure Active Directory**-Gruppen, denen der Benutzer angehört, und sind im Power BI-Dienst verfügbar.
Informationen zum Erstellen einer Gruppe finden Sie unter [Erstellen einer Gruppe](https://support.powerbi.com/knowledgebase/articles/654250).

GET https://api.powerbi.com/beta/myorg/groups/{group_id}/dashboards

###URI-Parameter

| Name| Beschreibung| Datentyp|
|-|-|-|
| group_id| GUID der zu verwendenden <b>Gruppe</b>.Sie können die Gruppen-ID mithilfe des Vorgangs [Get Groups](Get-Groups.md) abrufen.| Zeichenfolge|
###Header

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

**Kacheln** weisen eine GUID-**ID**, einen **Titel** und eine **embedUrl** auf.
Verwenden Sie die embedUrl, um eine Kachel in eine App einzubetten.
Weitere Informationen finden Sie unter [Integrieren einer Power BI-Kachel](Integrate-a-Power-BI-Tile.md).

    {
      "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#tiles","value":
      [
        {
          "id":"{tile_guid}",
          "title":"tile_title",
          "embedUrl":"https://api.powerbi.com/embed?dashboardId={dashboard_id}&tileId={title_id}"
        }
      ]
    }

###Textbeispiel

Das folgende Beispiel zeigt eine JSON-Antwort mit zwei Kacheln.

    {
    "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#tiles", "value":
        [
        {
           "id":"f16dc78a--afe1098a100d",
           "title":"ProductID",
           "embedUrl":"https://dxt.powerbi.com/embed?dashboardId=43127a01--e971d4cdc2fb&tileId=f16dc78a-6897-afe1098a100d"
        },
        {
         "id":"9ab3925d--4d6c896df0c9",
           "title":"Count of CountryRegionCode",
           "embedUrl":"https://dxt.powerbi.com/embed?dashboardId=43127a01--e971d4cdc2fb&tileId=9ab3925d--4d6c896df0c9"
            }
        ]
    }

<a name="example"/>




