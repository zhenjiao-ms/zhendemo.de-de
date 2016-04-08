---
title: Delete Rows (Zeilen l&#246;schen)
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0775aad-c351-4339-bdd4-b610011d6a55
---
# Delete Rows (Zeilen l&#246;schen)
---

<a name="top"/>
[Anforderung](#request) | [Antwort](#response) | [Beispiel](#example) | [Auf Apiary testen](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/create-a-dataset) | [Client- und Web-App-Beispiele](Power-BI-Samples.md)

Der Vorgang **Delete Rows** löscht **Zeilen** aus einer **Tabelle** in einem **Dataset** oder einem **Dataset** in einer **Gruppe**.

**Erforderlicher Bereich**: Dataset.ReadWrite.All

Informationen zum Festlegen des Berechtigungsbereichs finden Sie unter [Registrieren einer Client-App](https://msdn.microsoft.com/en-US/library/dn877542.aspx) oder [Registrieren einer Web-App](https://msdn.microsoft.com/en-us/library/dn985955.aspx).
<a name="request"/>
##Anforderung

DELETE https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/tables/{table_name}/rows

###Gruppen

Gruppen sind eine Sammlung einheitlicher **Azure Active Directory**-Gruppen, denen der Benutzer angehört, und sind im Power BI-Dienst verfügbar.
Informationen zum Erstellen einer Gruppe finden Sie unter [Erstellen einer Gruppe](https://support.powerbi.com/knowledgebase/articles/654250).

DELETE https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets/{dataset_id}/tables/{table_name}/rows

###URI-Parameter

| Name| Beschreibung| Datentyp|
|-|-|-|
| group_id| GUID der zu verwendenden **Gruppe**.Sie können die Gruppen-ID mithilfe des Vorgangs [Get Groups](Get-Groups.md) abrufen.| Zeichenfolge|
| dataset_id| GUID des zu verwendenden **Datasets**.Sie können die Dataset-ID mit dem Vorgang [Get Datasets](Get-Datasets.md) abrufen.| Zeichenfolge|
| table_name| Name der **Tabelle** im **Dataset**.| Zeichenfolge|
###DELETE-Beispiel

https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/tables/Product/Rows
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


###Text

Keiner

<a name="example"/>
##Beispiel für Delete Rows

---

[[Anfang des Artikels]] (#top)

Im folgenden C#-Beispiel wird der Vorgang **Delete Rows** aufgerufen.
Bei dem Beispiel wird davon ausgegangen, dass Sie über ein Dataset mit einer Tabelle **Product** verfügen.
Informationen zum Erstellen eines Datasets finden Sie unter dem Vorgang [Create Dataset (Dataset erstellen)](Create-Dataset.md).
Das Beispiel zeigt außerdem Folgendes:

- Abrufen einer Dataset-ID mithilfe einer LINQ-Abfrage.
    Informationen zum Abrufen einer Dataset-ID finden Sie unter [Get Datasets (Datasets abrufen)](Get-Datasets.md).

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
        public string DeleteRows()
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
            // Assumes the Dataset named SalesMarketing has a Table named Product
            string tableName = "Product";
            //RedirectUri you used when you register your app.
            //For a client app, a redirect uri gives Azure AD more details on the application that it will authenticate.
            // You can use this redirect uri for your client app
            string redirectUri = "https://login.live.com/oauth20_desktop.srf";
            //Resource Uri for Power BI API
            string resourceUri = "https://analysis.windows.net/powerbi/api";
            //OAuth2 authority Uri
            string authorityUri = "https://login.windows.net/common/oauth2/authorize";
            string powerBIApiUrl = String.Format("https://api.powerbi.com/v1.0/myorg/datasets/{0}/tables/{1}/Rows", datasetId, tableName);
            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.
            // AcquireToken will acquire an Azure access token
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            string token = authContext.AcquireToken(resourceUri, clientId, new Uri(redirectUri), PromptBehavior.RefreshSession).AccessToken;
            //DELETE web request to delete rows.
            //To delete rows in a group, use the Groups uri: https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets/{dataset_id}/tables/{table_name}/rows
            HttpWebRequest request = System.Net.WebRequest.Create(powerBIApiUrl) as System.Net.HttpWebRequest;
            request.KeepAlive = true;
            request.Method = "DELETE";
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
                    return responseContent;
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


