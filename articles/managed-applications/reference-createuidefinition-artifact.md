---
title: 'Verwaltete Azure-Anwendung: Referenz für das Artefakt „createUiDefinition“'
description: Bei diesem Artikel handelt es sich um einen Referenzartikel für das Artefakt „createUiDefinition“.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 07/11/2019
ms.openlocfilehash: 5168e977a7bc289ec8981ef1df8cddd5cc6df678
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68336632"
---
# <a name="reference-user-interface-elements-artifact"></a>Referenz: Artefakt für Benutzeroberflächenelemente

Bei diesem Artikel handelt es sich um einen Referenzartikel für ein Artefakt vom Typ *createUiDefinition.json* in Azure Managed Applications. Weitere Informationen zum Erstellen von Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).

## <a name="user-interface-elements"></a>Benutzeroberflächenelemente

Der folgende JSON-Code zeigt ein Beispiel für eine Datei vom Typ *createUiDefinition.json* für Azure Managed Applications:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {}
    ],
    "steps": [
      {
        "name": "applicationSettings",
        "label": "Application Settings",
        "subLabel": {
          "preValidation": "Configure your application settings",
          "postValidation": "Done"
        },
        "bladeTitle": "Application Settings",
        "elements": [
          {
            "name": "funcname",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the function to be created",
            "toolTip": "Name of the function to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "storagename",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the storage to be created",
            "toolTip": "Name of the storage to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "zipFileBlobUri",
            "type": "Microsoft.Common.TextBox",
            "defaultValue": "https://github.com/Azure/azure-quickstart-templates/tree/master/101-custom-rp-with-function/artifacts/functionzip/functionpackage.zip",
            "label": "The Uri to the uploaded function zip file",
            "toolTip": "The Uri to the uploaded function zip file",
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "funcname": "[steps('applicationSettings').funcname]",
      "storageName": "[steps('applicationSettings').storagename]",
      "zipFileBlobUri": "[steps('applicationSettings').zipFileBlobUri]"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte

- [Tutorial: Erstellen einer verwalteten Anwendung mit benutzerdefinierten Aktionen und Ressourcen](tutorial-create-managed-app-with-custom-provider.md)
- [Referenz: Bereitstellungsvorlagenartefakt](reference-main-template-artifact.md)
- [Referenz: Ansichtsdefinitionsartefakt](reference-view-definition-artifact.md)