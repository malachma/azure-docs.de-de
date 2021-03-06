---
title: Kennenlernen der Benutzeroberfläche von Azure IoT Central | Microsoft-Dokumentation
description: Machen Sie sich als Ersteller mit den wichtigsten Bereichen der Benutzeroberfläche von Azure IoT Central vertraut, die Sie zur Erstellung von IoT-Lösungen verwenden.
author: dominicbetts
ms.author: dobett
ms.date: 07/05/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: 82996db232fde0424ccc8e3e478a70a5892231e6
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70884829"
---
# <a name="take-a-tour-of-the-azure-iot-central-ui-preview-features"></a>Kennenlernen der Benutzeroberfläche von Azure IoT Central (Previewfunktionen)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

In diesem Artikel wird die Benutzeroberfläche von Microsoft Azure IoT Central vorgestellt. Über die Benutzeroberfläche können Sie Azure IoT Central-Lösungen und die damit verbundenen Geräte erstellen, verwalten und verwenden.

_Ersteller_ verwenden die Benutzeroberfläche von Azure IoT Central, um ihre Azure IoT Central-Lösung zu definieren. Die Benutzeroberfläche bietet folgende Möglichkeiten:

* Definieren der Art von Gerät, das eine Verbindung mit Ihrer Lösung herstellt
* Konfigurieren der Regeln und Aktionen für Ihre Geräte
* Anpassen der Benutzeroberfläche für einen _Bediener_, der Ihre Lösung verwendet

_Bediener_ verwenden die Benutzeroberfläche von Azure IoT Central, um ihre Azure IoT Central-Lösung zu verwalten. Die Benutzeroberfläche bietet folgende Möglichkeiten:

* Überwachen Ihrer Geräte
* Konfigurieren Ihrer Geräte
* Behandeln und Beheben von Problemen mit Ihren Geräten
* Bereitstellen von neuen Geräten

## <a name="use-the-left-navigation-menu"></a>Verwenden des linken Navigationsmenüs

Über das linke Navigationsmenü können Sie auf die verschiedenen Bereiche der Anwendung zugreifen. Wählen Sie **<** oder **>** aus, um die Navigationsleiste zu erweitern oder zu reduzieren:

:::row:::
  :::column span="":::
      ![Left navigation menu](media/overview-iot-central-tour-pnp/navigationbar.png)
  :::column-end:::
  :::column span="2":::
     Unter **Dashboard** wird Ihr Anwendungsdashboard angezeigt. Als Ersteller können Sie das Dashboard für Ihre Bediener anpassen. Benutzer können auch ihre eigenen Dashboards erstellen.
     
     **Geräte** dient zum Auflisten der simulierten und echten Geräte, die den einzelnen Gerätevorlagen in der Anwendung zugeordnet sind. Bediener verwenden den **Device Explorer**, um ihre verbundenen Geräte zu verwalten.

     Unter **Gerätegruppen** können Sie Gerätegruppen anzeigen und erstellen. Bediener können Gerätegruppen als logische, durch eine Abfrage angegebene Sammlung von Geräten erstellen.

     Unter **Regeln** können Sie Regeln bearbeiten, die basierend auf Telemetriedaten ausgelöst werden und anpassbare Aktionen auslösen.

     **Analytics** dient zum Anzeigen von Analysen, die auf Gerätetelemetriedaten für Geräte und Gerätegruppen basieren. Bediener können benutzerdefinierte Ansichten auf der Grundlage von Gerätedaten erstellen, um basierend auf ihrer Anwendung Erkenntnisse zu gewinnen.

     **Aufträge** ermöglicht die Massenverwaltung von Geräten, da Sie Aufträge für die Geräteaktualisierung in großem Umfang erstellen und ausführen können.

     Unter **Gerätevorlagen** werden die Tools angezeigt, die ein Ersteller zum Erstellen und Verwalten von Gerätevorlagen verwendet.

     Unter **Datenexport** kann ein Administrator einen fortlaufenden Export an andere Azure-Dienste (etwa Speicher und Warteschlangen) konfigurieren.

     **Verwaltung** dient zum Anzeigen der Seiten für die Anwendungsverwaltung, auf denen ein Administrator Anwendungseinstellungen, Benutzer und Rollen verwalten kann.
   :::column-end:::
:::row-end:::

## <a name="search-help-and-support"></a>Suche, Hilfe und Support

Das obere Menü wird auf jeder Seite angezeigt:

![Symbolleiste](media/overview-iot-central-tour-pnp/toolbar.png)

* Wenn Sie nach Gerätevorlagen und Geräten suchen möchten, geben Sie einen Wert für die **Suche** ein.
* Wählen Sie zum Ändern der Sprache der Benutzeroberfläche oder des Designs das Symbol **Einstellungen** aus.
* Wählen Sie das Symbol **Konto** aus, um sich von der Anwendung abzumelden.
* Wenn Sie Hilfe oder Unterstützung benötigen, klicken Sie auf das Dropdownmenü **Hilfe**, um eine Liste mit Ressourcen anzuzeigen. In einer Testanwendung beinhalten die Supportressourcen Zugriff auf den [Livechat](howto-show-hide-chat.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

Sie können zwischen einem hellen und einem dunklen Benutzeroberflächendesign wählen:

![Auswählen eines Designs für die Benutzeroberfläche](media/overview-iot-central-tour-pnp/themes.png)

> [!NOTE]
> Die Option zum Wählen zwischen hellen und dunklen Designs ist nicht verfügbar, wenn Ihr Administrator ein benutzerdefiniertes Design für die Anwendung konfiguriert hat.

## <a name="dashboard"></a>Dashboard

![Dashboard](media/overview-iot-central-tour-pnp/homepage.png)

* Nach der Anmeldung bei Ihrer Azure IoT Central-Anwendung wird als Erstes das Dashboard angezeigt. Als Ersteller können Sie das Anwendungsdashboard für andere Benutzer anpassen, indem Sie Kacheln hinzufügen. Weitere Informationen finden Sie im Tutorial zum [Einrichten einer Gerätevorlage](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

* Als Bediener können Sie personalisierte Dashboards erstellen und zwischen diesen und dem Standarddashboard wechseln. Weitere Informationen finden Sie in der Anleitung [Erstellen und Verwalten persönlicher Dashboards](howto-personalize-dashboard.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="devices"></a>Geräte

![Seite "Geräte"](media/overview-iot-central-tour-pnp/explorer.png)

Auf der Explorer-Seite werden die _Geräte_ in Ihrer Azure IoT Central-Anwendung angezeigt (gruppiert nach _Gerätevorlage_).

* Eine Gerätevorlage definiert einen Gerätetyp, der eine Verbindung mit Ihrer Anwendung herstellen kann. Weitere Informationen finden Sie unter [Define a new device type in your Azure IoT Central application](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung).
* Bei einem Gerät handelt es sich um ein echtes oder simuliertes Gerät in Ihrer Anwendung. Weitere Informationen finden Sie unter [Add a real device to your Azure IoT Central application](tutorial-add-device-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Hinzufügen eines echten Geräts zu Ihrer Azure IoT Central-Anwendung).

## <a name="device-groups"></a>Gerätegruppen

![Seite „Gerätegruppen“](media/overview-iot-central-tour-pnp/devicesets.png)

Auf der Seite _Gerätegruppen_ werden Gerätegruppen angezeigt, die der Ersteller erstellt hat. Bei einer Gerätegruppe handelt es sich um eine Sammlung verwandter Geräte. Ein Ersteller definiert eine Abfrage, um die in einer Gerätegruppe enthaltenen Geräte zu identifizieren. Gerätegruppen werden verwendet, wenn Sie die Analyse in Ihrer Anwendung anpassen. Weitere Informationen finden Sie im Artikel [Verwenden von Gerätegruppen in Ihrer Azure IoT Central-Anwendung](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="rules"></a>Regeln

![Seite „Regeln“](media/overview-iot-central-tour-pnp/rules.png)

Auf der Seite „Regeln“ können Sie Regeln auf der Grundlage von Telemetriedaten, Gerätestatus oder Geräteereignissen definieren. Wenn eine Regel ausgelöst wird, kann sie eine Aktion auslösen, etwa das Senden einer E-Mail an einen Bediener. Der Ersteller verwendet diese Seite zum Erstellen und Verwalten von Regeln. Weitere Informationen finden Sie unter [Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central](tutorial-configure-rules-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="analytics"></a>Analytics

![Seite „Analytics“](media/overview-iot-central-tour-pnp/analytics.png)

Anhand der Diagramme auf der Analyseseite können Sie das Verhalten der mit Ihrer Anwendung verbundenen Geräte nachvollziehen. Bediener verwenden diese Seite zur Überwachung und Untersuchung von Problemen mit verbundenen Geräten. Der Ersteller kann definieren, welche Diagramme auf dieser Seite angezeigt werden. Weitere Informationen finden Sie im Artikel [How to use analytics to analyze your device data](howto-use-device-groups-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Analysieren Ihrer Anwendungsdaten mithilfe von Analysen).

## <a name="jobs"></a>Aufträge

![Seite „Aufträge“](media/overview-iot-central-tour-pnp/jobs.png)

Auf der Seite „Aufträge“ können Sie Massenvorgänge für die Geräteverwaltung auf Ihren Geräten ausführen. Ersteller verwenden diese Seite, um Eigenschaften, Einstellungen und Befehle für Geräte zu aktualisieren. Weitere Informationen finden Sie im Artikel [Ausführen eines Auftrags](howto-run-a-job.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="device-templates"></a>Gerätevorlagen

![Seite „Gerätevorlagen“](media/overview-iot-central-tour-pnp/templates.png)

Auf der Seite „Gerätevorlagen“ können Ersteller die Gerätevorlagen in der Anwendung erstellen und verwalten. In einer Gerätevorlage werden spezielle Geräteeigenschaften angegeben, z.B. die folgenden:

* Telemetriedaten, Status und Ereignismessungen.
* Eigenschaften
* Befehle.

Der Ersteller kann auch Formulare und Dashboards für Bediener erstellen, die zum Verwalten von Geräten verwendet werden.

Weitere Informationen finden Sie im Tutorial [Define a new device type in your Azure IoT Central application](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung).

## <a name="data-export"></a>Datenexport

![Seite „Datenexport“](media/overview-iot-central-tour-pnp/export.png)

Auf der Seite „Datenexport“ können Administratoren definieren, wie Daten (beispielsweise Telemetriedaten) aus der Anwendung gestreamt werden sollen. Die exportierten Daten können von anderen Diensten gespeichert oder zu Analysezwecken genutzt werden. Weitere Informationen finden Sie im Artikel [Exportieren von Daten in Azure IoT Central](howto-export-data.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

## <a name="administration"></a>Verwaltung

![Verwaltungsseite](media/overview-iot-central-tour-pnp/administration.png)

Die Verwaltungsseite enthält Links zu den Tools, die ein Administrator verwendet, um beispielsweise Benutzer und Rollen in der Anwendung zu definieren oder die Benutzeroberfläche anzupassen. Weitere Informationen finden Sie im Artikel [How to administer your application](howto-administer-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Verwalten Ihrer Anwendung).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie sich nun einen Überblick über Azure IoT Central verschafft und sich mit dem Layout der Benutzeroberfläche vertraut gemacht haben, empfehlen wir, mit der Schnellstartanleitung zum [Erstellen einer Azure IoT Central-Anwendung](quick-deploy-iot-central-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) fortzufahren.
