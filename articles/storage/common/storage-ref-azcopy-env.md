---
title: azcopy env | Microsoft-Dokumentation
description: Dieser Artikel enthält Referenzinformationen zum Befehl „azcopy env“.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 15e72493190e1bc56e779c22695bc51bd05da940
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70196756"
---
# <a name="azcopy-env"></a>azcopy env

Zeigt die Umgebungsvariablen, die das Verhalten von AzCopy konfigurieren können.

## <a name="synopsis"></a>Zusammenfassung

```azcopy
azcopy env [flags]
```

> [!IMPORTANT]
> Wenn Sie eine Umgebungsvariable über die Befehlszeile festlegen, kann diese Variable in Ihrem Befehlszeilenverlauf gelesen werden. Erwägen Sie das Löschen von Variablen mit Anmeldeinformationen aus Ihrem Befehlszeilenverlauf. Wenn Sie verhindern möchten, dass Variablen in Ihrem Verlauf angezeigt werden, können Sie mithilfe eines Skripts den Benutzer zur Eingabe seiner Anmeldeinformationen auffordern und die Umgebungsvariable festlegen.

## <a name="options"></a>Optionen

|Option|BESCHREIBUNG|
|--|--|
|-h, --help|Zeigt Hilfeinhalt zum Befehl „env“. |
|–show-sensitive|Zeigt sensible/geheime Umgebungsvariablen.|

## <a name="options-inherited-from-parent-commands"></a>Von übergeordneten Befehlen geerbte Optionen

|Option|BESCHREIBUNG|
|---|---|
|–cap-mbps uint32|Begrenzt die Übertragungsrate, in Megabit pro Sekunde. Der Schritt-für-Schritt-Durchsatz kann von der Obergrenze geringfügig abweichen. Wenn diese Option auf „null“ festgelegt oder weggelassen wird, ist der Durchsatz nicht begrenzt.|
|–output-type string|Format der Befehlsausgabe. Folgende Optionen sind verfügbar: „text“, „json“. Der Standardwert lautet „text“.|

## <a name="see-also"></a>Weitere Informationen

- [azcopy](storage-ref-azcopy.md)
