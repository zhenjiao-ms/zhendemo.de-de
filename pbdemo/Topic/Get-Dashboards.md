---
title: Get Dashboards
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39cc3286-8d68-4966-9fe5-36e9fb772cd2
---
# Get Dashboards
---

<a name="top"/>
[Anforderung](#request) | [Antwort](#response)

Der Vorgang **Get Dashboards (Dashboards abrufen)** gibt eine JSON-Liste aller **Dashboards**-Objekte zurück, einschließlich einer ID und eines Anzeigenamens (displayName) oder einer JSON-Liste aus einer **Gruppe**.

**Erforderlicher Bereich**: Dashboard.Read.All

Informationen zum Festlegen des Berechtigungsbereichs finden Sie unter [Registrieren einer Client-App](https://msdn.microsoft.com/en-US/library/dn877542.aspx) oder [Registrieren einer Web-App](https://msdn.microsoft.com/en-us/library/dn985955.aspx).
<a name="request"/>
##Anforderung

GET https://api.powerbi.com/beta/myorg/dashboards

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

**Dashboards** haben eine GUID-**ID** und einen **Namen**.

    {
      "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#datasets","value":
      [
        {
          "id":"{dashboard_id GUID}",
          "displayName":"{dashboard_displayName}"
        }
      ]
    }

###Textbeispiel

Das folgende Beispiel zeigt die JSON-Antwort für das **Dashboard** „MyDashboard“.

    {
        "@odata.context":"https://api.powerbi.com/beta/myorg/$metadata#dashboards", "value":
        [
           {
              "id":"43127a01--e971d4cdc2fb",
                "displayName":"My Dashboard"
           }
        ]
    }

<a name="example"/>




