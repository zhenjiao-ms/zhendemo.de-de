---
title: Authentifizieren f&#252;r Power&#160;BI-Dienst
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b3e918a-9699-414d-9c25-87ecd1fa972b
robots: noindex,nofollow
---
# Authentifizieren f&#252;r Power&#160;BI-Dienst
---


Dieser Artikel ist eine Einführung in die Authentifizierung in Power BI und bietet Informationen zum Abrufen eines Zugriffstokens mithilfe einer Client-ID.
Wenn Sie mit dem Erstellen einer Power BI-APP loslegen möchten, finden Sie Informationen unter [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md).

Die Power BI-REST-API ist eine REST-basierte API, die den programmgesteuerten Zugriff auf Dashboard-Ressourcen wie Datasets, Tabellen und Zeilen ermöglicht.
Eine Einführung in die Power BI-REST-API erhalten Sie unter [Einführung in die Power BI-REST-API](Introduction-to-Power-BI-REST-API.md).
Um eine sichere Anmeldung und Autorisierung Ihrer App zu ermöglichen, authentifizieren Sie sie bei **Azure Active Directory** (Azure AD).


###Inhalt dieses Artikels

- [Einführung in die Authentifizierung in Power BI](#intro)
- [Client-ID für Azure-App](#clientID)
- [Geheimer Clientschlüssel für Azure-Web-App](#clientSecret)

<a name="intro"/>
##Einführung in die Authentifizierung in Power BI

Power BI-Apps sind in ** Azure Active Directory** (Azure AD) integriert, um eine sichere Anmeldung und Autorisierung Ihrer App zu ermöglichen.
Zum Integrieren einer Power BI-App in Azure AD registrieren Sie Ihre Anwendung über das Azure-Verwaltungsportal in Azure AD.
Wenn Sie eine App in Azure Active Directory registrieren, wird die Authentifizierung nach Azure AD ausgelagert.
Bei der App-Registrierung geben Sie in Azure AD die Speicher-URL Ihrer Anwendung, die Ziel-URL zum Senden von Antworten nach der Authentifizierung und den URI zum Identifizieren Ihrer Anwendung an.
Wenn Sie eine Client-App oder Web-App in Azure AD registrieren, übergeben Sie Ihren App-Zugriff an die Power BI-REST-API.

Eine Power BI-App verwendet eine **Client-ID**, um sich bei Azure AD zu identifizieren.
Weitere Informationen finden Sie unter [Client-ID für Azure-App](#clientID).
Für eine Web-App benötigen Sie auch einen geheimen Clientschlüssel.
Weitere Informationen finden Sie unter [Geheimer Clientschlüssel für Azure-Web-App](#clientSecret).

So registrieren und authentifizieren Sie eine Power BI-App:

- **Power BI-Client-App**: Siehe [Registrieren einer Client-App](Register-a-client-app.md) und [Authentifizieren einer Power BI-Client-App](Authenticate-a-client-app.md).

- **Power BI-Web-App**: Siehe [Registrieren einer Web-App](Register-a-web-app.md) und [Authentifizieren einer Power BI-Web-App](Authenticate-a-web-app.md).

- So verwenden Sie die Azure-Authentifizierung auf verschiedenen Plattformen: Die [Azure-Authentifizierungsbibliotheken](https://msdn.microsoft.com/library/azure/dn151135.aspx) sind auf unterschiedlichen Plattformen verfügbar. Dies ermöglicht es Entwicklern, Benutzer einfach im Cloud-basierten oder lokalen Active Directory (AD) zu authentifizieren, sodass sie zum Schutz von API-Aufrufen Zugriffstoken erhalten.
    Dieses Thema enthält eine Übersicht über die auf unterschiedlichen Plattformen verfügbaren Authentifizierungsbibliotheken und damit verbundene nützliche Ressourcen, einschließlich Quellcode und Beispiele.

<a name="clientID"/>
##Client-ID für Azure-App

Eine Azure-App besitzt eine **Client-ID**, mit der sie sich bei den Benutzern identifiziert, von denen sie Berechtigungen anfordert.
Mit einer **Client-ID** rufen Sie ein Authentifizierungstoken ab.
Informationen zum Abrufen einer Azure-**Client-ID** finden Sie unter [Abrufen einer Client-App-ID](Register-a-client-app.md#clientID).

Ein vollständiges Beispiel, das zeigt, wie eine Azure-**Client-ID** zum Authentifizieren einer Client-App verwendet wird, finden Sie unter [Authentifizieren einer Client-App](Authenticate-a-client-app.md).

Der folgende C#-Code verwendet beispielsweise zum Abrufen eines Zugriffstokens eine Azure-App-Client-ID.


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

<a name="clientSecret"/>
##Geheimer Clientschlüssel für Azure-Web-App

Wenn Sie eine Web-App registrieren, erhalten Sie einen geheimen Client**schlüssel**.
Der geheime Client**schlüssel** dient der sicheren Identifizierung der Web-App beim **Power BI-Dienst**.
Informationen zum Abrufen eines geheimen Azure-Client**schlüssels** finden Sie unter [Abrufen eines geheimen Clientschlüssels](Register-a-web-app.md#clientSecret).

Ein vollständiges Beispiel, das zeigt, wie eine Azure-**Client-ID** und ein geheimer Client**schlüssel** zum Authentifizieren einer Web-App verwendet werden, finden Sie unter [Authentifizieren einer Web-App](Authenticate-a-web-app.md).

##Verwandte Themen

- [Erste Schritte beim Erstellen einer Power BI-App](Get-started-creating-a-Power-BI-app.md)
- [Abrufen eines Azure Active Directory-Mandanten](https://azure.microsoft.com/en-us/documentation/articles/active-directory-howto-tenant/)
- [Erstellen eines Azure Active Directory-Mandanten](Create-an-Azure-Active-Directory-tenant.md)
- [Registrieren einer Client-App](Register-a-client-app.md)
- [Registrieren einer Web-App](Register-a-web-app.md)



