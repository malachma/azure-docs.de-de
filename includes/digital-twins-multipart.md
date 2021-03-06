---
title: include file
description: include file
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 08/12/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: f9b1b32775393196a77bef3aa2257f6fb66afdcb
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012136"
---
> [!NOTE]
> Mehrteilige Anforderungen umfassen in der Regel drei Informationselemente:
> * Einen Header vom Typ **Content-Type**:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * Eine **Content-Disposition**:
>   * `form-data; name="metadata"`
> * Der hochzuladende Dateiinhalt
>
> **Content-Type** und **Content-Disposition** variieren je nach Verwendungsszenario.

Mehrteilige Anforderungen können programmgesteuert vorgenommen werden (über C#), über einen REST-Client oder Tools wie z.B. [Postman](https://docs.microsoft.com/azure/digital-twins/how-to-configure-postman#multi). REST-Clienttools weisen möglicherweise verschiedene Ebenen der Unterstützung für komplexe mehrteiligen Anforderungen auf. Konfigurationseinstellungen können von Tool zu Tool leicht variieren. Überprüfen Sie, welches Tool am besten für Ihre Anforderungen geeignet ist.

> [!IMPORTANT]
> Mehrteilige Anforderungen an Verwaltungs-APIs von Azure Digital Twins bestehen normalerweise aus zwei Teilen:
> * Blobmetadaten (z.B. ein zugeordneter MIME-Typ), der von **Content-Type** und/oder **Content-Disposition** deklariert wird.
> * Blobinhalt einschließlich der unstrukturierten Inhalte einer hochzuladenden Datei
>
> Bei **PATCH**-Anforderungen wird keiner der beiden Teile benötigt. Bei **POST**- oder Erstellungsvorgängen werden dagegen beide Teile benötigt.

Der [Occupancy Quickstart-Quellcode](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/api/update.cs) enthält vollständige C#-Beispiele, die zeigen, wie mehrteilige Anforderungen an die Verwaltungs-APIs von digitalen Zwillingen in Azure gerichtet werden.