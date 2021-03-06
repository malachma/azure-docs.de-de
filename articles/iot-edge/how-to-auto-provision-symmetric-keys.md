---
title: Automatische Bereitstellung von Geräten mit DPS mithilfe des Nachweises symmetrischer Schlüssel – Azure IoT Edge | Microsoft-Dokumentation
description: Verwenden des Nachweises symmetrischer Schlüssel zum Testen der automatischen Gerätebereitstellung für Azure IoT Edge mithilfe des Device Provisioning-Diensts
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: mrohera
ms.date: 07/10/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 3c21c0bdce6f6a5cd3c8f634bf400600b30a8ead
ms.sourcegitcommit: c556477e031f8f82022a8638ca2aec32e79f6fd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2019
ms.locfileid: "68414589"
---
# <a name="create-and-provision-an-iot-edge-device-using-symmetric-key-attestation"></a>Erstellen und Bereitstellen eines IoT Edge-Geräts mithilfe des Nachweises symmetrischer Schlüssel

Azure IoT Edge-Geräte können genau wie nicht Edge-fähige Geräte mit dem [Gerätebereitstellungsdienst](../iot-dps/index.yml) automatisch bereitgestellt werden. Wenn Sie mit der automatischen Bereitstellung nicht vertraut sind, lesen Sie die Informationen unter [Konzepte für die automatische Bereitstellung](../iot-dps/concepts-auto-provisioning.md), bevor Sie fortfahren.

In diesem Artikel erfahren Sie, wie Sie mit den folgenden Schritten auf einem IoT Edge-Gerät mithilfe des Nachweises symmetrischer Schlüssel eine individuelle Registrierung beim Device Provisioning-Dienst erstellen:

* Erstellen Sie eine neue Instanz für den IoT Hub Device Provisioning-Dienst (DPS).
* Erstellen Sie eine individuelle Registrierung für das Gerät.
* Installieren Sie die IoT Edge-Runtime, und verbinden Sie das Gerät mit dem IoT Hub.

Der Nachweis des symmetrischen Schlüssels ist eine einfache Methode zum Authentifizieren eines Geräts mit einer Device Provisioning Service-Instanz. Diese Nachweismethode stellt eine „Hallo Welt“-Umgebung für Entwickler bereit, die noch nicht mit der Gerätebereitstellung vertraut sind oder keine strengen Sicherheitsanforderungen haben. Die Gerätebestätigung bzw. der Nachweis mithilfe eines [TPM](../iot-dps/concepts-tpm-attestation.md) (Trusted Platform Module) ist sicherer und sollte verwendet werden, wenn striktere Sicherheitsanforderungen gelten.

## <a name="prerequisites"></a>Voraussetzungen

* Aktiver IoT Hub
* Physisches oder virtuelles Gerät

## <a name="set-up-the-iot-hub-device-provisioning-service"></a>Einrichten des IoT Hub Device Provisioning Service

Erstellen Sie eine neue Instanz des IoT Hub Device Provisioning Service in Azure, und verknüpfen Sie sie mit Ihrem IoT-Hub. Befolgen Sie die Anweisungen unter [Einrichten des IoT Hub Device Provisioning Service](../iot-dps/quick-setup-auto-provision.md).

Nachdem Sie den Device Provisioning-Dienst ausgeführt haben, kopieren Sie den Wert von **ID-Bereich** von der Seite „Übersicht“. Sie können diesen Wert verwenden, wenn Sie IoT Edge-Runtime konfigurieren.

## <a name="choose-a-unique-registration-id-for-the-device"></a>Auswählen einer eindeutigen Registrierungs-ID für das Gerät

Eine eindeutige Registrierungs-ID muss definiert werden, um jedes Gerät zu identifizieren. Sie können die MAC-Adresse, Seriennummer oder eindeutige Informationen vom Gerät verwenden.

In diesem Beispiel wird eine Kombination aus MAC-Adresse und Seriennummer genutzt, um die folgende Zeichenfolge für eine Registrierungs-ID zu bilden.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Erstellen Sie eine eindeutige Registrierungs-ID für Ihr Gerät. Gültige Zeichen sind alphanumerische Kleinbuchstaben und Bindestriche („-“).

## <a name="create-a-dps-enrollment"></a>Erstellen einer DPS-Registrierung

Verwenden Sie die Registrierungs-ID Ihres Geräts, um eine individuelle Registrierung im DPS zu erstellen.

Wenn Sie eine Registrierung im DPS erstellen, haben Sie die Möglichkeit zum Angeben von **Anfänglicher Status von Gerätezwilling**. Im Gerätezwilling können Sie Tags zum Gruppieren von Geräten nach jeder beliebigen Metrik, z.B. Region, Umgebung, Speicherort oder Geräte, festlegen, die Sie in Ihrer Projektmappe benötigen. Diese Tags werden zum Erstellen von [automatischen Bereitstellungen](how-to-deploy-monitor.md) verwendet.

> [!TIP]
> Gruppenregistrierungen sind ebenfalls möglich, wenn Sie einen Nachweis symmetrischer Schlüssel verwenden und sie dieselben Entscheidungen wie die individuellen Registrierungen beinhalten.

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu Ihrer Instanz des IoT Hub Device Provisioning Service.

1. Klicken Sie in **Einstellungen** auf **Registrierungen verwalten**.

1. Klicken Sie auf **Individuelle Registrierung hinzufügen**, und führen Sie dann die folgenden Schritte aus, um die Registrierung zu konfigurieren:  

   1. Wählen Sie unter **Mechanismus** die Option **Symmetrischer Schlüssel** aus.

   1. Aktivieren Sie das Kontrollkästchen **Schlüssel automatisch generieren**.

   1. Geben Sie die **Registrierungs-ID** an, die Sie für Ihr Gerät erstellt haben.

   1. Stellen Sie nach Wunsch eine **IoT Hub-Geräte-ID** für Ihr Gerät bereit. Sie können mithilfe von Geräte-IDs ein einzelnes Gerät als Ziel für die Modulbereitstellung festlegen. Wenn Sie keine Geräte-ID angeben, wird die Registrierungs-ID verwendet.

   1. Wählen Sie **True** aus, um anzugeben, dass die Registrierung für ein IoT Edge-Gerät erfolgt. Bei einer Gruppenregistrierung müssen alle Geräte IoT Edge-Geräte sein, oder keines von ihnen darf ein IoT Edge-Gerät sein.

   1. Übernehmen Sie für **Wählen Sie, wie Geräte den Hubs zugewiesen werden sollen** den Standardwert aus der Zuordnungsrichtlinie des Device Provisioning-Diensts, oder wählen Sie einen anderen speziellen Wert für diese Registrierung.

   1. Wählen Sie den verknüpften **IoT-Hub**, mit dem Sie Ihr Gerät verbinden möchten. Sie können mehrere Hubs auswählen, und das Gerät wird dann je nach gewählter Zuteilungsrichtlinie einem dieser Hubs zugewiesen.

   1. Wählen Sie **Verarbeitung von Daten bei der erneuten Bereitstellung auswählen**, um anzugeben, wie Gerätedaten bei einer erneuten Bereitstellung behandelt werden sollen.

   1. Fügen Sie nach Wunsche einen Tagwert zu **Anfänglicher Status von Gerätezwilling** hinzu. Sie können mithilfe von Tags Gruppen von Geräten als Ziel für die Modulbereitstellung festlegen. Beispiel:

      ```json
      {
         "tags": {
            "environment": "test"
         },
         "properties": {
            "desired": {}
         }
      }
      ```

   1. Stellen Sie sicher, dass **Wiederholung aktivieren** auf **Aktivieren** festgelegt ist.

   1. Wählen Sie **Speichern** aus.

Nachdem nun eine Registrierung für dieses Gerät vorhanden ist, kann die IoT Edge-Runtime das Gerät während der Installation automatisch bereitstellen. Kopieren Sie den Wert für den **Primärschlüssel**  Ihrer Registrierung, der beim Erstellen des Geräteschlüssels verwendet werden soll.

## <a name="derive-a-device-key"></a>Ableiten eines Geräteschlüssels

Ihr Gerät verwendet den abgeleiteten Geräteschlüssel mit der eindeutigen Registrierungs-ID, um während der Bereitstellung den Nachweis symmetrischer Schlüssel mit der Registrierung durchzuführen. Verwenden Sie für die Generierung des Geräteschlüssels den Schlüssel, den Sie aus der DPS-Registrierung kopiert haben, um einen [HMAC-SHA256](https://wikipedia.org/wiki/HMAC)-Wert für die eindeutige Registrierungs-ID des Geräts zu berechnen und das Ergebnis in das Base64-Format zu konvertieren.

Nehmen Sie den Primär- oder Sekundärschlüssel Ihrer Registrierung nicht in den Gerätecode auf.

### <a name="linux-workstations"></a>Linux-Arbeitsstationen

Wenn Sie eine Linux-Arbeitsstation verwenden, können Sie Ihren abgeleiteten Geräteschlüssel mit OpenSSL generieren, wie im folgenden Beispiel gezeigt.

Ersetzen Sie den Wert von **KEY** durch den **Primärschlüssel**, den Sie zuvor notiert haben.

Ersetzen Sie den Wert von **REG_ID** durch die Registrierungs-ID Ihres Geräts.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

### <a name="windows-based-workstations"></a>Windows-Arbeitsstationen

Wenn Sie eine Windows-Arbeitsstation verwenden, können Sie die abgeleiteten Geräteschlüssel mit PowerShell generieren, wie im folgenden Beispiel gezeigt.

Ersetzen Sie den Wert von **KEY** durch den **Primärschlüssel**, den Sie zuvor notiert haben.

Ersetzen Sie den Wert von **REG_ID** durch die Registrierungs-ID Ihres Geräts.

```powershell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```powershell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```

## <a name="install-the-iot-edge-runtime"></a>Installieren der IoT Edge-Runtime

Die IoT Edge-Runtime wird auf allen IoT Edge-Geräten bereitgestellt. Die Komponenten werden in Containern ausgeführt, und Sie können weitere Container auf dem Gerät bereitstellen, um Code im Edge-Bereich auszuführen.

Sie benötigen die folgenden Informationen, wenn Sie Ihr Gerät bereitstellen:

* Den Wert für den DPS-**ID-Bereich**
* Die von Ihnen erstellte **Registrierungs-ID** für das Gerät
* Den abgeleiteten Geräteschlüssel des Geräts für den Nachweis symmetrischer Schlüssel

### <a name="linux-device"></a>Linux-Gerät

Befolgen Sie die Anweisungen für die Architektur Ihres Geräts. Stellen Sie sicher, dass Sie die IoT Edge-Runtime für die automatische und nicht für die manuelle Bereitstellung konfigurieren.

[Installieren der Azure IoT Edge-Runtime unter Linux](how-to-install-iot-edge-linux.md)

Der Abschnitt in der Konfigurationsdatei für die Bereitstellung von symmetrischen Schlüsseln sieht wie folgt aus:

```yaml
# DPS symmetric key provisioning configuration
provisioning:
   source: "dps"
   global_endpoint: "https://global.azure-devices-provisioning.net"
   scope_id: "{scope_id}"
   attestation:
      method: "symmetric_key"
      registration_id: "{registration_id}"
      symmetric_key: "{symmetric_key}"
```

Ersetzen Sie die Platzhalterwerte für `{scope_id}`, `{registration_id}` und `{symmetric_key}` durch die Daten, die Sie zuvor gesammelt haben.

### <a name="windows-device"></a>Windows-Gerät

Befolgen Sie die Anweisungen zum Installieren der IoT Edge-Runtime auf dem Gerät, für das Sie einen abgeleiteten Geräteschlüssel generiert haben. Stellen Sie sicher, dass Sie die IoT Edge-Runtime für die automatische und nicht für die manuelle Bereitstellung konfigurieren.

[Installation und automatische Bereitstellung von IoT Edge unter Windows](how-to-install-iot-edge-windows.md#option-2-install-and-automatically-provision)

## <a name="verify-successful-installation"></a>Bestätigen einer erfolgreichen Installation

Wenn die Runtime erfolgreich gestartet wurde, können Sie zu Ihrem IoT Hub navigieren und mit dem Bereitstellen von IoT Edge-Modulen auf Ihrem Gerät beginnen. Verwenden Sie die folgenden Befehle auf Ihrem Gerät, um zu überprüfen, ob das Installieren und Starten der Runtime erfolgreich war.

### <a name="linux-device"></a>Linux-Gerät

Überprüfen Sie den Status des IoT Edge-Diensts.

```cmd/sh
systemctl status iotedge
```

Untersuchen Sie die Dienstprotokolle.

```cmd/sh
journalctl -u iotedge --no-pager --no-full
```

Führen Sie ausgeführte Module auf.

```cmd/sh
iotedge list
```

### <a name="windows-device"></a>Windows-Gerät

Überprüfen Sie den Status des IoT Edge-Diensts.

```powershell
Get-Service iotedge
```

Untersuchen Sie die Dienstprotokolle.

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
```

Führen Sie ausgeführte Module auf.

```powershell
iotedge list
```

Sie können überprüfen, ob die individuelle Registrierung, die Sie im Device Provisioning Service erstellt haben, verwendet wurde. Navigieren Sie zu Ihrer Device Provisioning Service-Instanz im Azure-Portal. Öffnen Sie die Registrierungsdetails für die von Ihnen erstellte individuelle Registrierung. Beachten Sie, dass der Status der Registrierung **Zugewiesen** lautet und die Geräte-ID aufgeführt ist.

## <a name="next-steps"></a>Nächste Schritte

Der Registrierungsprozess des Device Provisioning-Diensts ermöglicht Ihnen, die Geräte-ID und die Tags von Gerätezwillingen beim Bereitstellen des neuen Geräts zu sehen. Sie können diese Werte verwenden, um einzelne Geräte oder Gruppen von Geräten über die automatische Geräteverwaltung als Ziel festzulegen. Weitere Informationen finden Sie unter [Bedarfsgerechtes Bereitstellen und Überwachen von IoT Edge-Modulen mithilfe des Azure-Portals](how-to-deploy-monitor.md) oder [Verwenden der Azure CLI](how-to-deploy-monitor-cli.md).
