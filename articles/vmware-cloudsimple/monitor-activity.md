---
title: 'Azure-VMware-Lösung von CloudSimple: Überwachen der Aktivität in der privaten Cloud'
description: Beschreibt die Informationen, die zur Aktivität in der Azure-VMware-Lösung durch die CloudSimple-Umgebung verfügbar sind, einschließlich Warnungen, Ereignissen, Aufgaben und Überwachung.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/13/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: ddb3741c987e839fafb8bc222231547988d72f01
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543764"
---
# <a name="monitor-vmware-solution-by-cloudsimple-activity"></a>Überwachen der VMware-Lösung durch CloudSimple-Aktivität

CloudSimple-Aktivitätsprotokolle bieten Einblicke in Vorgänge, die im CloudSimple-Portal durchgeführt werden.  Die Liste enthält Warnungen, Ereignisse, Aufgaben und Überwachung.  Verwenden Sie die Aktivitätsprotokolle, um zu bestimmen, wann welche Vorgänge ausgeführt wurden, und wer sie ausgeführt hat.  Aktivitätsprotokolle enthalten keine Lesevorgänge, die von einem Benutzer durchgeführt werden.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="access-the-cloudsimple-portal"></a>Zugriff auf das CloudSimple-Portal

Rufen Sie das [CloudSimple-Portal](access-cloudsimple-portal.md) auf.

## <a name="activity-information"></a>Informationen zur Aktivität

Um auf die Aktivitätsseiten zuzugreifen, wählen Sie im seitlichen Menü **Aktivität** aus.

![Übersicht zur Aktivitätsseite](media/activity-page-overview.png)

Um Details zu Aktivitäten auf der Aktivitätsseite anzuzeigen, wählen Sie die Aktivität aus. Ein Detailbereich wird auf der rechten Seite geöffnet. Die Aktionen im Bereich sind vom Typ der Aktivität abhängig. Klicken Sie auf **X**, um den Bereich zu schließen.

Klicken Sie auf eine Spaltenüberschrift, um die Anzeige zu sortieren.  Sie können Spalten nach bestimmten Werten filtern, die Sie anzeigen möchten.  Laden Sie den Aktivitätsbericht durch Klicken auf das Symbol **Als CSV herunterladen** herunter.

## <a name="alerts"></a>Alerts

Bei Warnungen handelt es sich um Benachrichtigungen über wichtige Aktivitäten in der CloudSimple-Umgebung.  Warnungen enthalten Ereignisse, die sich auf die Abrechnung oder den Benutzerzugriff auswirken.

Wenn Sie Warnungen bestätigen und aus der Liste entfernen möchten, wählen Sie mindestens eine in der Liste aus, und klicken Sie auf **Bestätigen**.

Die folgenden Spalten mit Informationen sind für Warnungen verfügbar. Klicken Sie auf **Spalten bearbeiten**, und wählen Sie die Spalten aus, die Sie anzeigen möchten.

| Column | BESCHREIBUNG |
------------ | ------------- |
| Warnungstyp | Kategorie der Warnung.|
| Time | Zeitpunkt, zu dem die Warnung auftrat. |
| severity | Bedeutung der Warnung.|
| Ressourcenname | Name, der der Ressource zugewiesen ist, z. B. der Name der privaten Cloud. |
| Ressourcentyp | Kategorie der Ressource: Private Cloud, Cloud-Rack. |
| Ressourcen-ID | Bezeichner der Ressource. |
| BESCHREIBUNG | Beschreibung des Auslösers der Warnung. |
| Bestätigt | Angabe, ob die Warnung bestätigt wurde. |

## <a name="events"></a>Events

Mit Ereignissen werden Benutzer- und Systemaktivitäten im CloudSimple-Portal angezeigt. Auf der Seite „Ereignisse“ werden die mit einer bestimmten Ressource verknüpften Aktivitäten sowie der Schweregrad der Auswirkungen angezeigt.

Die folgenden Spalten mit Informationen sind für Warnungen verfügbar. Klicken Sie auf **Spalten bearbeiten**, und wählen Sie die Spalten aus, die Sie anzeigen möchten.

| Column | BESCHREIBUNG |
------------ | ------------- |
| Time | Angabe des Zeitpunkts, zu dem das Ereignis aufgetreten ist, mit Datum und Uhrzeit. |
| Ereignistyp | Numerischer Code, der das Ereignis identifiziert. |
| severity | Ereignisschweregrad.|
| Ressourcenname | Name, der der Ressource zugewiesen ist, z. B. der Name der privaten Cloud. |
| Ressourcentyp | Kategorie der Ressource: Private Cloud, Cloud-Rack. |
| BESCHREIBUNG | Beschreibung des Auslösers der Warnung. |

## <a name="tasks"></a>Aufgaben

Bei Aufgaben handelt es sich um Aktivitäten in einer privaten Cloud, die voraussichtlich 30 Sekunden oder länger dauern. (Aktivitäten, die voraussichtlich weniger als 30 Sekunden dauern, werden nur als Ereignisse gemeldet.) Öffnen Sie die Seite „Aufgaben“, um den Fortschritt von Aufgaben für die private Cloud zu verfolgen.

Die folgenden Spalten mit Informationen sind für Warnungen verfügbar. Klicken Sie auf **Spalten bearbeiten**, und wählen Sie die Spalten aus, die Sie anzeigen möchten.

| Column | BESCHREIBUNG |
------------ | ------------- |
| ID der Aufgabe | Eindeutiger Bezeichner für die Aufgabe. |
| Vorgang | Aktion, die die Aufgabe ausführt. |
| Benutzer | Der Benutzer, dem die Durchführung der Aufgabe zugewiesen ist. |
| Ressourcenname | Name, der der Ressource zugewiesen ist. |
| Ressourcentyp | Kategorie der Ressource: Private Cloud, Cloud-Rack. |
| Ressourcen-ID | Bezeichner der Ressource. |
| Start | Startzeit für die Aufgabe. |
| End | Endzeit für die Aufgabe. |
| Status | Aktueller Aufgabenstatus. |
| Verstrichene Zeit | Zeitspanne, die die Durchführung der Aufgabe (sofern durchgeführt) in Anspruch nahm bzw. (bei laufender Durchführung) bisher in Anspruch genommen hat. |
| BESCHREIBUNG | Aufgabenbeschreibung. |

## <a name="audit"></a>Audit

Überwachungsprotokolle verfolgen die Benutzeraktivität nach. Überwachungsprotokolle können Sie zum Überwachen von Benutzeraktivitäten für alle Benutzer verwenden.

Die folgenden Spalten mit Informationen sind für Warnungen verfügbar. Klicken Sie auf **Spalten bearbeiten**, und wählen Sie die Spalten aus, die Sie anzeigen möchten.

| Column | BESCHREIBUNG |
------------ | ------------- |
| Time | Zeitpunkt des Überwachungseintrags. |
| Vorgang | Aktion, die die Aufgabe ausführt. |
| Benutzer | Benutzer, der der Aufgabe zugewiesen ist. |
| Ressourcenname | Name, der der Ressource zugewiesen ist. |
| Ressourcentyp | Kategorie der Ressource: Private Cloud, Cloud-Rack. |
| Ressourcen-ID | Bezeichner der Ressource. |
| Ergebnis | Ergebnis der Aktivität, z. B. **Erfolg**. |
| Benötigte Zeit | Zeitspanne zum Durchführen dieser Aufgabe. |
| BESCHREIBUNG | Beschreibung der Aktion. |

## <a name="next-steps"></a>Nächste Schritte

* [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md)
* Weitere Informationen zu [privaten Clouds](cloudsimple-private-cloud.md)
