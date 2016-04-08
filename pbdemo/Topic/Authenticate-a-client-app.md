---
title: Authentifizieren einer Client-App
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 53e5ee1c-2a6e-414c-bb2b-798e7f148a9f
robots: noindex,nofollow
---
# Authentifizieren einer Client-App
---

[Laden Sie das Beispiel einer .NET Client-App herunter](http://go.microsoft.com/fwlink/?LinkId=619280)|[Zeigen Sie den Code auf GitHub an](http://go.microsoft.com/fwlink/?LinkId=619429)

In diesem Artikel erfahren Sie, wie Power BI-Client-Apps authentifiziert werden.
Er enthält C#-Beispiele, wobei der Authentifizierungsprozess für andere Programmiersprachen identisch ist.

Ein vollständiges C#-Beispiel, das zeigt, wie eine Power BI-Client-App authentifiziert wird, finden Sie unter [Beispiel für Client-App](Power-BI-client-app-sample.md).

##Inhalt dieses Artikels

*   [Voraussetzungen für das Authentifizieren von Power BI-Client-Apps](#What)
*   [Anfordern von Daten bei der Power BI-REST-API mit einem Token](#Datarequest)
*   [Kontextfluss für Azure-Authentifizierung](#Flow)
*   [Hinzufügen einer Azure Active Directory-Authentifizierungsbibliothek](#Library)

Power BI-Client-Apps verwenden **Azure Active Directory** (AAD) zum Authentifizieren von Benutzern und zum Schutz von Anwendungen.
Bei der Authentifizierung wird eine Anwendung oder ein Benutzer identifiziert.
Damit Ihre Client-App in AAD identifiziert werden kann, registrieren Sie sie in AAD.
Wenn Sie eine Client-App in AAD registrieren, übergeben Sie Ihren App-Zugriff an die Power BI-APIs.
Informationen zum Registrieren Ihrer Power BI-Client-App finden Sie unter [Registrieren einer Client-App](Register-a-client-app.md).

Aufrufe der Power BI-REST-API erfolgen im Namen eines authentifizierten Benutzers. Dabei wird im Autorisierungsheader der Anforderung ein Token übermittelt.
Das Token wird über Azure Active Directory abgerufen.

**Hinweis** Für die private Power BI-Vorschau werden Apps im Azure-Verwaltungsportal als mehrinstanzfähige Apps erstellt.

<a name="What"></a>

##Voraussetzungen für das Authentifizieren von Power BI-Client-Apps

Führen Sie zum Authentifizieren einer Power BI-Client-App und zum Ausführen einer REST-Webanforderung folgende Schritte aus:

1.  **Registrieren Sie Ihre Client-App**: Informationen zum Registrieren einer Power BI-Client-App finden Sie unter [Registrieren einer Client-App](Register-a-client-app.md).
    Wenn Sie eine Client-App in **Azure Active Directory** registrieren, übergeben Sie Ihren App-Zugriff an die Power BI-APIs.
2.  **Weisen Sie die Client-ID für Ihre App zu**: Informationen zum Abrufen der Client-ID für Ihre App finden Sie unter [Anfordern einer Client-App-ID](Register-a-client-app.md#clientID).
    Die Anwendung identifiziert sich mithilfe der Client-ID bei den Benutzern, von denen sie Berechtigungen anfordert.
    
    *   Weisen Sie in Ihrem Client-App-Code der Client-ID Ihrer Azure-Anwendung die Variable **clientID** zu.
3.  **Weisen Sie den Umleitungs-URI zu**: In Bezug auf eine Client-App bietet ein Umleitungs-URI Azure AD ausführlichere Informationen über die zu authentifizierende Anwendung.
    Ein URI (Uniform Resource Identifier) ist ein Wert, der den Namen einer Ressource identifiziert.
    
    *   Weisen Sie in Ihrem Client-App-Code "https://login.live.com/oauth20_desktop.srf" den **redirectUri** zu.
        Da eine Clientanwendung keinen externen Dienst zum Umleiten hat, ist dieser URI der standardmäßige Platzhalter für Clientanwendungen.
4.  **Weisen Sie den Ressourcen-URI für die Power BI-API zu**: Der Ressourcen-URI identifiziert die Power BI-API-Ressource.
    
    *   Weisen Sie in Ihrem Client-App-Code "https://analysis.windows.net/powerbi/api" den  **resourceUri** zu.
5.  **Weisen Sie den OAuth2- Zertifizierungsstellen-URI zu**: Der Zertifizierungsstellen-URI identifiziert die OAuth2-Zertifizierungsstellenressource.
    
    *   Weisen Sie in Ihrem Client-App-Code "https://login.windows.net/common/oauth2/authorize" einen Zertifizierungsstellen-URI zu.
6.  **Weisen Sie den Dataset-URI für die Power BI-API-Datasets zu**: Der Dataset-URI identifiziert die Power BI-API-Datasets-Ressource.
    
    *   Weisen Sie in Ihrem Client-App-Code "https://api.powerbi.com/v1.0/myorg/datasets" den **datasetsUri** zu.

Um eine Datenanforderung an den Power BI-REST-Dienst zu stellen, müssen Sie ein Zugriffstoken angeben.
In einer .NET-Client-App verwenden Sie die [Windows Azure-Authentifizierungsbibliothek (ADAL)](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx), um ein Zugriffstoken abzurufen.
Dies ist der Prozess.
Im Folgenden finden Sie ein Beispiel für die Methode **AccessToken()**.

Wenn Sie keine Windows Azure-Authentifizierungsbibliothek (ADAL) haben, finden Sie weitere Informationen unter [Hinzufügen einer Azure Active Directory-Authentifizierungsbibliothek](#Library).

**Wichtig** Zum Authentifizieren einer Client-App fügen Sie in die Windows Azure-Authentifizierungsbibliothek (ADAL) einen Verweis auf **Microsoft.IdentityModel.Clients.ActiveDirectory** ein.

##Schritte zum Abrufen eines Zugriffstoken

1.  **Erstellen Sie eine Instanz von AuthenticationContext**: AuthenticationContext ist die Hauptklasse. Sie repräsentiert die Zertifizierungsstelle für die Tokenausstellung für Azure AD-Ressourcen.
    Der Konstruktor akzeptiert:
    
    *   Einen OAuth2 authorityUri 


    ```
            //OAuth2 authority Uri
            string authorityUri = "https://login.windows.net/common/oauth2/authorize";
    AuthenticationContext  authContext = new AuthenticationContext(authorityUri);

    ```

1.  **Rufen Sie mit AuthenticationContext.AcquireToken() ein Token ab**: Die Methode akzeptiert:
    
    *   Einen resourceUri für Power BI-API
    *   Ihre clientID für die Power BI-App
    *   Ihren redirectUri für die Power BI-App 


    ```
    string token = authContext.AcquireToken(resourceUri, clientId, new Uri(redirectUri), PromptBehavior.RefreshSession).AccessToken;

    ```

Weitere Informationen darüber, wie mit AuthenticationContext ein Token abgerufen wird, finden Sie unter [Kontextfluss für Azure-Authentifizierung](#Flow).

###C#-Beispiel: Zugriffstoken abrufen

In einer .NET-Client-App rufen Sie mit **AuthenticationContext** ein Zugriffstoken ab.


```
      static string AccessToken()
      {
            //Get access token: 
            // To call a Power BI REST operation, create an instance of AuthenticationContext and call AcquireToken
            // AuthenticationContext is part of the Active Directory Authentication Library NuGet package
            // To install the Active Directory Authentication Library NuGet package in Visual Studio, 
            //  run "Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory" from the nuget Package Manager Console.

            //Resource Uri for Power BI API
            string resourceUri = "https://analysis.windows.net/powerbi/api";

            string clientId = {clientIDFromAzureAppRegistration};

            //A redirect uri gives AAD more details about the specific application that it will authenticate.
            //Since a client app does not have an external service to redirect to, this Uri is the standard placeholder for a client app.
            string redirectUri = "https://login.live.com/oauth20_desktop.srf";

            // Create an instance of AuthenticationContext to acquire an Azure access token
            // OAuth2 authority Uri
            string authorityUri = "https://login.windows.net/common/oauth2/authorize";
            AuthenticationContext authContext = new AuthenticationContext(authorityUri);

            // Call AcquireToken to get an Azure token from Azure Active Directory token issuance endpoint
            //  AcquireToken takes a Client Id that Azure AD creates when you register your client app.
            //  To learn how to register a client app and get a Client ID, see https://msdn.microsoft.com/en-US/library/dn877542(Azure.100).aspx   
            string token = authContext.AcquireToken(resourceUri, clientID, new Uri(redirectUri)).AccessToken;

            return token;
      }

```

<a name="Datarequest"></a>

##Anfordern von Daten bei der Power BI-REST-API mit einem Token

Nachdem Sie ein Zugriffstoken von Active Directory (AAD) erhalten haben, stellen Sie damit eine Webanforderung an die Power BI-REST-API.
Um eine Power BI-REST-Webanforderung zu erstellen, fügen Sie dem Anforderungsheader wie folgt ein Zugriffstoken hinzu:


```
request.Headers.Add("Authorization", String.Format("Bearer {0}", AccessToken()));

```


###C#-Beispiel - Power BI-REST-API-Datenanforderung mithilfe eines Token

Ein vollständiges C#-Beispiel, das zeigt, wie eine Power BI-Client-App authentifiziert wird und alle Power BI-REST-Vorgänge aufgerufen werden, finden Sie unter [Beispiel für Client-App](Power-BI-client-app-sample.md), oder [zeigen Sie den Code auf GitHub an](http://go.microsoft.com/fwlink/?LinkId=619429).


```
        static dataset[] GetDatasets()
        {
            //This is sample code to illustrate a Power BI operation. 
            //In a production application, refactor code into specific methods and use appropriate exception handling.

            //Power BI Datasets Url
            string powerBIApiUrl = "https://api.powerbi.com/v1.0/myorg/datasets";

            //Get Azure AD access token (see above)
            string token = AccessToken();

            //GET web request to list all datasets.
            //To get a datasets in a group, use the Groups uri: https://api.PowerBI.com/v1.0/myorg/groups/{group_id}/datasets
            HttpWebRequest request = System.Net.WebRequest.Create(powerBIApiUrl) as System.Net.HttpWebRequest;
            request.KeepAlive = true;
            request.Method = "GET";
            request.ContentLength = 0;
            request.ContentType = "application/json";

            //Add access token to Request header
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


```

<a name="Flow"></a>

##Kontextfluss für Azure-Authentifizierung

In einer .NET-Client-App rufen Sie mit **AuthenticationContext** ein Azure-Zugriffstoken ab.
**AuthenticationContext** ist die Hauptklasse. Sie repräsentiert die Zertifizierungsstelle für die Tokenausstellung für Azure AD-Ressourcen.
**AuthenticationContext** bewirkt Folgendes:

1.  AuthenticationContext startet den Fluss durch Umleiten des Benutzer-Agents an den Autorisierungsendpunkt von Azure Active Directory.
    Der Benutzer authentifiziert sich und gibt seine Zustimmung, sofern diese erforderlich ist.
2.  Der Autorisierungsendpunkt von Azure Active Directory leitet den Benutzer-Agent mit einem Autorisierungscode an AuthenticationContext zurück.
    Der Benutzer-Agent gibt einen Autorisierungscode an den Umleitungs-URI der Client-App zurück.
3.  AuthenticationContext fordert vom Azure Active Directory-Endpunkt für die Tokenausstellung ein Zugriffstoken an.
    Der Autorisierungscode wird als Nachweis präsentiert, dass der Benutzer zugestimmt hat.
4.  Der Azure Active Directory-Endpunkt für die Tokenausstellung gibt ein Zugriffstoken zurück.
5.  Die Client-App verwendet das Zugriffstoken für die Authentifizierung bei der Web-API.
6.  Nach der Authentifizierung der Client-App gibt die Power BI REST-API die angeforderten Daten zurück.

Weitere Informationen zum Azure Active Directory (Azure AD)-Autorisierungsfluss finden Sie unter [Gewähren eines Autorisierungscodes](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx).

<a name="Library"></a>

##Hinzufügen einer Azure Active Directory-Authentifizierungsbibliothek

In einer .NET-Client-App verwenden Sie **AuthenticationContext** in der **Active Directory-Authentifizierungsbibliothek**, um ein Azure-Zugriffstoken abzurufen.
Sie können das NuGet-Paket für die **Active Directory-Authentifizierungsbibliothek** von Visual Studio installieren.
Wenn Sie ein NuGet-Paket installieren, erstellt Visual Studio einen Verweis auf die erforderlichen Assemblys.

1.  Klicken Sie mit der rechten Maustaste auf eine Lösung.
2.  Wählen Sie **NuGet-Pakete verwalten**.
3.  Suchen Sie die **Active Directory-Authentifizierungsbibliothek**.
4.  Wählen Sie in der Liste der Pakete die **Active Directory-Authentifizierungsbibliothek** aus, und klicken Sie auf **Installieren**.

##Verwandte Themen

*   [Azure AD-Authentifizierungsbibliothek für .NET](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx)
*   [Active Directory-Authentifizierungsbibliothek (ADAL) v1 für .NET](http://www.cloudidentity.com/blog/2013/09/12/active-directory-authentication-library-adal-v1-for-net-general-availability/)
*   [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/en-us/library/azure/dn645545.aspx)
*   [Gewähren eines Autorisierungscodes](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx)
*   [Authentifizierungsszenarien für Azure AD](https://msdn.microsoft.com/en-US/library/azure/dn499820.aspx)


