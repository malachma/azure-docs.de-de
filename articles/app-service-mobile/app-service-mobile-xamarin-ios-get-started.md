---
title: Erste Schritte mit Azure Mobile App Service-Apps für Xamarin.iOS-Apps | Microsoft Docs
description: Befolgen Sie dieses Lernprogramm für die ersten Schritte bei der Verwendung von Mobile Apps für die Xamarin.iOS-Entwicklung.
services: app-service\mobile
documentationcenter: xamarin
author: elamalani
manager: crdun
editor: ''
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: emalani
ms.openlocfilehash: 57867eeca9f29cfc3983cbdca94c830aa7a20500
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67446258"
---
# <a name="create-a-xamarinios-app"></a>Erstellen einer Xamarin.iOS-App
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

> [!NOTE]
> Im Rahmen von Visual Studio App Center wird in neue und integrierte Dienste investiert, die für die Entwicklung mobiler Apps von zentraler Bedeutung sind. Entwickler können **Build**-, **Test**- und **Verteilungs**dienste nutzen, um eine Pipeline für Continuous Integration und Delivery einzurichten. Nach der Bereitstellung der App können Entwickler den Status und die Nutzung ihrer App mithilfe der **Analyse**- und **Diagnose**dienste überwachen und mit Benutzern über den **Push**dienst interagieren. Entwickler können auch den **Authentifizierung**sdienst nutzen, um ihre Benutzer zu authentifizieren, und den **Daten**dienst, um App-Daten dauerhaft in der Cloud zu speichern und zu synchronisieren. Besuchen Sie noch heute das [App Center](https://appcenter.ms/?utm_source=zumo&utm_campaign=app-service-mobile-xamarin-ios-get-started).
>

## <a name="overview"></a>Übersicht
In diesem Lernprogramm erfahren Sie, wie Sie mithilfe eines mobilen Azure-App-Back-Ends einen cloudbasierten Back-End-Dienst einer mobilen Xamarin.iOS-App hinzufügen können.  Sie erstellen sowohl ein neues mobiles App-Back-End als auch eine einfache Xamarin-iOS-App des Typs *Aufgabenliste*, die App-Daten in Azure speichert.

Das Absolvieren dieses Lernprogramms ist Voraussetzung für alle anderen Xamarin.iOS-Lernprogramme zur Verwendung des Features "Mobile Apps" von Azure App Service.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Falls Sie kein Konto haben, können Sie sich für eine Azure-Testversion registrieren. So erhalten Sie bis zu 10 kostenlose mobile Apps, die Sie auch nach Ablauf der Testversion weiter nutzen können. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio für Mac. Siehe [Setup und Installation für Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019).
* Ein Mac Xcode 9.0 oder höher.
  
## <a name="create-an-azure-mobile-app-backend"></a>Erstellen eines Azure Mobile App-Back-Ends
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Erstellen einer Datenbankverbindung und Konfigurieren des Client- und Serverprojekts
[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinios-app"></a>Ausführen der Xamarin.iOS-App
1. Öffnen Sie das Xamarin.iOS-Projekt.

2. Wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu der mobilen App, die Sie erstellt haben. Suchen Sie auf dem Blatt `Overview` nach der URL, die den öffentlichen Endpunkt für Ihre mobile App darstellt. Beispiel: der Sitename für meinen App-Namen „test123“ wird https://test123.azurewebsites.net.

3. Öffnen Sie die Datei `QSTodoService.cs` in diesem Ordner: „xamarin.iOS/ZUMOAPPNAME“. Der Anwendungsname lautet `ZUMOAPPNAME`.

4. Ersetzen Sie in der Klasse `QSTodoService` die Variable `ZUMOAPPURL` durch den oben angegebenen öffentlichen Endpunkt.

    `const string applicationURL = @"ZUMOAPPURL";`

    wird zu
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Drücken Sie die Taste F5, um die App in einem iPhone-Emulator bereitzustellen und auszuführen.

6. Geben Sie in der App einen sinnvollen Text wie z. B. *Tutorial fertigstellen* ein, und klicken Sie dann auf die Schaltfläche „+“.

    ![][10]

    Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden von dem mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

   > [!NOTE]
   > Sie können den Code überprüfen, der zum Abfragen und Einfügen von Daten auf das mobile App-Back-End zugreift. Der Code befindet sich in der C#-Datei "ToDoActivity.cs".
   
<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png