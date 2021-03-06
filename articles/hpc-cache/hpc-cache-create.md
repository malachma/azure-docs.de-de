---
title: Erstellen einer Azure HPC Cache-Instanz
description: Hier erfahren Sie, wie Sie eine Azure HPC Cache-Instanz erstellen.
author: ekpgh
ms.service: hpc-cache
ms.topic: tutorial
ms.date: 09/06/2019
ms.author: v-erkell
ms.openlocfilehash: 479adf9419cdd6b04e50fa479d47b56762b2bdc6
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70774623"
---
# <a name="create-an-azure-hpc-cache"></a>Erstellen einer Azure HPC Cache-Instanz

Erstellen Sie Ihren Cache mithilfe des Azure-Portals.

![Screenshot: Cacheübersicht im Azure-Portal mit der Schaltfläche „Erstellen“ im unteren Bereich](media/hpc-cache-home-page.png)

## <a name="define-basic-details"></a>Definieren grundlegender Informationen

![Screenshot: Projektdetailseite im Azure-Portal](media/create-1.png)

Wählen Sie auf der Seite **Projektdetails** das Abonnement und die Ressourcengruppe zum Hosten der Azure HPC Cache-Instanz aus. Achten Sie darauf, dass sich das Abonnement in der Liste für den [Vorschauzugriff](hpc-cache-prereqs.md#azure-subscription) befindet.

Legen Sie unter **Dienstdetails** den Cachenamen sowie folgende weitere Attribute fest:

* Standort: Wählen Sie eine der [unterstützten Regionen](hpc-cache-overview.md#region-availability) aus.
* Virtuelles Netzwerk: Sie können ein bereits vorhandenes virtuelles Netzwerk auswählen oder ein neues erstellen.
* Subnetz: Wählen Sie ein Subnetz mit mindestens 64 IP-Adressen (/24) aus, das ausschließlich für Azure HPC Cache verwendet wird, oder erstellen Sie ein entsprechendes Subnetz.

## <a name="set-cache-capacity"></a>Festlegen der Cachekapazität
<!-- change link in GUI -->

Legen Sie auf der Seite **Cache** die Kapazität Ihrer Azure HPC Cache-Instanz fest. Dieser Wert bestimmt, wie viele Daten der Cache enthalten und wie schnell er Clientanforderungen bewältigen kann. Nach dem Public Preview-Zeitraum hat die Kapazität zudem Auswirkungen auf die Kosten des Caches.

Die Cachekapazität wird in IOPS (Input/Output operations Per Second, Eingabe-/Ausgabevorgängen pro Sekunde) gemessen. Legen Sie zum Auswählen der Kapazität die beiden folgenden Werte fest:

* Die maximale Datenübertragungsrate für den Cache (Durchsatz) in GB/Sekunde
* Die zugeordnete Speichermenge für zwischengespeicherte Daten in TB

Wählen Sie einen der verfügbaren Durchsatzwerte und eine der verfügbaren Cachespeichergrößen aus. Die IOPS-Kapazität wird berechnet und unterhalb der Werteauswahl angezeigt.

Beachten Sie, dass die tatsächliche Datenübertragungsrate von der Arbeitsauslastung, der Netzwerkgeschwindigkeit und der Art der Speicherziele abhängt. Wenn sich eine Datei nicht im Cache befindet oder als veraltet markiert ist, beansprucht der Dienst einen Teil des Durchsatzes, um sie aus dem Back-End-Speicher abzurufen. Der gewählte Wert legt den maximalen Durchsatz für den gesamten Cache fest und steht nicht vollständig für Clientanforderungen zur Verfügung.

Für den Cachespeicher verwaltet Azure HPC Cache, welche Dateien zwischengespeichert und vorab geladen werden, um die Cachetrefferraten zu maximieren. Der Cacheinhalt wird kontinuierlich bewertet, und seltener verwendete Dateien werden in den langfristigen Speicher verschoben. Wählen Sie eine Cachespeichergröße, die problemlos den aktiven Satz von Arbeitsdateien aufnehmen kann und Speicherplatz für Metadaten und andere Zusatzdaten bietet.

![Screenshot: Seite zum Festlegen der Cachegröße](media/create-cache-iops.png)

## <a name="add-storage-targets"></a>Hinzufügen von Speicherzielen

Bei Speicherzielen handelt es sich um den langfristigen Back-End-Speicher für den Inhalt Ihres Caches.

Speicherziele können im Rahmen der Cacheerstellung definiert oder nachträglich im Portal auf der Seite Ihres Caches über den Link im Abschnitt **Konfigurieren** hinzugefügt werden.

![Screenshot: Seite „Speicherziele“](media/create-targets.png)

Klicken Sie auf den Link **Speicherziel hinzufügen**, um Ihre Back-End-Speichersysteme zu definieren. Bei dem Speicher kann es sich um Azure-Blobcontainer oder um lokale NFS-Systeme handeln.

Sie können bis zu zehn verschiedene Speicherziele definieren.

Eine Schritt-für-Schritt-Anleitung zum Hinzufügen eines Speicherziels finden Sie unter [Hinzufügen von Speicher](hpc-cache-add-storage.md). Für Blobspeicher und NFS-Exporte müssen jeweils unterschiedliche Schritte ausgeführt werden.

Bei beiden Speichertypen müssen Sie den Ort des Back-End-Speichersystems (entweder eine NFS-Adresse oder einen Blobcontainernamen) sowie den clientseitigen Namespacepfad angeben.

Achten Sie beim Erstellen eines Blobspeicherziels darauf, dass der Cache über Zugriffsberechtigungen für das Speicherkonto verfügt, wie unter [Hinzufügen der Zugriffssteuerungsrollen zu Ihrem Konto](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) beschrieben. Sollten Sie nicht sicher sein, ob die Rollenkonfiguration erfolgreich sein wird, erstellen Sie zuerst den Cache, und fügen Sie dann den Blobspeicher hinzu.

Geben Sie bei Erstellung eines NFS-Speicherziels ein [Nutzungsmodell](hpc-cache-add-storage.md#choose-a-usage-model) an. Auf der Grundlage der Nutzungsmodelleinstellung kann der Cache Ihren Workflow optimieren.

## <a name="add-resource-tags-optional"></a>Hinzufügen von Ressourcentags (optional)

Auf der Seite **Tags** können Sie Ihrer Azure HPC Cache-Instanz [Ressourcentags](https://go.microsoft.com/fwlink/?linkid=873112) hinzufügen. 

## <a name="finish-creating-the-cache"></a>Fertigstellen der Cacheerstellung

Nachdem Sie den neuen Cache konfiguriert haben, klicken Sie auf die Registerkarte **Überprüfen + erstellen**. Das Portal überprüft Ihre Auswahl und ermöglicht es Ihnen, die gewählten Optionen selbst noch einmal zu überprüfen. Ist alles korrekt, klicken Sie auf **Erstellen**. 

Die Cacheerstellung dauert ungefähr zehn Minuten. Sie können den Status über den Benachrichtigungsbereich des Azure-Portals nachverfolgen. Nach Abschluss des Vorgangs wird eine Benachrichtigung mit einem Link zu der neuen Azure HPC Cache-Instanz angezeigt. 

Der Cache wird außerdem in der Liste **Ressourcen** Ihres Abonnements angezeigt. 

![Screenshot: Azure HPC Cache-Instanz im Azure-Portal](media/finished-hpc-cache.png)

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihr Cache in der Liste **Ressourcen** angezeigt wird, können Sie ihn für den Clientzugriff einbinden, ihn für die Verschiebung Ihrer Arbeitssatzdaten in ein neues Azure-Blobspeicherziel verwenden oder zusätzliche Datenquellen definieren.

* [Einbinden der Azure HPC Cache-Instanz](hpc-cache-mount.md)
* [Verschieben von Daten in Azure Blob Storage für Azure HPC Cache](hpc-cache-ingest.md)
* [Hinzufügen von Speicherzielen](hpc-cache-add-storage.md)
