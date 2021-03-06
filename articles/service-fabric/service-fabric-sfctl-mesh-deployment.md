---
title: Azure Service Fabric-CLI – sfctl mesh deployment | Microsoft-Dokumentation
description: Beschreibt die sfctl mesh deployment-Befehle der Service Fabric-CLI.
services: service-fabric
documentationcenter: na
author: Christina-Kang
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2018
ms.author: bikang
ms.openlocfilehash: b3f506b46ef563f47fc7c67b759d3fcd08b7509d
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035184"
---
# <a name="sfctl-mesh-deployment"></a>sfctl mesh deployment
Erstellt Service Fabric Mesh-Ressourcen.

## <a name="commands"></a>Befehle

|Get-Help|BESCHREIBUNG|
| --- | --- |
| create | Erstellt eine Bereitstellung von Service Fabric-Mesh-Ressourcen. |

## <a name="sfctl-mesh-deployment-create"></a>sfctl mesh deployment create
Erstellt eine Bereitstellung von Service Fabric-Mesh-Ressourcen.

### <a name="arguments"></a>Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --input-yaml-files [erforderlich] | Durch Trennzeichen getrennte relative/absolute Dateipfade aller YAML-Dateien oder relativer/absoluter Pfad des Verzeichnisses (rekursiv) mit YAML-Dateien. |
| --parameters | Ein relativer/absoluter Pfad zur YAML-Datei oder ein JSON-Objekt, das die Parameter enthält, die außer Kraft gesetzt werden müssen. |

### <a name="global-arguments"></a>Globale Argumente

|Argument|BESCHREIBUNG|
| --- | --- |
| --debug | Erhöht die Protokollierungsausführlichkeit, sodass alle Debugprotokolle angezeigt werden. |
| --help -h | Zeigt diese Hilfemeldung an und beendet. |
| --output -o | Das Ausgabeformat.  Zulässige Werte\: „json“, „jsonc“, „table“, „tsv“.  Standardwert\: „json“. |
| --query | JMESPath-Abfragezeichenfolge. Weitere Informationen und Beispiele finden Sie unter „http\://jmespath.org/“. |
| --verbose | Erhöht die Protokollierungsausführlichkeit. Verwenden Sie „--debug“, wenn Sie vollständige Debugprotokolle wünschen. |

### <a name="examples"></a>Beispiele

Konsolidiert und stellt alle Ressourcen für den Cluster bereit, indem die Parameter in der YAML-Datei außer Kraft gesetzt werden.

```
sfctl mesh deployment create --input-yaml-files ./app.yaml,./network.yaml --parameters
./param.yaml
```

Konsolidiert und stellt alle Ressourcen in einem Verzeichnis für den Cluster bereit, indem die Parameter in der YAML-Datei außer Kraft gesetzt werden.

```
sfctl mesh deployment create --input-yaml-files ./resources --parameters ./param.yaml
```

Konsolidiert und stellt alle Ressourcen in einem Verzeichnis für den Cluster bereit, indem die Parameter außer Kraft gesetzt werden, die direkt als JSON-Objekt übergeben werden.

```
sfctl mesh deployment create --input-yaml-files ./resources --parameters "{ 'my_param' :
{'value' : 'my_value'} }"
```


## <a name="next-steps"></a>Nächste Schritte
- [Einrichten](service-fabric-cli.md) der Service Fabric-Befehlszeilenschnittstelle
- Informationen zum Verwenden der Service Fabric-Befehlszeilenschnittstelle mit den [Beispielskripts](/azure/service-fabric/scripts/sfctl-upgrade-application)