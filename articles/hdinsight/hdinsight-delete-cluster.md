---
title: 'Gewusst wie: Löschen eines HDInsight-Clusters – Azure'
description: Informationen zu den verschiedenen Möglichkeiten, einen Azure HDInsight-Cluster zu löschen
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 08bfdcab0b7fbb8e533cb8d2d6a74d315ad4074c
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70885229"
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a>Löschen eines HDInsight-Clusters mithilfe von Browser, PowerShell oder Azure-Befehlszeilenschnittstelle

Die Abrechnung für einen HDInsight-Cluster beginnt, sobald der Cluster erstellt wurde, und endet mit dem Löschen des Clusters. Die Gebühren werden anteilig nach Minuten erhoben. Daher sollten Sie Ihren Cluster immer löschen, wenn Sie ihn nicht mehr verwenden. In diesem Artikel erfahren Sie, wie Sie einen Cluster mithilfe des [Azure-Portals](https://portal.azure.com), des [Azure PowerShell Az-Modul](https://docs.microsoft.com/powershell/azure/overview) und der [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) löschen.

> [!IMPORTANT]  
> Das Löschen eines HDInsight-Clusters führt nicht zum Löschen der zugeordneten Azure Storage-Konten oder Data Lake Storage-Instanz. Sie können die in diesen Diensten gespeicherten Daten künftig wiederverwenden.

## <a name="azure-portal"></a>Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie vom linken Menü zu **Alle Dienste** > **Analytics** > **HDInsight-Cluster**, und wählen Sie Ihren Cluster aus.

3. Wählen Sie in der Standardansicht das Symbol **Löschen** aus. Folgen Sie der Aufforderung, Ihren Cluster zu löschen.
   
    ![Löschen-Symbol](./media/hdinsight-delete-cluster/hdinsight-delete-cluster.png)

## <a name="azure-powershell-az-module"></a>Azure PowerShell Az-Modul

Ersetzen Sie im folgenden Code `CLUSTERNAME` durch den Namen Ihres HDInsight-Clusters. Geben Sie über eine PowerShell-Eingabeaufforderung den folgenden Befehl ein, um den Cluster zu löschen:

```powershell
Remove-AzHDInsightCluster -ClusterName CLUSTERNAME
```

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

Ersetzen Sie im folgenden Code `CLUSTERNAME` durch den Namen Ihres HDInsight-Clusters und `RESOURCEGROUP` durch den Namen Ihrer Ressourcengruppe.  Geben Sie über eine Eingabeaufforderung Folgendes ein, um den Cluster zu löschen:

```azurecli
az hdinsight delete --name CLUSTERNAME --resource-group RESOURCEGROUP
```
