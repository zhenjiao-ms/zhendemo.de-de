---
title: Erste Schritte beim Erstellen von Power&#160;BI-Apps
ms.custom: na
ms.prod: powerbi
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04f06c61-2dbf-4f21-b1a5-652dc773f481
robots: noindex,nofollow
---
# Erste Schritte beim Erstellen von Power&#160;BI-Apps
Power BI zeigt interaktive Dashboards an und kann aus vielen unterschiedlichen Datenquellen in Echtzeit erstellt und aktualisiert werden.
Mit der Microsoft Power BI-REST-API können Sie programmgesteuert auf bestimmte Power BI-Ressourcen zugreifen.
Die API ermöglicht es Ihnen, Apps auf allen Plattformen zu erstellen, die das Aufrufen von REST-Vorgängen unterstützen.

Sie können auf Apiary die **[Power BI-REST-API testen](http://docs.powerbi.apiary.io/#)**, ohne eine App zu erstellen.
Weitere Informationen zur Power BI-REST-API finden Sie unter [Referenz für Power BI-REST-API](Power-BI-REST-API-reference.md).

Bevor Sie eine Power BI-App erstellen können, benötigen Sie ein **Azure Active Directory-Verzeichnis** und einen **Power BI-Dienst**.
Zum Erstellen einer Power BI-App benötigen Sie Folgendes:

1.  Ein **Azure Active Directory-Verzeichnis** mit mindestens einem Organisationsbenutzer.
    
    *   Wenn Sie kein Azure Active Directory (AAD)-Verzeichnis haben, finden Sie weitere Informationen unter [Einrichten von Azure Active Directory](#setup).
    *   Informationen zum Erstellen eines Organisationsbenutzers in einem Azure AD-Mandanten finden Sie unter [Hinzufügen eines Benutzers zu Azure Active Directory](#newuser).
2.  Einen **Power BI-Dienst**.
    Sie können sich schnell und einfach bei Power BI registrieren. Eine Anleitung finden Sie unter [Registrieren bei Power BI](https://portal.office.com/Start?Sku=a403ebcc-fae0-4ca2-8c8c-7a907fd6c235/).
    
    *                 **Hinweis** Um sich bei Power BI registrieren zu können, benötigen Sie ein AAD-Organisationsbenutzerkonto.
3.  Sobald Sie den Power BI-Dienst verfügbar haben, können Sie Ihre Client-App oder Web-App registrieren.
    Informationen zum Registrieren Ihrer Power BI-App finden Sie unter [Registrieren eine Client-App](Register-a-client-app.md) oder [Registrieren eine Web-App](Register-a-web-app.md).

<a name="setup"></a>

###Einrichten von Azure Active Directory

Power BI-Apps sind in Azure Active Directory (AD) integriert, um eine sichere Anmeldung und Autorisierung Ihrer App zu ermöglichen.
Zum Integrieren einer Power BI-App in Azure AD registrieren Sie Ihre Anwendung über das Azure-Verwaltungsportal in Azure AD.

####So richten Sie Azure Active Directory ein

1.  Navigieren Sie zu https://manage.windowsazure.com und melden Sie sich mit dem Konto an, das über ein Azure-Abonnement verfügt.
2.  Klicken Sie im linken Bereich auf das Verwaltungssymbol für **ACTIVE DIRECTORY**.
3.  Klicken Sie am unteren Rand der Seite auf **NEU**.
4.  Wählen Sie **APP-DIENSTE** > **VERZEICHNIS** > **BENUTZERDEFINIERT ERSTELLEN**.
    
                  ![Erstellen eines neuen AD-Verzeichnisses]
5.  Geben Sie einen Namen und den Domänennamen ein.
    Wählen Sie für das Land bzw. die Region die Vereinigten Staaten oder das Land, in dem Power BI verfügbar ist.
    
                  ![Hinzufügen von Verzeichnissen]
6.  Klicken Sie auf das Symbol "OK".
    Ein Azure Active Directory-Verzeichnis wird erstellt.
    <a name="newuser"></a>

####Hinzufügen eines Benutzers zu Azure Active Directory

1.  Klicken Sie in Ihrem Azure Active Directory-Verzeichnis auf **BENUTZER**.
    
                  ![Hinzufügen von Benutzern]
2.  Klicken Sie unten auf der Seite auf **BENUTZER HINZUFÜGEN**.
    Für die Registrierung bei Power BI wird ein Benutzerkonto verwendet.
    
     ![Hier Bildbeschreibung eingeben]
    
    1.  Wählen Sie für **ART DES BENUTZERS** die Option **Neuer Benutzer in Ihrem Unternehmen**.
    2.  Geben Sie im Feld **BENUTZERNAME** den Benutzernamen ein.
    3.  Geben Sie {Azure_AD_Name}. onmicrosoft.com ein.
        Der Azure AD-Name ist mit der Mandanten-ID identisch.
    4.  Klicken Sie auf **Weiter**.
        
                      ![Benutzerprofil]
    5.  Geben Sie ein **Benutzerprofil** ein.
    6.  Klicken Sie auf **Weiter**.
        Für "ROLLE" können Sie "Benutzer" verwenden.
    7.  Klicken Sie auf **Erstellen**, um ein temporäres Kennwort zu erstellen.
        Dem neuen Benutzer wird ein temporäres Kennwort zugewiesen, das bei der ersten Anmeldung geändert werden muss.
    8.  Klicken Sie auf der Seite "Vorübergehendes Kennwort abrufen" auf das Symbol für **Fertigstellen**.
        Ein neuer Azure AD-Benutzer wird erstellt.

Nachdem Sie ein **Azure Active Directory-Verzeichnis** erstellt haben, registrieren Sie sich bei Power BI.

<a name="signup"></a>

###Registrieren bei Power BI

1.                [Registrieren Sie sich bei Power BI](https://portal.office.com/Start?Sku=a403ebcc-fae0-4ca2-8c8c-7a907fd6c235/).
2.  Geben Sie auf der Registrierungsseite von Power BI den Benutzer ein, den Sie im Schritt [Hinzufügen eines Benutzers zu Azure Active Directory](#newuser) erstellt haben.
    Das Format lautet {Benutzername}@{Azure_AD_Name}.onmicrosoft.com.

Während der Registrierung wird der Benutzer überprüft und der Power BI-Dienst in Ihrem Azure AD-Verzeichnis bereitgestellt.
Nach der Bereitstellung von Power BI ist ein Power BI-Mandant für einen Benutzer verfügbar.

              **Hinweis**:  Bei der erstmaligen Registrierung bei Power BI kann die Bereitstellung bis zu 30 Minuten dauern.

Nachdem Sie sich bei Power BI registriert haben, können Sie eine Power BI-App registrieren.

###Registrieren Ihrer Power BI-App

Sie können jetzt eine Power BI-App registrieren.
Informationen zum Registrieren einer App finden Sie unter [Registrieren eine Client-App](Register-a-client-app.md) oder [Registrieren eine Web-App](Register-a-web-app.md).

Nach der Registrierung Ihrer App können Sie die Power BI-REST-API-Vorgänge aufrufen.
Die folgenden Beispiele sollen Ihnen beim Schreiben Ihrer ersten Apps helfen.

###Beispiele für Power BI-REST-API

*                 [Beispiel für Client-App-Authentifizierung](Client-app-authentication-sample.md): Hier sehen Sie, wie Sie ein Zugriffstoken für eine Client-App abrufen und alle Power BI-REST-Vorgänge ausführen.
*                 [Beispiel für ASP.NET-Web-App](Authenticate-a-web-app.md) Hier sehen Sie, wie Sie ein Zugriffstoken für eine Web-App abrufen und einen Power BI-REST-Vorgang ausführen.


