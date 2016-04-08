---
title: Create Dataset (Dataset erstellen)
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7567da39-9e24-4da0-b078-146775f736be
---
# Create Dataset (Dataset erstellen)
---

[Anforderung](#request) | [Antwort](#response) |[Beispiel](#example) | [Auf Apiary testen](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset) | [Client- und Web-App-Beispiele](Power-BI-Samples.md)
<a name="top"/>

Der Vorgang **Create Dataset** erstellt ein neues **Dataset** aus einer JSON-Schemadefinition und gibt die **Dataset**-ID sowie die Eigenschaften des erstellten Datasets zurück.
Ein **Dataset** kann in einer **Gruppe** erstellt werden.

**Erforderlicher Bereich**: Dataset.ReadWrite.All


Informationen zum Festlegen des Berechtigungsbereichs finden Sie unter [Registrieren einer Client-App](https://msdn.microsoft.com/en-US/library/dn877542.aspx) oder [Registrieren einer Web-App](https://msdn.microsoft.com/en-us/library/dn985955.aspx).
<a name="request"/>
##Anforderung

POST https://api.powerbi.com/v1.0/myorg/datasets

**Aktivieren einer standardmäßigen Beibehaltungsrichtlinie**

POST https://api.powerbi.com/v1.0/myorg/datasets?defaultRetentionPolicy={None | basicFIFO}

###Abfragezeichenfolgenparameter

| Name| Beschreibung| Werte|
|-|-|-|
| defaultRetentionPolicy| Aktiviert eine standardmäßige Beibehaltungsrichtlinie zum automatischen Bereinigen veralteter Daten, während ein konstanter Fluss neuer Daten in das Dashboard gewahrt bleibt.Weitere Informationen zur automatischen Beibehaltungsrichtlinie finden Sie unter [Automatische Beibehaltungsrichtlinie für Echtzeitdaten](Automatic-retention-policy-for-real-time-data.md).| None oder basicFIFO|

###Gruppen

Gruppen sind eine Sammlung einheitlicher **Azure Active Directory**-Gruppen, denen der Benutzer angehört, und sind im Power BI-Dienst verfügbar.
Informationen zum Erstellen einer Gruppe finden Sie unter [Erstellen einer Gruppe](https://support.powerbi.com/knowledgebase/articles/654250).

POST https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets

###URI-Parameter

| Name| Beschreibung| Datentyp|
|-|-|-|
| group_id| GUID der zu verwendenden **Gruppe**.Sie können die Gruppen-ID mithilfe des Vorgangs [Get Groups](Get-Groups.md) abrufen.| Zeichenfolge|

###Header

Inhaltstyp: application/json
Autorisierung: Träger eyJ0eX ... FWSXfwtQ

###Textschema

Ein **Dataset** verfügt über einen **Namen** und eine Sammlung von **Tabellen**objekten (Tabellen).
Eine **Tabelle** verfügt über einen **Namen** und eine Sammlung von **Spalten**objekten (Spalten).
Eine **Spalte** weist einen **Namen** und einen **Datentyp** auf.

Eine Liste der Power BI-REST-Datentypen finden Sie unter [Unterstützte Datentypen](Supported-data-types.md).

    {"name": "dataset_name", "tables": 
        [{"name": "", "columns": 
            [{ "name": "column_name1", "dataType": "data_type"},
             { "name": "column_name2", "dataType": "data_type"},
             { ... }
            ]
          }
        ]
    }

###Textbeispiel

Ein **Dataset** "SalesMarketing" mit einem **Tabellen**schema (Tables) "Product".
Die **Tabelle** "Product" enthält **Spalten**, die ein Produkt beschreiben.

    {"name": "SalesMarketing","tables": 
        [{"name": "Product", "columns": 
            [{ "name": "ProductID", "dataType": "Int64"},
             { "name": "Name", "dataType": "string"},
             { "name": "Category", "dataType": "string"},
             { "name": "IsCompete", "dataType": "bool"},
             { "name": "ManufacturedOn", "dataType": "DateTime"}
            ]
          }
        ]
    }

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
    <td>201</td>
    <td>Erstellt. Die Anforderung wurde erfüllt und ein neues Dataset erstellt.</td>
  </tr>
</table>

###Inhaltstyp

application/json


###Textschema

Eine Antwort vom Typ **Create Dataset** weist das folgende Schema auf:

    {          
        "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets/$entity",       
        "id":"{dataset_id}",
        "name":"{name}",
        "defaultRetentionPolicy":"{None | basicFIFO}"
    }

###Textbeispiel

Es folgt ein Beispiel einer JSON-Antwort für ein **Dataset** "SalesMarketing", wobei **defaultRetentionPolicy** auf **basicFIFO** festgelegt ist.
Die **Beibehaltungsrichtlinie** bereinigt automatisch veraltete Daten, während ein konstanter Fluss neuer Daten in das Dashboard gewahrt bleibt.

    {
        "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets/$entity",
        "id":"7c0b090e--b51-172874c749e0",
        "name":"SalesMarketing",
        "defaultRetentionPolicy":"basicFIFO"
    }

<a name="example"/>
##Beispiel für Create Dataset

---

[[Anfang des Artikels]] (#top)

Im folgenden C#-Beispiel wird der Vorgang **Create Dataset** aufgerufen.

**Hinweis** Das [Beispiel für eine Client-App](Power-BI-client-app-sample.md) zeigt außerdem, wie Sie eine JSON-Anforderung und -Antwort **serialisieren** und **deserialisieren** sowie Vorgänge vom Typ **Gruppe** aufrufen.

###Zum Ausführen dieses Codeausschnitts ist Folgendes erforderlich:

[![s1](../Image/Samples-1.png) – Azure Active Directory-Mandant](Create+an+Azure+Active+Directory+tenant.md)

[![s2](../Image/Samples-2.png) – Registrieren bei Power BI](https://powerbi.microsoft.com)

[![s3](../Image/Samples-3.png) – Registrieren der App, um eine Client-ID zu erhalten](Register+a+client+app.md)

        using System;
        using System.Net;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.IO;
        using System.Web.Script.Serialization;
        static string CreateDataset()
        {
            //This is sample code to illustrate a Power BI operation. 
            //In a production application, refactor code into specific methods and use appropriate exception handling.
            //The client id that Azure AD creates when you register your client app.
            //  To learn how to register a client app, see https://msdn.microsoft.com/en-US/library/dn877542(Azure.100).aspx
            string clientID = "{client id from Azure AD app registration}";
            //RedirectUri you used when you register your app.
            //For a client app, a redirect uri gives Azure AD more details on the application that it will authenticate.
            // You can use this redirect uri for your client app
            string redirectUri = "https://login.live.com/oauth20_desktop.srf";
            //Resource Uri for Power BI API
            string resourceUri = "https://analysis.windows.net/powerbi/api";
            //OAuth2 authority Uri
            string authorityUri = "https://login.windows.net/common/oauth2/authorize";
            string powerBIApiUrl = "https://api.powerbi.com/v1.0/myorg";
            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.
            // AcquireToken will acquire an Azure access token
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            string token = authContext.AcquireToken(resourceUri, clientID, new Uri(redirectUri)).AccessToken;
            //POST web request to create a dataset.
            //To create a Dataset in a group, use the Groups uri: https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets
            HttpWebRequest request = System.Net.WebRequest.Create(string.Format("{0}/datasets", powerBIApiUrl)) as System.Net.HttpWebRequest;
            request.KeepAlive = true;
            request.Method = "POST";
            request.ContentLength = 0;
            request.ContentType = "application/json";
            request.Headers.Add("Authorization", String.Format("Bearer {0}", token));
            //Create dataset JSON for POST request
            string json = "{\"name\": \"SalesMarketing\", \"tables\": " +
                "[{\"name\": \"Product\", \"columns\": " +
                "[{ \"name\": \"ProductID\", \"dataType\": \"Int64\"}, " +
                "{ \"name\": \"Name\", \"dataType\": \"string\"}, " +
                "{ \"name\": \"Category\", \"dataType\": \"string\"}," +
                "{ \"name\": \"IsCompete\", \"dataType\": \"bool\"}," +
                "{ \"name\": \"ManufacturedOn\", \"dataType\": \"DateTime\"}" +
                "]}]}";
            //POST web request
            byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(json);
            request.ContentLength = byteArray.Length;
            //Write JSON byte[] into a Stream
            using (Stream writer = request.GetRequestStream())
            {
                writer.Write(byteArray, 0, byteArray.Length);
                var response = (HttpWebResponse)request.GetResponse();
                return response.StatusCode.ToString();
            }
        }


