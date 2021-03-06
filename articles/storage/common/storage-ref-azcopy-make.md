---
title: azcopy make | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy make“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 9afcd8de1af42424649dd8e44fc07f7bfd881257
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196736"
---
# <a name="azcopy-make"></a>azcopy make

Erstellt einen Container oder eine Dateifreigabe.

## <a name="synopsis"></a>Zusammenfassung

Erstellt einen Container oder eine Dateifreigabe, der bzw. die durch die angegebene Ressourcen-URL dargestellt wird.

```azcopy
azcopy make [resourceURL] [flags]
```

## <a name="examples"></a>Beispiele

```azcopy
azcopy make "https://[account-name].[blob,file,dfs].core.windows.net/[top-level-resource-name]"
```

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|-h, --help|Dient zum Anzeigen von Hilfeinhalt zum Befehl „make“. |
|--quota-gb uint32|Gibt die maximale Größe der Freigabe in Gigabyte (GiB) an. 0 bedeutet, dass Sie das Standardkontingent des Dateidiensts akzeptieren.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|--cap-mbps uint32|Begrenzt die Übertragungsrate (in Megabit pro Sekunde). Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|--output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“ und „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
