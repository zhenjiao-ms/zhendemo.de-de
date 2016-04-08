---
title: Get Tables (Tabellen abrufen)
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b0081acc-4f94-44e5-89c0-ef14e1e407dc
---
# Get Tables (Tabellen abrufen)
---

<a name="top"/>
[Anforderung](#request) | [Antwort](#response) | [Beispiel](#example) | [Auf Apiary testen](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset) | [Client- und Web-App-Beispiele](Power-BI-Samples.md)

Der Vorgang **Get Tables (Tabellen abrufen)** gibt eine JSON-Liste von **Tabellen** für das angegebene **Dataset** zurück.
**Get Tables** gibt auch eine JSON-Liste aus einer **Gruppe** zurück.

**Erforderlicher Bereich**: Dataset.ReadWrite.All oder Dataset.Read.All

Informationen zum Festlegen des Berechtigungsbereichs finden Sie unter [Registrieren einer Client-App](https://msdn.microsoft.com/en-US/library/dn877542.aspx) oder [Registrieren einer Web-App](https://msdn.microsoft.com/en-us/library/dn985955.aspx).
<a name="request"/>
##Anforderung

GET https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/tables

###Gruppen

Gruppen sind eine Sammlung einheitlicher **Azure Active Directory**-Gruppen, denen der Benutzer angehört, und sind im Power BI-Dienst verfügbar.
Informationen zum Erstellen einer Gruppe finden Sie unter [Erstellen einer Gruppe](https://support.powerbi.com/knowledgebase/articles/654250).

GET https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets/{Dataset_-ID}/tables

###URI-Parameter

| Name| Beschreibung| Datentyp|
|-|-|-|
| group_id| GUID der zu verwendenden **Gruppe**.Sie können die Gruppen-ID mithilfe des Vorgangs [Get Groups](Get-Groups.md) abrufen.| Zeichenfolge|
| dataset_id| GUID des zu verwendenden **Datasets**.Sie können die Dataset-ID mit dem Vorgang [Get Datasets](Get-Datasets.md) abrufen.| Zeichenfolge|
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

Eine **Tabelle** hat einen **Namen**.

    {
        "@odata.context":" https://api.powerbi.com/v1.0/myorg/$metadata#tables","value":
        [
            {
                "name":"{table_name}"
            }
        ]
    }

###Textbeispiel

Es folgt ein Beispiel einer JSON-Antwort für eine **Tabelle** "Product".

    {
        "@odata.context":" https://api.powerbi.com/v1.0/myorg/$metadata#tables","value":
        [
            {
                "name":"Product"
            }
        ]
    }

<a name="example"/>
##Beispiel für Get Tables

---

[[Anfang des Artikels]] (#top)

Im folgenden C#-Beispiel wird der Vorgang **Get Tables** aufgerufen.
Bei dem Beispiel wird davon ausgegangen, dass Sie über ein Dataset namens "SalesMarketing" verfügen.
Informationen zum Erstellen eines Datasets finden Sie unter dem Vorgang [Create Dataset](Create-Dataset.md).
Das Beispiel zeigt außerdem Folgendes:

- Abrufen einer Dataset-ID mithilfe einer LINQ-Abfrage.
    Informationen zum Abrufen einer Dataset-ID finden Sie unter [Get Datasets](Get-Datasets.md).
- **Deserialisieren** der JSON-Antwort mit der **JavaScriptSerializer**-Klasse.

**Hinweis** Das [Beispiel für eine Client-App](Power-BI-client-app-sample.md) zeigt außerdem, wie Sie eine JSON-Anforderung und -Antwort **serialisieren** und **deserialisieren** sowie Vorgänge vom Typ **Gruppe** aufrufen.

###Zum Ausführen dieses Codeausschnitts ist Folgendes erforderlich:

[![s1](../Image/Samples-1.png) – Azure Active Directory-Mandant](Create+an+Azure+Active+Directory+tenant.md)

[![s2](../Image/Samples-2.png) – Registrieren bei Power BI](https://powerbi.microsoft.com)

[![s3](../Image/Samples-3.png) – Registrieren der App, um eine Client-ID zu erhalten](Register+a+client+app.md)

![s4](../Image/Samples-4.png) – Sie benötigen eine **Dataset**-ID.
Informationen zum Abrufen einer Dataset-ID finden Sie unter dem Vorgang [Get Datasets](Get-Datasets.md).



        using System;
        using System.Net;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using System.IO;
        using System.Web.Script.Serialization;
        using System.Linq;
        public table[] GetTables()
        {
            //This is sample code to illustrate a Power BI operation. 
            //In a production application, refactor code into specific methods and use appropriate exception handling.
            //The client id that Azure AD creates when you register your client app.
            //  To learn how to register a client app, see https://msdn.microsoft.com/en-US/library/dn877542(Azure.100).aspx  
            string clientID = "{client id from Azure AD app registration}";
            //Assuming you have a dataset named SalesMarketing
            // To get a dataset id, see Get Datasets operation.           
            dataset[] datasets = GetDatasets();
            string datasetId = (from d in datasets where d.Name == "SalesMarketing" select d).FirstOrDefault().Id;
            //RedirectUri you used when you register your app.
            //For a client app, a redirect uri gives Azure AD more details on the application that it will authenticate.
            // You can use this redirect uri for your client app
            string redirectUri = "https://login.live.com/oauth20_desktop.srf";
            //Resource Uri for Power BI API
            string resourceUri = "https://analysis.windows.net/powerbi/api";
            //OAuth2 authority Uri
            string authorityUri = "https://login.windows.net/common/oauth2/authorize";
            string powerBIApiUrl = String.Format("https://api.powerbi.com/v1.0/myorg/datasets/{0}/tables", datasetId);
            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.
            // AcquireToken will acquire an Azure access token
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            string token = authContext.AcquireToken(resourceUri, clientId, new Uri(redirectUri), PromptBehavior.RefreshSession).AccessToken;
            //GET web request to list all tables.
            //To get tables in a group, use the Groups uri: https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets/{dataset_id}/tables
            HttpWebRequest request = System.Net.WebRequest.Create(powerBIApiUrl) as System.Net.HttpWebRequest;
            request.KeepAlive = true;
            request.Method = "GET";
            request.ContentLength = 0;
            request.ContentType = "application/json";
            request.Headers.Add("Authorization", String.Format("Bearer {0}", token));
            //Get HttpWebResponse from GET request
            using (HttpWebResponse httpResponse = request.GetResponse() as System.Net.HttpWebResponse)
            {
                //Get StreamReader that holds the response stream
                using (StreamReader reader = new System.IO.StreamReader(httpResponse.GetResponseStream()))
                {
                    string responseContent = reader.ReadToEnd();
                    JavaScriptSerializer jsonSerializer = new JavaScriptSerializer();
                    Tables tables = (Tables)jsonSerializer.Deserialize(responseContent, typeof(Tables));
                    return tables.value;
                }
            }
        }
        public class Datasets
        {
            public dataset[] value { get; set; }
        }
        public class dataset
        {
            public string Id { get; set; }
            public string Name { get; set; }
        }
        public class Tables
        {
            public table[] value { get; set; }
        }
        public class table
        {
            public string Name { get; set; }
        }
        public class Groups
        {
            public group[] value { get; set; }
        }
        public class group
        {
            public string Id { get; set; }
            public string Name { get; set; }
        }     


