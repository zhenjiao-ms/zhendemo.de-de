---
title: Registrieren einer Web-App
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e59e2fad-8b10-4e24-b3fa-30f4cc9b4a62
robots: noindex,nofollow
---
# Registrieren einer Web-App
---

In diesem Artikel erfahren Sie, wie Sie eine Power BI-Web-App in Azure Active Directory (Azure AD) registrieren.
Damit Ihre Anwendung auf die Power BI-REST-API zugreifen kann, müssen Sie sie in **Azure Active Directory** registrieren.
Auf diese Weise können Sie eine Identität für Ihre Anwendung erstellen und Berechtigungen für Power BI-REST-Ressourcen angeben.
Eine Liste mit Power BI-Berechtigungen finden Sie unter [Power BI-Berechtigungen](Power-BI-permissions.md).

**Wichtig** Bevor Sie eine Power BI-App registrieren, brauchen Sie ein [Azure Active Directory und einen Organisationsbenutzer](Create-an-Azure-Active-Directory-tenant.md) sowie ein [Power BI-Dienstkonto](Sign-up-for-Power-BI-service.md).


Es gibt zwei Methoden zum Registrieren Ihrer Web-App: mit dem  Power BI-App-Registrierungstool oder mit dem Azure-Verwaltungsportal.
Das Power BI-App-Registrierungstool ist die einfachste Option, da nur wenige Felder ausgefüllt werden müssen.
Mit diesem Tool müssen Sie jedoch das Azure-Verwaltungsportal verwenden, um Ihre App-Einstellungen zu verwalten.

###Inhalt dieses Artikels

- [Registrieren Sie eine Web-App mit dem Azure-Verwaltungsportal](#web)
- [Abrufen einer Client-ID ](#clientID)
- [Abrufen eines geheimen Clientschlüssels](#clientSecret)

<a name="web"></a>
##Registrieren Sie eine Web-App mit dem Azure-Verwaltungsportal

Wenn Sie eine Web-App registrieren, erhalten Sie eine **Client-ID** und einen geheimen **Clientschlüssel**.
Die Anwendung identifiziert sich mithilfe der **Client-ID** bei den Benutzern, von denen sie Berechtigungen anfordert.
Der geheime **Clientschlüssel** dient der sicheren Identifizierung der Web-App beim **Power BI-Dienst**.

Informationen zum Authentifizieren einer Web-App mit einer Azure AD-**Client-ID** und einem geheimen **Clientschlüssel** finden Sie unter [Authentifizieren einer Web-App](Authenticate-a-web-app.md).

So registrieren Sie eine Client-App:

1. Akzeptieren Sie die [Nutzungsbedingungen für die Microsoft Power BI-API](https://powerbi.microsoft.com/en-us/api-terms).
2. Melden Sie sich bei Ihrem Microsoft Azure-Abonnement unter https://manage.windowsazure.com an.
3. Wählen Sie im linken Dienstbereich **ACTIVE DIRECTORY**.
4. Klicken Sie auf ein beliebiges Active Directory-Verzeichnis.

    ![Schritt 3](../Image/Register-app-3.png)

5. Klicken Sie auf **ANWENDUNGEN**.

    ![Schritt 4](../Image/Register-app-4.png)

6. Klicken Sie auf **HINZUFÜGEN**.
    
    ![Schritt 5](../Image/Register-app-5.png)
7.  Geben Sie unter **Erzählen Sie uns von Ihrer Anwendung** im Feld **NAME** einen Namen ein. Wählen Sie als Typ **WEBANWENDUNG UND/ODER WEB-API**, und klicken Sie auf das Symbol **Weiter**.

    ![Schritt 7](../Image/RegisterWebApp7.png)

8. Geben Sie unter **App-Eigenschaften** eine **ANMELDE-URL** und einen **APP-ID-URI** ein.
    Die **ANMELDE-URL** ist Ihre Web-App-URL, beispielsweise https://localhost:44307.
    Der **APP-ID-URI** ist Ihr Azure-Mandanten-URI, gefolgt vom Namen Ihrer App.
    Beispiel: https://IhrMandant.onmicrosoft.com/IhreWebApp.

    ![Schritt 8](../Image/RegisterWebApp8.png)

9.  Klicken Sie auf das Symbol **Fertig stellen**.
10. Wählen Sie auf der Anwendungsseite **KONFIGURIEREN** aus.
    Auf der Seite **KONFIGURIEREN** finden Sie eine **Client-ID** und einen **Schlüssel** für Ihre App.

    ![Schritt 10](../Image/RegisterWebApp10.png)

11. Für eine Web-App benötigen Sie einen geheimen **Clientschlüssel**.
    Wählen Sie im Abschnitt **Schlüssel** eine Dauer aus.
    Der Schlüssel wird nach dem **Speichern** angezeigt.
    Erstellen Sie unbedingt eine Kopie des Schlüssels, da er ansonsten für zukünftige Navigationsvorgänge auf der Konfigurationsseite nicht verfügbar ist.

12. Klicken Sie auf der Seite **KONFIGURATION** auf **Anwendung hinzufügen**.
13. Wählen Sie unter **Berechtigungen für andere Anwendungen** die Option **Power BI-Dienst** aus.

    ![Schritt 12](../Image/Register-app-12.png)

    **Wichtig** Wenn **Power BI-Dienst** in der Liste **Berechtigungen für andere Anwendungen** nicht angezeigt wird, registrieren Sie sich beim [Power BI-Dienst](https://www.powerbi.com/).
    Zum Registrieren beim Power BI-Dienst benötigen Sie in Ihrem Azure Active Directory (AD)-Mandanten mindestens einen Organisationsbenutzer.
    Wenn Sie keinen AAD-Mandanten (Azure Active Directory) besitzen, finden Sie unter [Einrichten von Azure Active Directory](Setup-Azure-Active-Directory.md) Informationen zum Erstellen eines Azure AD-Mandanten und eines damit verbundenen Organisationsbenutzers.

14. Klicken Sie in der unteren rechten Ecke der Seite auf das Symbol **Fertig stellen**.
15. Wählen Sie in der Gruppe **Berechtigungen für andere Anwendungen** alle unter **Delegierte Berechtigungen** angezeigten Berechtigungen.
    Weitere Informationen zu Power BI-Berechtigungen finden Sie unter [Power BI-Berechtigungen](Power-BI-Permissions.md).

    ![Schritt 14](../Image/Register-app-14.png)

16. Klicken Sie auf **Speichern**.

    ** Wichtig **
    Für eine Web-App benötigen Sie einen geheimen **Clientschlüssel**.
    Der geheime Client**schlüssel** wird nach dem **Speichern** angezeigt.
    Erstellen Sie unbedingt eine Kopie des Schlüssels, da er ansonsten für zukünftige Navigationsvorgänge auf der Konfigurationsseite nicht verfügbar ist.


<a name="clientID"></a>
##Abrufen einer Client-App-ID

Wenn Sie eine Web-App registrieren, erhalten Sie eine **Client-ID**.
Die Anwendung identifiziert sich mithilfe der **Client-ID** bei den Benutzern, von denen sie Berechtigungen anfordert.

So rufen Sie eine Client-App-ID ab:

1. Melden Sie sich bei Ihrem Microsoft Azure-Abonnement unter https://manage.windowsazure.com an.
2. Wählen Sie im linken Dienstbereich **ACTIVE DIRECTORY** aus.
3. Klicken Sie auf ein beliebiges Active Directory-Verzeichnis.
4. Klicken Sie auf **ANWENDUNGEN**.
5. Wählen Sie eine Anwendung aus.
6. Wählen Sie auf der Anwendungsseite **KONFIGURIEREN** aus.
7. Kopieren Sie auf der Seite **KONFIGURIEREN** die **CLIENT-ID**.

    ![Schritt 1.3](../Image/Register-app-3a.png)

<a name="clientSecret"></a>
##Abrufen eines geheimen Clientschlüssels

Für eine Web-App benötigen Sie einen geheimen **Clientschlüssel**.
Wenn Sie eine Web-App registrieren, generiert Azure AD einen Schlüssel (siehe Schritt 11 oben).
Wählen Sie im Abschnitt **Schlüssel** eine Dauer aus.
Der Schlüssel wird nach dem Speichern angezeigt.
Erstellen Sie unbedingt eine Kopie des Schlüssels, da er ansonsten für zukünftige Navigationsvorgänge auf der Konfigurationsseite nicht verfügbar ist.

##Nächste Schritte zum Erstellen einer Power BI-App

- [Erstellen Ihrer Power BI-App](Introduction-to-creating-a-Power-BI-app.md)
- [Informationen zum Authentifizieren in Azure AD](Authenticate-to-Power-BI-service.md)





