---
title: Hinzufügen von Authentifizierung zu Apache Cordova mit Mobile Apps| Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Mobile Apps in Azure App Service verwenden, um die Benutzer Ihrer Apache Cordova-App über verschiedene Identitätsanbieter, einschließlich Google, Facebook, Twitter und Microsoft, zu authentifizieren.
services: app-service\mobile
documentationcenter: javascript
author: elamalani
manager: crdun
editor: ''
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: b0634038dbf5771ac1aa0bc00d007e758171b238
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443507"
---
# <a name="add-authentication-to-your-apache-cordova-app"></a>Hinzufügen von Authentifizierung zu Ihrer Apache Cordova-App
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

> [!NOTE]
> Im Rahmen von Visual Studio App Center wird in neue und integrierte Dienste investiert, die für die Entwicklung mobiler Anwendungen von zentraler Bedeutung sind. Entwickler können **Build**-, **Test**- und **Verteilungs**dienste nutzen, um eine Pipeline für Continuous Integration und Delivery einzurichten. Nach der Bereitstellung der App können Entwickler den Status und die Nutzung ihrer App mithilfe der **Analyse**- und **Diagnose**dienste überwachen und mit Benutzern über den **Push**dienst interagieren. Entwickler können auch den **Authentifizierung**sdienst nutzen, um ihre Benutzer zu authentifizieren, und den **Daten**dienst, um App-Daten dauerhaft in der Cloud zu speichern und zu synchronisieren. Besuchen Sie noch heute das [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-cordova-get-started-users).
>

## <a name="summary"></a>Zusammenfassung
In diesem Tutorial fügen Sie dem Aufgabenlisten-Schnellstartprojekt unter Apache Cordova mithilfe eines unterstützten Identitätsanbieters eine Authentifizierung hinzu. Dieses Tutorial baut auf dem Tutorial [Erste Schritte mit Mobile Apps] auf, das Sie zuerst abschließen müssen.

## <a name="register"></a>Registrieren Ihrer App für die Authentifizierung und Konfigurieren von App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Ansehen eines Videos mit ähnlichen Schritten](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="permissions"></a>Einschränken von Berechtigungen für authentifizierte Benutzer
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nun können Sie überprüfen, ob der anonyme Zugriff auf Ihr Back-End deaktiviert wurde. In Visual Studio:

* Öffnen Sie das Projekt, das Sie im Tutorial [Erste Schritte mit Mobile Apps] erstellt haben.
* Führen Sie Ihre Anwendung im **Google Android-Emulator** aus.
* Vergewissern Sie sich, dass nach dem Start der App ein unerwarteter Verbindungsfehler angezeigt wird.

Aktualisieren Sie nun die App, um Benutzer vor dem Anfordern von Ressourcen des Mobile App-Back-Ends zu authentifizieren.

## <a name="add-authentication"></a>Hinzufügen von Authentifizierung zur App
1. Öffnen Sie Ihr Projekt in **Visual Studio**, und öffnen Sie dann die Datei `www/index.html` zur Bearbeitung.
2. Suchen Sie das `Content-Security-Policy` -Metatag im Kopfzeilenbereich.  Fügen Sie den OAuth-Host der Liste mit den zulässigen Quellen hinzu.

   | Anbieter | Name des SDK-Anbieters | OAuth-Host |
   |:--- |:--- |:--- |
   | Azure Active Directory | aad | https://login.microsoftonline.com |
   | Facebook | Facebook | https://www.facebook.com |
   | Google | Google | https://accounts.google.com |
   | Microsoft | microsoftaccount | https://login.live.com |
   | Twitter | Twitter | https://api.twitter.com |

    Hier ein Beispiel für Content-Security-Policy (implementiert für Azure Active Directory):

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    Ersetzen Sie `https://login.microsoftonline.com` durch den OAuth-Host aus der obigen Tabelle.  Weitere Informationen zum Content-Security-Policy-Metatag finden Sie in der [Whitelist Guide].

    Bei Verwendung auf geeigneten Mobilgeräten ist bei einigen Authentifizierungsanbietern keine Änderung von Content-Security-Policy erforderlich.  Beispielsweise sind bei Verwendung der Google-Authentifizierung auf einem Android-Gerät keine Änderungen an Content-Security-Policy notwendig.

3. Öffnen Sie die Datei `www/js/index.js` zur Bearbeitung, suchen Sie die `onDeviceReady()`-Methode, und fügen Sie unterhalb des Codes zur Clienterstellung Folgendes ein:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Dieser Code ersetzt den vorhandenen Code, der die Tabellenreferenz erstellt und die Benutzeroberfläche aktualisiert.

    Die Login()-Methode startet die Authentifizierung mit dem Anbieter. Die login()-Methode ist eine asynchrone Funktion, die eine JavaScript-Zusage zurückgibt.  Der Rest der Initialisierung wird in der Zusagenantwort platziert, sodass sie erst ausgeführt wird, wenn die login()-Methode abgeschlossen ist.

4. Ersetzen Sie in dem gerade hinzugefügten Code `SDK_Provider_Name` durch den Namen des Login-Anbieters. Verwenden Sie für Azure Active Directory beispielsweise `client.login('aad')`.
5. Führen Sie das Projekt aus.  Wenn die Initialisierung des Projekts abgeschlossen ist, zeigt Ihre Anwendung die OAuth-Anmeldeseite für den ausgewählten Authentifizierungsanbieter an.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über die [Authentifizierung] mit Azure App Service.
* Führen Sie das Tutorial fort, indem Sie Ihrer Apache Cordova-App [Pushbenachrichtigungen] hinzufügen.

Erfahren Sie, wie Sie die SDKs nutzen,

* [Apache Cordova-SDK]
* [ASP.NET Server-SDK]
* [Node.js Server-SDK]

<!-- URLs. -->
[Erste Schritte mit Mobile Apps]: app-service-mobile-cordova-get-started.md
[Whitelist Guide]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Pushbenachrichtigungen]: app-service-mobile-cordova-get-started-push.md
[Authentifizierung]: app-service-mobile-auth.md
[Apache Cordova-SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server-SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
