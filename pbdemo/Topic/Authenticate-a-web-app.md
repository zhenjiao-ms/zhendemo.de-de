---
title: Authentifizieren einer Web-App
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc2d0c29-b2e6-41e4-933a-8a82ed55e9e8
robots: noindex,nofollow
---
# Authentifizieren einer Web-App
---

[Herunterladen des Web-App-Beispiels](http://go.microsoft.com/fwlink/?LinkId=619279) |  Anzeigen des Codes auf GitHub: [Default.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619431) | [Redirect.aspx.cs](http://go.microsoft.com/fwlink/?LinkId=619432)

In diesem Artikel erfahren Sie, wie Power BI-Web-Apps authentifiziert werden.
Er enthält C#-Beispiele, wobei der Authentifizierungsprozess für andere Web-Programmiersprachen identisch ist.
Es gibt ein [Beispiel für Web-App auf GitHub](http://go.microsoft.com/fwlink/?LinkId=619279).
Weitere Informationen zum Ausführen des Beispiels finden Sie unter [Beispiel für Web-App](Power-BI-web-app-sample.md).


Power BI-Web-Apps verwenden Active Directory (AAD) zum Authentifizieren von Benutzern und zum Schutz von Anwendungen.
Bei der Authentifizierung wird eine Anwendung oder ein Benutzer identifiziert.
Damit Ihre Web-App in AAD identifiziert werden kann, registrieren Sie sie in AAD.
Wenn Sie eine Web-App in AAD registrieren, übergeben Sie Ihren App-Zugriff an die Power BI-REST-API-Ressourcen.
Informationen zum Registrieren Ihrer Power BI-Web-App finden Sie unter [Registrieren einer Web-App](Register-a-web-app.md).

##Inhalt dieses Artikels

- [Registrieren Ihrer Web-App](#register)
- [Konfigurieren von Power BI-Einstellungen für die Authentifizierung in Azure AD](#configure)
- [Erstellen einer Abfragezeichenfolge zum Abrufen eines Autorisierungscodes von Azure AD](#create)
- [Abrufen eines Azure AD-Zugriffstokens mithilfe des Autorisierungscodes](#acquire)
- [Aufrufen eines Power BI-Vorgangs mithilfe des Azure AD-Zugriffstokens](#use)
- [Hinzufügen einer Azure Active Directory-Authentifizierungsbibliothek](#add)

Weitere Informationen zum Azure Active Directory (Azure AD)-Autorisierungsfluss finden Sie unter [Gewähren eines Autorisierungscodes](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx).

**HINWEIS** Für Power BI werden Apps im Azure-Verwaltungsportal als mehrinstanzfähige Apps erstellt.

##Voraussetzungen für das Authentifizieren von Power BI-Web-Apps

Führen Sie zum Authentifizieren einer Power BI-Web-App und zum Ausführen einer REST-Webanforderung folgende Schritte aus:
Diese Schritte gelten für eine ASP.NET-Web-App, aber auch für andere Plattformen.
Weitere Informationen zu OAuth 2.0 in Azure AD finden Sie unter [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/en-us/library/azure/dn645545.aspx).
<a name="register"/>
###Schritt 1: Registrieren Ihrer Web-App

Wenn Sie eine Web-App in Azure Active Directory registrieren, übergeben Sie Ihren App-Zugriff an die Power BI-REST-API-Ressourcen.
Informationen zum Registrieren einer Power BI-Web-App finden Sie unter [Registrieren einer Web-App](Register-a-web-app.md).
<a name="configure"/>
###Schritt 2: Konfigurieren von Power BI-Einstellungen für die Authentifizierung in Azure AD

Zum Authentifizieren einer Power BI-Web-App in Azure AD sind die folgenden Einstellungen erforderlich:

<table>
  <tr>
    <td>
      <b>Einstellung</b>
    </td>
    <td>
      <b>Beschreibung</b>
    </td>
    <td>
      <b>Wert</b>
    </td>
  </tr>
  <tr>
    <td>Client-ID</td>
    <td>Die Anwendung identifiziert sich mithilfe der Client-ID bei den Benutzern, von denen sie Berechtigungen anfordert.</td>
    <td>Informationen zum Abrufen einer Client-ID für eine Power BI-App finden Sie unter [Anfordern einer Client-App-ID](Register-a-web-app.md#clientID).</td>
  </tr>
  <tr>
    <td>Geheimer Clientschlüssel</td>
    <td>Der geheime Clientschlüssel wird bei der Authentifizierung in Azure AD zusammen mit einer Client-ID gesendet, um eine Web-API aufzurufen.</td>
    <td>Informationen zum Abrufen eines geheimen Clientschlüssels für eine Power BI-App finden Sie unter [Anfordern eines geheimen Clientschlüssels](Register-a-web-app.md#clientSecret).</td>
  </tr>
  <tr>
    <td>Ressourcen-URI</td>
    <td>Der Ressourcen-URI für die zu autorisierende Power BI-Ressource. Wichtig ist, dass Sie den exakten URI verwenden.</td>
    <td>https://analysis.windows.net/powerbi/api</td>
  </tr>
  <tr>
    <td>Zertifizierungsstellen-URI</td>
    <td>Der Zertifizierungsstellen-URI ist eine Azure-Ressource, die mithilfe einer Client-ID ein Zugriffstoken abruft.</td>
    <td>https://login.windows.net/common/oauth2/authorize</td>
  </tr>
  <tr>
    <td>Umleitungs-URL</td>
    <td>Eine Umleitungs-URL für die Web-App-URL. Der Azure AD-Dienst leitet mit einem Authentifizierungscode zur Web-App-URL um.</td>
    <td>Beispiel: http://localhost:13526/Redirect</td>
  </tr>
</table>

<a name="create"/>
###Schritt 3: Erstellen einer Abfragezeichenfolge zum Abrufen eines Autorisierungscodes von Azure AD

Zum Authentifizieren einer Power BI-Web-App erstellen Sie zunächst eine URL-Abfragezeichenfolge für die Umleitung zum Azure AD-Authentifizierungsdienst.
Nachdem Sie gültige Anmeldeinformationen eingegeben haben, gibt Azure AD einen Autorisierungscode zurück.
Das folgende Beispiel zeigt eine vollqualifizierte Azure AD-URL mit einer Abfragezeichenfolge.
Mit einer URL wie dieser rufen Sie einen Autorisierungscode von Azure AD ab.
Verwenden Sie **Response.Redirect**() für die Umleitung zum Azure AD-Dienst, der einen Autorisierungscode von Azure AD zurückgibt.
In Schritt 4 rufen Sie mit dem Autorisierungscode ein Zugriffstoken nach Autorisierungscode ab.

    https://login.windows.net/common/oauth2/authorize
      ?response_type=code
      &client_id=1861585d...9a79c296
      &resource= https://analysis.windows.net/powerbi/api
      &redirect_uri= http://localhost:13526/Redirect

**Einstellungen der Abfragezeichenfolge für die Power BI-Authentifizierung**

<table>
  <tr>
    <td>
      <b>Einstellung</b>
    </td>
    <td>
      <b>Beschreibung</b>
    </td>
  </tr>
  <tr>
    <td>response_type=code</td>
    <td>Azure AD gibt einen Autorisierungscode zurück.</td>
  </tr>
  <tr>
    <td>client_id=1861585d...9a79c296</td>
    <td>Die Anwendung identifiziert sich mithilfe der Client-ID bei den Benutzern, von denen sie Berechtigungen anfordert. Sie erhalten die Client-ID bei der Registrierung Ihrer Azure-Anwendung.</td>
  </tr>
  <tr>
    <td>resource= https://analysis.windows.net/powerbi/api</td>
    <td>Der Ressourcen-URI für die zu autorisierende Power BI-Ressource. Wichtig ist, dass Sie den exakten URI verwenden.</td>
  </tr>
  <tr>
    <td>redirect_uri= http://localhost:13526/Redirect</td>
    <td>Nachdem sich der Benutzer authentifiziert hat, wird er von Azure AD zur Web-App zurückgeleitet.</td>
  </tr>
</table>

Dieses C#-Codebeispiel zeigt, wie Sie eine Autorisierungs-URL für Azure AD mit einer Abfragezeichenfolge erstellen und zum Zertifizierungsstellendienst von Azure AD umleiten.


**Beispiel: C#-Anmeldung**

        protected void signInButton_Click(object sender, EventArgs e)
        {
            //Create a query string
            //Create a sign-in NameValueCollection for query string
            var @params = new NameValueCollection
            {
                //Azure AD will return an authorization code. 
                //See the Redirect class to see how "code" is used to AcquireTokenByAuthorizationCode
                {"response_type", "code"},
                //Client ID is used by the application to identify themselves to the users that they are requesting permissions from. 
                //You get the client id when you register your Azure app.
                {"client_id", Properties.Settings.Default.ClientID},
                //Resource uri to the Power BI resource to be authorized
                {"resource", "https://analysis.windows.net/powerbi/api"},
                //After user authenticates, Azure AD will redirect back to the web app
                {"redirect_uri", "http://localhost:13526/Redirect"}
            };
            //Create sign-in query string
            var queryString = HttpUtility.ParseQueryString(string.Empty);
            queryString.Add(@params);
            //Redirect authority
            //Authority Uri is an Azure resource that takes a client id to get an Access token
            string authorityUri = "https://login.windows.net/common/oauth2/authorize/";
            Response.Redirect(String.Format("{0}?{1}", authorityUri, queryString));       
        }

<a name="acquire"/>
###Schritt 4: Abrufen eines Azure AD-Zugriffstokens mithilfe des Autorisierungscodes

Um eine Datenanforderung an den Power BI-REST-Dienst zu stellen, müssen Sie ein Zugriffstoken angeben.
In einer .NET-Web-App verwenden Sie die [Windows Azure-Authentifizierungsbibliothek (ADAL)](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx), um ein Zugriffstoken abzurufen.
Wenn Sie keine [Windows Azure-Authentifizierungsbibliothek (ADAL)](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx) haben, finden Sie weitere Informationen unter [Hinzufügen einer Azure Active Directory-Authentifizierungsbibliothek](#add).

Nachdem die App an den Zertifizierungsstellen-URI für Azure AD umgeleitet wurde und einen Autorisierungscode erhalten hat, ruft sie ein Token nach Autorisierungscode ab.
So ruft Ihre App den Autorisierungscode und ein Zugriffstoken ab:
**In einer Umleitungsklasse**:

1.  Rufen Sie den von Azure AD zurückgegebenen Azure AD-Autorisierungscode ab.

    ```
    string code = Request.Params.GetValues(0)[0];
    ```
2.  Erstellen Sie einen **AuthenticationContext**, indem Sie den Zertifizierungsstellen-URI und einen TokenCache übergeben.

    ```
    TokenCache TC = new TokenCache();

    AuthenticationContext AC = new AuthenticationContext(authorityUri, TC);
    ```

3.  Erstellen Sie **ClientCredential**-Informationen, indem Sie die **Client-ID** und den **geheimen Clientschlüssel** der Azure-App übergeben.


    ```
    ClientCredential cc = new ClientCredential(Properties.Settings.Default.ClientID, Properties.Settings.Default.ClientSecretKey);
    ```

4.  Rufen Sie ein Token nach Autorisierungscode ab, indem Sie den von Azure AD zurückgegebenen **Authentifizierungscode** und eine **Umleitungs-URL** übergeben.

    ```
    AuthenticationResult AR = AC.AcquireTokenByAuthorizationCode(code, new Uri(redirectUri), cc);
    ```

5.  Umleiten zurück zu default.aspx.

    ```
    Response.Redirect("/Default.aspx");
    ```

6.  Setzen Sie die Indexzeichenfolge "authResult" einer Sitzung auf "AuthenticationResult", sodass Sie das Ergebnis auf der Seite "default.aspx" verwenden können.

    ```
    Session["authResult"] = AR;
    ```

**Führen Sie auf der Seite "Default.aspx" folgenden Schritt aus**:

1.  Rufen Sie ein **AuthenticationResult** der Sitzung ab.
    Verwenden Sie in Schritt 4 das **AuthenticationResult**, um ein **AccessToken** für die Autorisierung abzurufen.

    ```
    AuthenticationResult authResult = (AuthenticationResult)Session["authResult"];
    ```

Hier sehen Sie den vollständigen Code zum Abrufen eines Azure-Zugriffstokens nach Autorisierungscode und zum Umleiten an default.aspx.

**Hinweis** Der Umleitungs-URI muss beim Anfordern von Autorisierungscode mit dem Parameter "redirect_uri" übereinstimmen.

    public partial class Redirect : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            //Redirect uri must match the redirect_uri used when requesting Authorization code.
            string redirectUri = "http://localhost:13526/Redirect";
            string authorityUri = "https://login.windows.net/common/oauth2/authorize/";
            // Get the auth code
            string code = Request.Params.GetValues(0)[0];
            // Get auth token from auth code       
            TokenCache TC = new TokenCache();
            AuthenticationContext AC = new AuthenticationContext(authorityUri, TC); 
            ClientCredential cc = new ClientCredential
                (Properties.Settings.Default.ClientID,
                Properties.Settings.Default.ClientSecretKey);
            AuthenticationResult AR = AC.AcquireTokenByAuthorizationCode(code, new Uri(redirectUri), cc);
            //Set Session "authResult" index string to the AuthenticationResult
            Session["authResult"] = AR;
            //Redirect back to Default.aspx
            Response.Redirect("/Default.aspx");
        }
    protected void Page_Load(object sender, EventArgs e)
    {
        //Test for AuthenticationResult
        if (Session["authResult"] != null)
        {
            //Get the authentication result from the session
            authResult = (AuthenticationResult)Session["authResult"];
            //Show Power BI Panel
            PBIPanel.Visible = true;
            signinPanel.Visible = false;
            //Set user and toek from authentication result
            userLabel.Text = authResult.UserInfo.DisplayableId;
            accessTokenTextbox.Text = authResult.AccessToken;
        }
        else
        {
            PBIPanel.Visible = false;
        }
    }

<a name="use"/>
###Schritt 5: Aufrufen eines Power BI-Vorgangs mithilfe des Azure AD-Zugriffstokens

Aufrufe der Power BI-REST-API erfolgen im Namen eines authentifizierten Benutzers. Dabei wird im Autorisierungsheader ein Zugriffstoken übermittelt, das über Azure Active Directory abgerufen wurde.
Nachdem Sie ein Zugriffstoken von Active Directory (AAD) erhalten haben, stellen Sie damit eine Webanforderung an die Power BI-REST-API.


Sobald Sie durch Anfordern eines Tokens nach Autorisierungscode (**AcquireTokenByAuthorizationCode**) ein **AuthenticationResult** festgelegt haben, erhalten Sie durch Abrufen der Eigenschaft **AccessToken** von **AuthenticationResult** ein Azure-Zugriffstoken.
Mit diesem Code rufen Sie **Datasets** für Power BI ab.
Der Beispielcode erstellt eine Instanz von **WebRequest** und deserialisiert die Antwortzeichenfolge als Dataset.
Für den Zugriff auf die Power BI-REST-API erstellen Sie einen Anforderungsheader und setzen "Autorisierung" auf "Träger {AccessToken}":

    request.Headers.Add("Authorization", String.Format("Bearer {0}", authResult.AccessToken));

**C#-Beispiel: Power BI-Datasets abrufen**

        protected void getDatasetsButton_Click(object sender, EventArgs e)
        {
            string responseContent = string.Empty;
            //The resource Uri to the Power BI REST API resource
            string datasetsUri = "https://api.powerbi.com/v1.0/myorg/datasets";
            //Configure datasets request
            System.Net.WebRequest request = System.Net.WebRequest.Create(datasetsUri) as System.Net.HttpWebRequest;
            request.Method = "GET";
            request.ContentLength = 0;
            request.Headers.Add("Authorization", String.Format("Bearer {0}", authResult.AccessToken));
            //Get datasets response from request.GetResponse()
            using (var response = request.GetResponse() as System.Net.HttpWebResponse)
            {
                //Get reader from response stream
                using (var reader = new System.IO.StreamReader(response.GetResponseStream()))
                {
                    responseContent = reader.ReadToEnd();
                    //Deserialize JSON string
                    //JavaScriptSerializer class is in System.Web.Script.Serialization
                    JavaScriptSerializer json = new JavaScriptSerializer();
                    Datasets datasets = (Datasets)json.Deserialize(responseContent, typeof(Datasets));
                    resultsTextbox.Text = string.Empty;
                    //Get each Dataset from 
                    foreach (dataset ds in datasets.value)
                    {
                        resultsTextbox.Text += String.Format("{0}\t{1}\n", ds.Id, ds.Name);
                    }
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

<a name="add"/>
##Hinzufügen einer Azure Active Directory-Authentifizierungsbibliothek

In einer .NET-Client-App verwenden Sie **AuthenticationContext** in der Active Directory-Authentifizierungsbibliothek, um ein Azure-Zugriffstoken abzurufen.
Sie können das NuGet-Paket für die Active Directory-Authentifizierungsbibliothek von Visual Studio installieren.
Wenn Sie ein NuGet-Paket installieren, erstellt Visual Studio einen Verweis auf die erforderlichen Assemblys.

1. Klicken Sie mit der rechten Maustaste auf eine Lösung.
2. Wählen Sie "NuGet-Pakete verwalten".
3. Suchen Sie die Active Directory-Authentifizierungsbibliothek.
4. Wählen Sie in der Liste der Pakete die Active Directory-Authentifizierungsbibliothek aus, und klicken Sie auf "Installieren".

##Verwandte Themen

* [Azure AD-Authentifizierungsbibliothek für .NET](https://msdn.microsoft.com/en-us/library/azure/jj573266.aspx)
* [Active Directory-Authentifizierungsbibliothek (ADAL) v1 für .NET](http://www.cloudidentity.com/blog/2013/09/12/active-directory-authentication-library-adal-v1-for-net-general-availability/)
* [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/en-us/library/azure/dn645545.aspx)
* [Gewähren eines Autorisierungscodes](https://msdn.microsoft.com/en-us/library/azure/dn645542.aspx)
* [Authentifizierungsszenarien für Azure AD](https://msdn.microsoft.com/en-US/library/azure/dn499820.aspx)





