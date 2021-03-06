---
title: IoT-Datenströme in Echtzeit mit Azure Stream Analytics
description: IoT-Sensortags und -Datenströme mit Stream Analytics und Echtzeit-Datenverarbeitung
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: f6a1d5e5a15a2af7db5b6256a6a0c5f19f0e7cf5
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2019
ms.locfileid: "67621001"
---
# <a name="get-started-with-azure-stream-analytics-to-process-data-from-iot-devices"></a>Erste Schritte mit Azure Stream Analytics zum Verarbeiten der Daten von IoT-Geräten

In diesem Tutorial erfahren Sie, wie Sie Datenstrom-Verarbeitungslogik erstellen, um Daten von IoT-Geräten (Internet of Things, Internet der Dinge) zu erfassen. Wir verwenden einen echten IoT-Anwendungsfall aus der Praxis, um zu zeigen, wie Sie Ihre Lösung schnell und wirtschaftlich erstellen.

## <a name="prerequisites"></a>Voraussetzungen

* [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/)
* Beispielabfrage und Datendateien (bei [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Szenario

Contoso, ein Unternehmen im Bereich der industriellen Automation, hat seinen Fertigungsprozess vollständig automatisiert. Die Maschinen im Werk verfügen über Sensoren, die Datenströme in Echtzeit ausgeben können. In diesem Szenario möchte ein Production Floor Manager in Echtzeit Einblick in die Sensordaten erhalten, um darin nach Mustern zu suchen und entsprechend darauf zu reagieren. Wir verwenden die Stream Analytics Query Language (SAQL) für die Sensordaten, um für den eingehenden Datenstrom interessante Muster zu ermitteln.

In diesem Fall werden die Daten mit einem SensorTag-Gerät von Texas Instruments generiert. Die Nutzlast der Daten liegt im JSON-Format vor und sieht wie folgt aus:

```json
{
    "time": "2016-01-26T20:47:53.0000000",  
    "dspl": "sensorE",  
    "temp": 123,  
    "hmdt": 34  
}  
```

In der Praxis können Hunderte dieser Sensoren einen Datenstrom mit Ereignissen generieren. Idealerweise führt ein Gatewaygerät hierbei Code aus, um diese Ereignisse mittels Push an [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) oder [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) zu übertragen. Ihr Stream Analytics-Auftrag erfasst diese Ereignisse über Event Hubs und führt in Echtzeit Analyseabfragen für die Datenströme aus. Die Ergebnisse können dann an eine der [unterstützten Ausgaben](stream-analytics-define-outputs.md) gesendet werden.

Zur einfacheren Verwendung enthält dieser Leitfaden zu den ersten Schritten eine Datei mit Beispieldaten, die mit echten SensorTag-Geräten erfasst wurden. Sie können Abfragen für die Beispieldaten ausführen und Ergebnisse anzeigen. In den nachfolgenden Tutorials erfahren Sie, wie Sie Ihren Auftrag mit Eingaben und Ausgaben verbinden und für den Azure-Dienst bereitstellen.

## <a name="create-a-stream-analytics-job"></a>Erstellen eines Stream Analytics-Auftrags
1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf das Pluszeichen, und geben Sie im Textfenster auf der rechten Seite den Text **STREAM ANALYTICS** ein. Wählen Sie anschließend in der Ergebnisliste **Stream Analytics-Auftrag** aus.
   
    ![Neuen Stream Analytics-Auftrag erstellen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)
2. Geben Sie einen eindeutigen Auftragsnamen ein, und überprüfen Sie, ob das richtige Abonnement für Ihren Auftrag angegeben ist. Erstellen Sie dann entweder eine neue Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe Ihres Abonnements aus.
3. Wählen Sie anschließend einen Standort für den Auftrag aus. Um für die Datenübertragung eine hohe Geschwindigkeit und eine Kostenreduzierung zu erzielen, ist es ratsam, den gleichen Standort wie für die Ressourcengruppe und das gewünschte Speicherkonto zu wählen.
   
    ![Erstellen eines neuen Stream Analytics-Auftrags – Details](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)
   
   > [!NOTE]
   > Sie sollten dieses Speicherkonto nur einmal pro Region erstellen. Dieser Speicher wird über alle Stream Analytics-Aufträge hinweg gemeinsam genutzt, die in dieser Region erstellt werden.
   > 
   > 
4. Aktivieren Sie das Kontrollkästchen, um den Auftrag im Dashboard zu platzieren, und klicken Sie dann auf **ERSTELLEN**.
   
    ![Stream Analytics-Auftrag wird gerade erstellt.](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)
5. Oben rechts im Browserfenster sollte „Bereitstellung wurde gestartet...“ angezeigt werden. Nach kurzer Zeit ändert sich dies in ein Fenster mit dem Hinweis, dass der Vorgang abgeschlossen ist (unten dargestellt).
   
    ![Stream Analytics-Bereitstellung war erfolgreich.](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

## <a name="create-an-azure-stream-analytics-query"></a>Erstellen einer Azure Stream Analytics-Abfrage
Nachdem der Auftrag erstellt wurde, können Sie ihn öffnen und eine Abfrage erstellen. Sie können leicht auf den Auftrag zugreifen, indem Sie auf die entsprechende Kachel klicken.

![Kachel für Stream Analytics-Auftrag im Azure-Portal](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Klicken Sie im Bereich **Auftragstopologie** auf das Feld **ABFRAGE**, um zum Abfrage-Editor zu wechseln. Mit dem Editor **ABFRAGE** können Sie eine T-SQL-Abfrage eingeben, die die Transformation der eingehenden Ereignisdaten ausführt.

![Abfragekachel im Stream Analytics-Dashboard](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Abfrage: Archivieren von Rohdaten
Die einfachste Form einer Abfrage ist eine Passthrough-Abfrage, bei der alle Eingabedaten unter dem angegebenen Ausgabeort archiviert werden. Laden Sie die Beispieldatendatei von [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) an einen Speicherort auf Ihrem Computer herunter. 

1. Fügen Sie die Abfrage aus der Datei „PassThrough.txt“ ein. 
   
    ![Einfügen der Abfrage in den Stream Analytics-Abfrage-Editor](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)
2. Klicken Sie auf die drei Punkte neben Ihrer Eingabe, und aktivieren Sie das Kontrollkästchen **Beispieldaten aus Datei hochladen**.
   
    ![Auswahl von „Beispieldaten aus Datei hochladen“](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)
3. Auf der rechten Seite wird ein Bereich geöffnet. Wählen Sie darin am Downloadort die Datendatei „HelloWorldASA-InputStream.json“ aus, und klicken Sie unten auf **OK**.
   
    ![Hochladen einer JSON-Beispieldatendatei](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)
4. Klicken Sie dann oben links im Fenster auf das Zahnrad **Test**, und führen Sie die Verarbeitung der Testabfrage für das Beispieldataset durch. Wenn die Verarbeitung abgeschlossen ist, wird unter Ihrer Abfrage ein Ergebnisfenster geöffnet.
   
    ![Testergebnisse für die Stream Analytics-Abfrage](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-the-data-based-on-a-condition"></a>Abfrage: Filtern der Daten basierend auf einer Bedingung
Wir versuchen nun, die Ergebnisse anhand einer Bedingung zu filtern. Wir möchten Ihnen Ergebnisse nur für die Ereignisse anzeigen, die von "sensorA" kommen. Die Abfrage ist in der Datei „Filtering.txt“ enthalten.

![Filtern eines Datenstroms](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Beachten Sie, dass mit der Abfrage, bei der die Groß-/Kleinschreibung berücksichtigt wird, ein Zeichenfolgenwert verglichen wird. Klicken Sie erneut auf das Zahnrad **Test**, um die Abfrage auszuführen. Mit der Abfrage sollten 389 Zeilen für 1860 Ereignisse zurückgegeben werden.

![Zweite Ausgabeergebnisse des Abfragetests](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-to-trigger-a-business-workflow"></a>Abfrage: Warnung zur Auslösung eines Geschäftsworkflows
Wir fügen unserer Abfrage nun weitere Details hinzu. Für jeden Typ von Sensor möchten wir die Durchschnittstemperatur pro 30-Sekunden-Fenster überwachen und die Ergebnisse nur anzeigen, wenn die Durchschnittstemperatur über 100 Grad liegt. Wir schreiben die folgende Abfrage und klicken dann auf **Test**, um die Ergebnisse anzuzeigen. Die Abfrage befindet sich in der Datei „ThresholdAlerting.txt“.

![Filterabfrage (30 Sekunden)](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Die Ergebnisse umfassen jetzt nur noch 245 Zeilen, und es werden die Namen der Sensoren aufgeführt, bei denen die Durchschnittstemperatur höher als 100 ist. Bei dieser Abfrage wird der Datenstrom mit den Ereignissen nach **dspl** (Sensorname) gruppiert, und es wird ein **rollierendes Fenster** von 30 Sekunden verwendet. Bei zeitlichen Abfragen muss angegeben werden, wie der Zeitablauf erfolgen soll. Mit der **TIMESTAMP BY**-Klausel haben wir die Spalte **OUTPUTTIME** angegeben, um Zeiten allen Zeitberechnungen zuzuordnen. Ausführliche Informationen finden Sie in den MSDN-Artikeln zu den Funktionen [Time Management](https://docs.microsoft.com/stream-analytics-query/time-management-azure-stream-analytics) (Zeitverwaltung) und [Windowing](https://docs.microsoft.com/stream-analytics-query/windowing-azure-stream-analytics).

### <a name="query-detect-absence-of-events"></a>Abfrage: Abwesenheit von Ereignissen erkennen
Wie können wir eine Abfrage schreiben, die einen Mangel an Eingabeereignissen findet? Hierzu ermitteln wir, wann ein Sensor zum letzten Mal Daten gesendet und danach fünf Sekunden lang keine weiteren Ereignisse gesendet hat. Die Abfrage ist in der Datei „AbsenceOfEvent.txt“ enthalten.

![Abwesenheit von Ereignissen erkennen](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Hier verwenden wir den Verknüpfungsvorgang **LEFT OUTER** für den gleichen Datenstrom (Selbstverknüpfung). Für eine innere Verknüpfung (**INNER**) wird nur dann ein Ergebnis zurückgegeben, wenn eine Übereinstimmung gefunden wird.  Wenn ein Ereignis von der linken Seite der Verknüpfung keine Übereinstimmung besitzt, wird bei **LEFT OUTER** für alle Spalten der rechten Seite eine Zeile mit NULL zurückgegeben. Dieses Verfahren ist äußerst hilfreich, um die Abwesenheit von Ereignissen zu ermitteln. Weitere Informationen zu [JOIN](https://docs.microsoft.com/stream-analytics-query/join-azure-stream-analytics) finden Sie in der MSDN-Dokumentation.

## <a name="conclusion"></a>Zusammenfassung
In diesem Tutorial erfahren Sie, wie Sie verschiedene Abfragen in der Stream Analytics-Abfragesprache schreiben und die Ergebnisse im Browser anzeigen. Dies ist aber erst der Anfang. Mit Stream Analytics haben Sie noch viele weitere Möglichkeiten. Stream Analytics unterstützt verschiedenste Ein- und Ausgaben und kann sogar Funktionen in Azure Machine Learning nutzen. Dies macht Stream Analytics zu einem zuverlässigen Tool für die Datenstromanalyse. Weitere Informationen zu Stream Analytics finden Sie in unserem [Lernpfad](https://docs.microsoft.com/azure/stream-analytics/). Weitere Informationen zum Schreiben von Abfragen finden Sie im Artikel zu [gängigen Abfragemustern](stream-analytics-stream-analytics-query-patterns.md).

