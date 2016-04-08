---
title: Get Datasets (Datasets abrufen)
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26aa56b1-d57b-4556-b5d3-25f034d93a65
---
# Get Datasets (Datasets abrufen)
---

<a name="top"/>
[Anforderung](#request) | [Antwort](#response) | [Beispiel](#example) | [Auf Apiary testen](http://docs.powerbi.apiary.io/#reference/datasets/datasets-collection/list-all-datasets) | [Client- und Web-App-Beispiele](Power-BI-Samples.md)

Der Vorgang **Get Datasets (Datasets abrufen)** gibt eine JSON-Liste aller **Dataset**-Objekte zurück, einschließlich eines Namens und einer ID oder einer JSON-Liste aus einer **Gruppe**.

**Erforderlicher Bereich**: Dataset.ReadWrite.All oder Dataset.Read.All

Informationen zum Festlegen des Berechtigungsbereichs finden Sie unter [Registrieren einer Client-App](https://msdn.microsoft.com/en-US/library/dn877542.aspx) oder [Registrieren einer Web-App](https://msdn.microsoft.com/en-us/library/dn985955.aspx).
<a name="request"/>
##Anforderung

GET https://api.powerbi.com/v1.0/myorg/datasets

###Gruppen

Gruppen sind eine Sammlung einheitlicher **Azure Active Directory**-Gruppen, denen der Benutzer angehört, und sind im Power BI-Dienst verfügbar.
Informationen zum Erstellen einer Gruppe finden Sie unter [Erstellen einer Gruppe](https://support.powerbi.com/knowledgebase/articles/654250).

GET https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets

###URI-Parameter

| Name| Beschreibung| Datentyp|
|-|-|-|
| group_id| GUID der zu verwendenden **Gruppe**.Sie können die Gruppen-ID mithilfe des Vorgangs [Get Groups](Get-Groups.md) abrufen.| Zeichenfolge|
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

**Datasets** haben eine GUID-**ID** und einen **Namen**.

    {
      "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets","value":
      [
        {
          "id":"{dataset_id GUID}",
          "name":"{dataset_name}"
        }
      ]
    }

###Textbeispiel

Es folgt ein Beispiel einer JSON-Antwort für ein **Dataset** "SalesMarketing".

    {
      "@odata.context":"https://api.powerbi.com/v1.0/myorg/$metadata#datasets","value":
      [
        {
          "id":"7c0b090e--172874c749e0",
          "name":"SalesMarketing"
        }
      ]
    }

<a name="example"/>
##Beispiel für Get Datasets

---

[[Anfang des Artikels]] (#top)

Im folgenden C#-Beispiel wird der Vorgang **Get Datasets** aufgerufen.
Das Beispiel zeigt, wie Sie die JSON-Antwort mit der **JavaScriptSerializer**-Klasse **deserialisieren**.

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
        public dataset[] GetDatasets()
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
            string powerBIApiUrl = "https://api.powerbi.com/v1.0/myorg/datasets";
            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.
            // AcquireToken will acquire an Azure access token
            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);
            string token = authContext.AcquireToken(resourceUri, clientId, new Uri(redirectUri), PromptBehavior.RefreshSession).AccessToken;
            //GET web request to list all datasets.
            //To get a datasets in a group, use the Groups uri: https://api.powerbi.com/v1.0/myorg/groups/{group_id}/datasets
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
                    Datasets datasets = (Datasets)jsonSerializer.Deserialize(responseContent, typeof(Datasets));
                    return datasets.value;
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


