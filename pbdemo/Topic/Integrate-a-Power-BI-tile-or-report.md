---
title: Integrieren einer Power BI-Kachel in eine App
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39f37a6c-b5d0-4b96-bc0b-7a5c7b4e00b2
robots: noindex,nofollow
---
# Integrieren einer Power BI-Kachel in eine App
---

Laden Sie das [Beispiel für die Integration von Power BI-Kacheln](https://github.com/PowerBI/Integrate-a-tile-into-an-app) von GitHub herunter.

Mit Power BI können Sie Anwendungsentwicklern die Integration von Power BI-**Kacheln** und -**Berichten** aus dem Power BI-Konto eines Benutzers durch Einbetten eines **IFrames** in eine App ermöglichen, wie etwa eine mobile App oder eine Web App. Der PowerBI.com-Dienst ermöglicht geschäftlichen Benutzern das Erstellen und Personalisieren von Diagrammen, Berichten und Dashboards in ihrem eigenen Power BI-Konto. Durch die Möglichkeit zum Einbetten einer **Kachel** oder eines **Berichts** können Sie Kacheln oder Berichte in eine App integrieren. Hier sind einige der Vorzüge, die sich aus dem Integrieren von Power BI-Kacheln oder -Berichten in Apps ergeben:

-   Stellen Sie Benutzern gegen eine geringe Investition anpassbare Datenkacheln oder -berichte in einer App bereit.
-   Die Benutzer können Daten aus beliebigen Datenquellen außerhalb der Anwendung integrieren, um das Kontextverständnis zu verbessern.
-   Die Benutzer können auf eine eingebettete Power BI-Kachel oder einen eingebetteten Power BI-Bericht klicken, um zum Power BI-**Dashboard** zu navigieren.

Die Integration einer Power BI-**Kachel** oder eines Power BI-**Berichts** in eine App erfolgt mithilfe eines **IFrame**-HTML-Elements. Beispielsweise können Sie eine benutzerdefinierte mobile App erstellen, um Power BI-Kacheln in Echtzeit auf den mobilen Geräten von Benutzern anzuzeigen. Nachdem Sie einen IFrame in eine App eingebettet haben, können Sie einen Klickereignishandler hinzufügen, damit der Benutzer zum **Dashboard** navigieren kann.

Dies sind die Schritte, die zum Integrieren einer Power BI-Kachel oder eines Power BI-Berichts in eine App ausgeführt werden müssen. Sie können außerdem das Beispiel [Integrate a tile or report into an app](https://github.com/PowerBI/Integrate-a-tile-or-report-into-an-app) herunterladen.

###So erstellen Sie eine App, die eine Power BI-Kachel integriert

| **Schritt**| **Siehe auch**
|:-|:-
| Für das Konto des Power BI-Benutzers muss mindestens ein **Dashboard** mit mindestens einer **Kachel** darauf konfiguriert sein.| Informationen zum Registrieren für Power BI finden Sie unter [Registrieren beim Power BI-Dienst](https://msdn.microsoft.com/en-US/library/mt186467.aspx).<br/><br/>Informationen zum Integrieren einer **Kachel** in ein **Dashboard** mithilfe der Power BI REST-API finden Sie unter [Abrufen einer Power BI-Kachel](#gettile).
| Rufen Sie in einer App ein Azure AD-Token (Active Directory) ab.| Informationen zum Abrufen eines Azure AD-Authentifizierungstokens finden Sie unter [Authentifizieren für Power BI-Dienst](https://msdn.microsoft.com/en-US/library/mt203565.aspx).Erfahren Sie mehr zu [Azure Active Directory-Authentifizierungsbibliotheken](https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-libraries/)
| Rufen Sie für eine Kachel die **Dashboard**-ID und die **Kachel**-ID ab, um eine **IFrame**-Kachel in eine App einzubetten| Informationen zum Abrufen von **Dashboard**-ID und **Kachel**-ID finden Sie unter [Abrufen einer Power BI-Kachel](#gettile).
| Legen Sie in einer App das SRC-Attribut für einen **IFrame** auf die **embedUrl** fest.| Informationen zum Festlegen eines **IFrame**-SRC-Attributs finden Sie unter [Einbetten einer Power BI-Kachel in eine Web-App](#embed).
| Fügen Sie einen Klickereignishandler für die Navigation zum Power BI-Dashboard hinzu.| Weitere Informationen finden Sie unter [Navigieren von einer Power BI-Kachel zum Dashboard](#navigate)

<br/>
**Wichtig** Um eine sichere Anmeldung und Autorisierung Ihrer App zu ermöglichen, authentifizieren Sie sie bei **Azure Active Directory** (Azure AD). Bevor Sie mit dem Einbetten einer Power BI-Kachel in eine App beginnen, müssen Sie Ihre App in Azure Active Directory registrieren. Weitere Informationen finden Sie unter [Registrieren einer Client-App] (https://msdn.microsoft.com/en-us/library/dn877542.aspx) oder [Registrieren einer Web-App](https://msdn.microsoft.com/en-us/library/dn985955.aspx). Informationen zur Authentifizierung für den Power BI-Dienst finden Sie unter [Authentifizieren für Power BI-Dienst](https://msdn.microsoft.com/en-US/library/mt203565.aspx).

<a name="gettile"/>
##Abrufen einer Power BI-Kachel

Zum Abrufen einer Power BI-**Kachel** benötigen Sie zuerst die ID des Dashboards eines Benutzers. Sie können eine **Dashboard**-ID mit dem Vorgang [Get Dashboards](Get-Dashboards.md) abrufen. Sobald Sie über eine **Dashboard**-ID verfügen, können Sie die **Kachel** mithilfe des Vorgangs [Get Tiles](Get-Tiles.md) abrufen. Den vollständigen Quellcode, aus dem Sie ersehen können, wie eine Power BI-Kachel eingebettet wird, erhalten Sie durch den Download des Beispiels [Integrate a tile into an app](https://github.com/PowerBI/Integrate-a-tile-into-an-app).

Dies sind die erforderlichen Schritte zum Abrufen einer Power BI-**Kachel**:

**Schritt 1 – Abrufen eines Azure AD-Authentifizierungstokens**

Informationen zum Abrufen eines Azure AD-Authentifizierungstokens finden Sie unter [Authentifizieren einer Web-App](https://msdn.microsoft.com/en-us/library/mt143610.aspx) oder [Authentifizieren einer Client-App](https://msdn.microsoft.com/en-us/library/dn877545.aspx).

**Schritt 2 – Abrufen der Dashboards eines Benutzers**

Sie rufen die **Dashboards** eines Benutzers mithilfe einer GET-Webanforderung und dem folgenden REST-URI ab. Der Vorgang [Get Dashboards](Get-Dashboards.md) gibt ein JSON-Array der Dashboards des Benutzers zurück, das eine **ID** und mit **displayName** den Anzeigenamen enthält. Sie können die **Dashboard-ID** im Vorgang [Get Tiles](Get-Tiles.md) verwenden, um die Kacheln des Benutzers abzurufen. Weitere Informationen finden Sie unter **Schritt 3 – Abrufen der Kacheln eines Benutzers**. Der Vorgang ist wie folgt:

GET REST URI

    https://api.powerbi.com/beta/myorg/dashboards

JSON-Antwort


    {
    "@odata.context":"http://api.powerbi.com/beta/myorg/$metadata#dashboards","value":
    [
        {
            "id":"43127a01--e971d4cdc2fb",
            "displayName":"My Dashboard"
         }
    ]
    }

**Hinweis** Ein umfassendes Beispiel zum Abrufen von Dashboards finden Sie unter **getDashboardsButton_Click** innerhalb des Beispiels [Integrate a tile into an app](https://github.com/PowerBI/Integrate-a-tile-into-an-app).

**Schritt 3 – Abrufen der Kachelinformationen eines Benutzers**

Sie können die **Kacheln** eines Benutzers mithilfe einer GET-Webanforderung und dem folgenden REST-URI abrufen. Verwenden Sie die Dashboard-**ID**, die Sie in **Schritt 2 – Abrufen der Dashboards eines Benutzers** in der **Kachel**-URL erhalten hatten (siehe unten). Der Vorgang [Get Tiles](Get-Tiles.md) gibt ein JSON-Array der Kacheln eines Benutzers auf einem Dashboard zurück, das eine ID, einen Titel und eine **embedUrl** enthält. Sie verwenden die **embedUrl**, um die Quell-URL für den **IFrame** festzulegen. Weitere Informationen finden Sie unter **Schritt 4 – Festlegen der IFrame-Quell-URL**. Der Vorgang ist wie folgt:

GET REST URI

     GET https://api.powerbi.com/beta/myorg/{dashboard_id}/tiles

JSON-Antwort

    {
        "@odata.context":"api.powerbi.com/beta/myorg/$metadata#tiles","value":
        [
            {
                "id":"f16dc78a--8970-afe1098a100d",
                "title":"ProductID",
                "embedUrl":"https://api.powerbi.com/embed
                ?dashboardId=43127a01--e971d4cdc2fb
                &tileId=f16dc78a-6897-afe1098a100d"
            },
            {
                "id":"9ab3925d--4d6c896df0c9",
                "title":"Count of CountryRegionCode",
                "embedUrl":"https://api.powerbi.com/embed
                ?dashboardId=43127a01--e971d4cdc2fb
                &tileId=9ab3925d--4d6c896df0c9"
            }
        ]
    }

**Hinweis** Ein umfassendes Beispiel zum Abrufen von Kacheln finden Sie unter **getTilesButton_Click** innerhalb des Beispiels [Integrate a tile into an app](https://github.com/PowerBI/Integrate-a-tile-into-an-app).

**Schritt 4 – Festlegen der IFrame-Quell-URL**

Sobald Sie eine **embedUrl** für eine **Kachel** besitzen, legen Sie das Attribut **SRC** für einen **IFrame** auf die **embedUrl** fest. Der nächste Abschnitt, [Einbetten einer Power BI-Kachel in eine App](#embed), beschreibt das Einbetten einer Kachel mithilfe eines IFrames.

<a name="embed"/>
##Einbetten einer Power BI-Kachel in eine App

Sie betten eine Power BI-**Kachel** mithilfe eines **IFrame**-HTML-Elements in eine App ein. Sie müssen außerdem eine Nachricht **loadTile** vom **IFrame** veröffentlichen, wenn dieses geladen wird, und ein **Authentifizierungstoken** sowie die Höhe und die Breite übergeben. Weitere Informationen finden Sie unter **Schritt 2 – Senden des Authentifizierungstokens**. Ein umfassendes Codebeispiel, aus dem Sie ersehen können, wie eine Power BI-Kachel mithilfe eines IFrames eingebettet wird, erhalten Sie, wenn Sie das [Integrate a tile into an app](https://github.com/PowerBI/Integrate-a-tile-into-an-app) herunterladen.

Dies sind die erforderlichen Schritte zum Einbetten einer **Kachel** mithilfe eines IFrames:

**Schritt 1 – Festlegen einer IFrame-Quelle auf die embedUrl einer Kachel**
Ein IFrame-src-Attribut wird wie folgt festgelegt:
iframe.src =  embedTileUrl + "&width=" + width + "&height=" + height;

Die **embedUrl** weist das folgende Muster und die folgenden URL-Parameter auf:

**embedUrl-Beispiel**

    https://app.powerbi.com/embed?dashboardId=302e6400--26968b3ee1e8&tileId=9bbf6732-f0b1--cb2c40c286fd

**Parameter für die Einbindungs-URL**

<table>
  <tr>
    <td>
      <b>**Name**</b>
    </td>
    <td>
      <b>**Beschreibung**</b>
    </td>
    <td>
      <b>**REST-Vorgang**</b>
    </td>
  </tr>
  <tr>
    <td>dashboardId</td>
    <td>Die ID des **Dashboards** des Benutzers.</td>
    <td>[Get Dashboards](Get--Dashboards.md)</td>
  </tr>
  <tr>
    <td>tileId</td>
    <td>Die ID der einzubettenden **Kachel**. Sie erhalten ein Array der **Kacheln** für eine **Dashboard**-ID.</td>
    <td>[Get Tiles](Get-Tiles.md)</td>
  </tr>
</table>

**Schritt 2 – Senden des Authentifizierungstokens**

Zum Anzeigen einer Power BI-**Kachel** in einem **IFrame** müssen Sie das **Authentifizierungstoken** übergeben, das Sie in **Schritt 1 – Abrufen eines Azure AD-Authentifizierungstokens** erhalten haben. Dazu weisen Sie dem Ereignis „IFrame.onload“ in folgender Weise eine POST-Methode zu:

    iframe.onload = postActionLoadTile;

Die POST-Methode (postActionLoadTile) erstellt zur Authentifizierung beim **Azure Active Directory**-Dienst eine Pushnachrichtenstruktur, die mit einer Methode „contentWindow.postMessage()“ gesendet wird. Bei erfolgreicher Authentifizierung kann der Benutzer die Power BI-**Kachel** anzeigen. Hier sehen Sie ein Beispiel für eine Methode zum Veröffentlichen von Nachrichten:

        // post the auth token to the iFrame. 
        function postActionLoadTile() {
            // get the access token.
            accessToken = document.getElementById('MainContent_accessTokenTextbox').value;
            // return if accessToken is empty
            if ("" === accessToken)
                return;
            var h = height;
            var w = width; 
            // construct the push message structure
            var m = { action: "loadTile", accessToken: accessToken, height: h, width: w};
            message = JSON.stringify(m);
            // push the message.
            iframe = document.getElementById('iFrameEmbedTile');
            iframe.contentWindow.postMessage(message, "*");;
        }

Sobald der Benutzer eine Power BI-Kachel anzeigen kann, kann er auf die Kachel klicken, um zum Power BI-Dashboard zu navigieren, das die Kachel enthält.

<a name="navigate"/>
##Navigieren von einer Power BI-Kachel zum Dashboard

Damit der Benutzer durch Klicken zum Power BI-**Dashboard** navigieren kann, müssen Sie in der Veröffentlichungsnachricht der **Kachel** auf Klicks in das übergeordnete Fenster lauschen und diese entsprechend verarbeiten. Dies sind die dazu erforderlichen Schritte. Ein umfassendes Codebeispiel, das die Navigation von einer Power BI-**Kachel** zum Dashboard veranschaulicht, finden Sie im Beispiel [Integrate a tile into an app](https://github.com/PowerBI/Integrate-a-tile-into-an-app).

**Schritt 1 – Hinzufügen eines Ereignislisteners**

Fügen Sie im Ereignishandler „window.onload“ einen Ereignislistener hinzu (oder an), der die Veröffentlichungsnachricht der **Kachel** bei Klicks in das übergeordnete Fenster verarbeitet.

    window.onload = function () {
    ...
    // listen for message to receive tile click messages.
    if (window.addEventListener) {
        window.addEventListener("message", receiveMessage, false);
        } else {
        window.attachEvent("onmessage", receiveMessage);
        }
    ...
    }

**Schritt 2 – Abrufen der Nachrichtendaten und Öffnen des Dashboardfensters in einer Funktion**

Rufen Sie in einer **receiveMessage(event)**-Funktion die Nachrichtendaten ab, indem Sie den JSON-Code in „event.data“ analysieren (siehe das Beispiel unten). Um zu dem Dashboard zu navigieren, das die Kachel enthält, müssen Sie eine URL erstellen, die die **Dashboard**-ID enthält. Aus der Funktion „receiveMessage“ unten können Sie ersehen, wie Sie eine Dashboard-URL aus der IFrame-Quelle erstellen können. Die **Dashboard**-URL ist wie folgt:

    https://msit.powerbi.com/dashboards/61b7e871--6572c921e271

###Funktion „Sample receiveMessage(event)“

    function receiveMessage(event)
    {
        if (event.data) {
        try {
            messageData = JSON.parse(event.data);
            if (messageData.event === "tileClicked")
            {
                //Get IFrame source and construct dashboard url
                iFrameSrc = document.getElementById(event.srcElement.iframe.id).src;
                //Split IFrame source to get dashboard id
                var dashboardId = iFrameSrc.split("dashboardId=")[1].split("&")[0];
                //Get PowerBI service url
                urlVal = iFrameSrc.split("/embed")[0] + "/dashboards/{0}";
                urlVal = urlVal.replace("{0}", dashboardId);
                window.open(urlVal);
            }
        }
        catch (e) {
            // In a production app, handle exception
        }
    }
    }

##Fazit

Die Schritte in diesem Artikel beschreiben allgemein, wie ein **IFrame** zum Einbetten einer Power BI-**Kachel** in eine App verwendet wird. Das Beispiel [Integrate a tile into an app](https://github.com/PowerBI/Integrate-a-tile-into-an-app) zeigt den gesamten Code, der zum Einbetten einer Power BI-Kachel in einer Web App erforderlich ist. Das Beispiel zeigt außerdem das Hinzufügen eines Klickereignisses für die **Kachel** , damit der Benutzer von der **Kachel** aus zu seinem Power BI **-Dashboard** navigieren kann.

##Siehe auch

-   [Authentifizieren einer Web-App](https://msdn.microsoft.com/en-US/library/mt203565.aspx)
-   [Registrieren einer Web-App](https://msdn.microsoft.com/en-us/library/dn985955.aspx)
-   [Azure Active Directory-Authentifizierungsbibliotheken](https://azure.microsoft.com/en-us/documentation/articles/active-directory-authentication-libraries/)
-   [Erste Schritte beim Erstellen einer Power BI-App](https://msdn.microsoft.com/en-US/library/mt186372.aspx)
-   Power BI-REST-Vorgänge [Get Dashboards](Get-Dashboards.md) und [Get Tiles](Get-Tiles.md)





