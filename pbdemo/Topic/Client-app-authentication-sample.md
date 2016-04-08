---
title: Beispiel f&#252;r Client-App-Authentifizierung
ms.custom: na
ms.prod: powerbi
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e14cde2-b7d0-48db-ae1e-6497c834afe1
robots: noindex,nofollow
---
# Beispiel f&#252;r Client-App-Authentifizierung
              [Herunterladen des Beispiels für erste Schritte mit .NET](https://github.com/PowerBI/getting-started-for-dotnet/tree/master/PBIGettingStarted)

Das Beispiel für erste Schritte mit Power BI behandelt folgende Themen:

*   Registrieren einer systemeigenen Client-App
*   Abrufen eines Zugriffstoken für Azure Active Directory
*   Abrufen aller Datasets
*   Erstellen eines Datasets
*   Hinzufügen von Zeilen zu einem Dataset
*   Hinzufügen einer Active Directory-Authentifizierungsbibliothek

Wenn Sie zum ersten Mal eine Power BI-App erstellen, finden Sie unter [Erste Schritte beim Erstellen von Power BI-Apps](Getting-started-creating-Power-BI-apps.md) nützliche Informationen.

Registrieren Sie die Beispiel-Client-App vor dem Ausführen des Beispielcodes in Ihrem Azure Active Directory-Verzeichnis.
Power BI-Apps sind in Azure Active Directory (AD) integriert, um eine sichere Anmeldung und Autorisierung Ihrer App zu ermöglichen.
Zum Integrieren einer Power BI-App in Azure AD registrieren Sie Ihre Anwendung über das Azure-Verwaltungsportal in Azure AD.

Informationen zum Registrieren einer Anwendung in Azure Active Directory finden Sie unter [Registrieren einer Client-App](Register-a-client-app.md).

##Registrieren einer systemeigenen Client-App

Wenn Sie eine systemeigene Client-App registrieren (z. B. eine Konsolenanwendung), erhalten Sie eine Client-ID.
Die Anwendung identifiziert sich mithilfe der Client-ID bei den Benutzern, von denen sie Berechtigungen anfordert.
Bei einer .NET-Anwendung rufen Sie mit der Client-ID ein Authentifizierungstoken ab.
Weitere Informationen über die Power BI-Authentifizierung finden Sie unter [Authentifizieren bei Power BI](http://go.microsoft.com/fwlink/?LinkId=519359).

Informationen zum Registrieren einer Anwendung in Azure Active Directory finden Sie unter [Registrieren einer Client-App](Register-a-client-app.md).

###Abrufen einer Client-App-ID

1.  Wählen Sie im linken Bereich des **Microsoft Azure-Portals** die Option **ACTIVE DIRECTORY** oder in der Liste mit allen Elementen die Option **Verzeichnis******.
    Wenn Sie kein Azure Active Directory-Verzeichnis für Ihre Organisation haben, finden Sie weitere Informationen unter [Einrichten von Azure Active Directory](https://msdn.microsoft.com/en-US/library/mt203553.aspx).
2.  Wählen Sie auf der Seite **Azure Active Directory** die Option **ANWENDUNGEN**.
3.  Wählen Sie Ihre Anwendung aus.
4.  Kopieren Sie auf der Seite **KONFIGURIEREN** die **CLIENT-ID**.

###So führen Sie das Beispiel aus

1.  Ersetzen Sie **clientID** durch Ihre Client-App-ID.
    Weitere Informationen zum Abrufen einer Clients-App-ID finden Sie unter [Abrufen einer Client-App-ID](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
    
    
    ```
    private static string clientID = "";

    ```
    

###Abrufen eines Zugriffstoken für Azure Active Directory

1.  Fügen Sie Ihrem Projekt das NuGet-Paket für die Active Directory-Authentifizierungsbibliothek hinzu.
    Informationen zum Hinzufügen einer Active Directory-Authentifizierungsbibliothek finden Sie unter [Hinzufügen einer Active Directory-Authentifizierungsbibliothek](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
2.  Fügen Sie einen Verweis auf Microsoft.IdentityModel.Clients.ActiveDirectory hinzu.
3.  Erstellen Sie eine neue Instanz von **AuthenticationContext** zum Übergeben einer Zertifizierungsstelle.
    Eine Zertifizierungsstelle kennt den Dienst, den Sie aufrufen möchten, und weiß, wie Sie authentifiziert werden.
4.  Beziehen Sie ein Token für **Azure Active Directory**, indem Sie **AcquireToken** aufrufen.
    //Client-App-ID.
    Weitere Informationen zum Abrufen einer Client-App-ID finden Sie unter [Registrieren einer App](Register-an-app.md).


```
     private static string clientID = ""; 

     //Power BI resource uri
     private static string resourceUri = "https://analysis.windows.net/powerbi/api";           
     //OAuth2 authorityUri
     private static string authorityUri = "https://login.windows.net/common/oauth2/authorize";

     //Create a new AuthenticationContext passing an Authority.
     AuthenticationContext authContext = new AuthenticationContext(authority);

     //Get an Azure Active Directory token by calling AcquireToken
     string token = authContext.AcquireToken(resourceUri, clientID, new Uri(redirectUri)).AccessToken.ToString();

```


###Abrufen aller Datasets

1.  Rufen Sie ein Zugriffstoken ab.
    Weitere Informationen finden Sie unter [Abrufen eines Zugriffstokens für Azure Active Directory](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
2.  Erstellen Sie mit einer GET-Methode ein Instanz von **HttpWebRequest**.
    Im Beispiel wird DatsetRequest(datasetsUri, "GET", AccessToken) verwendet, um eine Anforderung an den Dienst zu stellen.
    Weitere Informationen finden Sie unter [Erstellen einer Power BI-Anforderung](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
3.  Beziehen Sie vom Power BI-Dienst eine Antwort.


```
    private static string datasetsUri = "https://api.powerbi.com/v1.0/myorg/datasets";

    static PBIDatasets GetAllDatasets()
    {
        PBIDatasets PBIDatasets = null;

        //In a production application, use more specific exception handling.
        try
        {
            //Create a GET web request to list all datasets
            HttpWebRequest request = DatasetRequest(datasetsUri, "GET", AccessToken());

            //Get HttpWebResponse from GET request
            string responseContent = GetResponse(request);

            //Deserialize JSON string from response
            PBIDatasets = JsonConvert.DeserializeObject<PBIDatasets>(responseContent);

        }
        catch (Exception ex)
        {
            //In a production application, handle exception
        }

        return PBIDatasets;
    }

```


###Erstellen eines Datasets

1.  Rufen Sie ein Zugriffstoken ab.
    Weitere Informationen finden Sie unter [Abrufen eines Zugriffstokens für Azure Active Directory](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
2.  Erstellen Sie mit einer POST-Methode eine Instanz von **HttpWebRequest**.
    Im Beispiel wird DatsetRequest(datasetsUri, "POST", AccessToken) verwendet, um eine Anforderung an den Dienst zu stellen.
    Weitere Informationen finden Sie unter [Erstellen einer Power BI-Anforderung](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
3.  Beziehen Sie vom Power BI-Dienst eine Antwort.


```
    static void CreateDataset()
    {
        //In a production application, use more specific exception handling.           
        try
        {
            //Create a POST web request to list all datasets
            HttpWebRequest request = DatasetRequest(datasetsUri, "POST", AccessToken());

            var datasets = GetAllDatasets().Datasets.GetDataset(datasetName);

            if (datasets.Count() == 0)
            {
                //POST request using the json schema from Product
                Console.WriteLine(PostRequest(request, new Product().ToJsonSchema(datasetName)));
            }
            else
            {
                Console.WriteLine("Dataset exists");
            }
        }
        catch (Exception ex)
        {

            Console.WriteLine(ex.Message);
        }
    }
 ```

###How to add rows to a dataset
1.  Get an access token. See How to get an [How to get an Azure Active Directory access token](https://msdn.microsoft.com/en-US/library/dn889824.aspx).
2.  Create an **HttpWebRequest** using a POST method. The sample uses DatsetRequest(datasetsUri, "POST", AccessToken) to make a request to the service. See [How to make a Power BI request](https://msdn.microsoft.com/en-US/library/dn889824.aspx). 
3.  To identify the dataset to add rows to, the resource uri for a POST method has a dataset id.
4.  Get a response from the Power BI service.


```

    static void AddRowsFromClass()
    {
        //Get dataset id from a table name
        string datasetId = GetAllDatasets().Datasets.GetDataset(datasetName).First().Id;

        //In a production application, use more specific exception handling. 
        try
        {
            HttpWebRequest request = DatasetRequest(String.Format("{0}/{1}/tables/{2}/rows", datasetsUri, datasetId, tableName), "POST", AccessToken());

            //Create a list of Product
            List<Product> products = new List<Product>
            {
                new Product{ProductID = 1, Name="Adjustable Race", Category="Components", IsCompete = true, ManufacturedOn = new DateTime(2014, 7, 30)},
                new Product{ProductID = 2, Name="LL Crankarm", Category="Components", IsCompete = true, ManufacturedOn = new DateTime(2014, 7, 30)},
                new Product{ProductID = 3, Name="HL Mountain Frame - Silver", Category="Bikes", IsCompete = true, ManufacturedOn = new DateTime(2014, 7, 30)},
            };

            //POST request using the json from a list of Product
            //NOTE: Posting rows to a model that is not created through the Power BI API is not currently supported. 
            //      Please create a dataset by posting it through the API following the instructions on http://dev.powerbi.com.
            Console.WriteLine(PostRequest(request, products.ToJson(JavaScriptConverter<Product>.GetSerializer())));

        }
        catch (Exception ex)
        {

            Console.WriteLine(ex.Message);
        }
    }


 ```

###How to make a Power BI request
To make a GET or POST request on the Power BI service:
1.  Create an **HttpWebRequest** from a dataset resource url. 
2.  Set **Method** to GET or POST. GET returns JSON objects, such as a dataset, from the service. POST is similar to saving data, it POST's JSON objects to the service.
3.  Add an Authorization header to an **HttpWebRequest** object.

```
private static string datasetsUri = "https://api.powerbi.com/v1.0/myorg/datasets";

  private static HttpWebRequest DatsetRequest(string datasetsUri, string method, string authorizationToken)
  {
      HttpWebRequest request = System.Net.WebRequest.Create(datasetsUri) as System.Net.HttpWebRequest;
      request.KeepAlive = true;
      request.Method = method;
      request.ContentLength = 0;
      request.ContentType = "application/json";

      //Add an Authorization header to an HttpWebRequest object
      request.Headers.Add("Authorization", String.Format( "Bearer {0}", authorizationToken));

      return request;
  }
  ```

###How to get a JSON response from the service
To GET a JSON response from an **HttpWebRequest** request, call request.**GetResponse()** and read the response into a **StreamReader**.

```
    private static string GetResponse(HttpWebRequest request)
    {
        string response = string.Empty;

        using (HttpWebResponse httpResponse = request.GetResponse() as System.Net.HttpWebResponse)
        {
            //Get StreamReader that holds the response stream
            using (StreamReader reader = new System.IO.StreamReader(httpResponse.GetResponseStream()))
            {
                response = reader.ReadToEnd();                 
            }
        }

        return response;
    }
```

###How to POST JSON to the service
To POST JSON to the service from an **HttpWebRequest** request and json string, write a json byte[] array into a **Stream**.

```
    private static void PostRequest(HttpWebRequest request, string json)
    {
        byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(json);
        request.ContentLength = byteArray.Length;

        //Write JSON byte[] into a Stream
        using (Stream writer = request.GetRequestStream())
        {
            writer.Write(byteArray, 0, byteArray.Length);
        }
    }

 ```


##Hinzufügen einer Active Directory-Authentifizierungsbibliothek

Sie können das NuGet-Paket für die **Active Directory-Authentifizierungsbibliothek** von Visual Studio installieren.
Wenn Sie ein NuGet-Paket installieren, erstellt Visual Studio einen Verweis auf die erforderlichen Assemblys.

1.  Klicken Sie mit der rechten Maustaste auf eine Lösung.
2.  Wählen Sie **NuGet-Pakete verwalten**.
3.  Suchen Sie die **Active Directory-Authentifizierungsbibliothek**.
4.  Wählen Sie in der Liste der Pakete die **Active Directory-Authentifizierungsbibliothek** aus, und klicken Sie auf **Installieren**.


