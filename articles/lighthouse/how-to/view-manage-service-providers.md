---
title: Anzeigen und Verwalten von Dienstanbietern in Azure-Portal
description: Kunden können die Seite „Dienstanbieter“ im Azure-Portal verwenden, um Informationen zu Dienstanbietern, Angeboten von Dienstanbietern und delegierten Ressourcen anzuzeigen.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: a45458e7417bba058522fdc0dbc34fee04ad9af8
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810824"
---
# <a name="view-and-manage-service-providers"></a>Dienstanbieter anzeigen und verwalten

Kunden können die Seite **Dienstanbieter** im [Azure-Portal](https://portal.azure.com) verwenden, um Informationen zu Dienstanbietern und Angeboten von Dienstanbietern anzuzeigen, um bestimmte Ressourcen mithilfe der [delegierten Azure-Ressourcenverwaltung](../concepts/azure-delegated-resource-management.md) zu delegieren sowie um zusätzliche Angebote von Dienstanbietern zu suchen und kaufen. Zwar beziehen wir uns hier auf Dienstanbieter und Kunden, doch können Unternehmen, die mehrere Mandanten verwalten, denselben Prozess verwenden, um ihre Verwaltungserfahrung zu konsolidieren.

Um auf die Seite **Dienstanbieter** im Azure-Portal zuzugreifen, können die Kunden **Alle Dienste** auswählen, dann nach **Dienstanbieter** suchen und es auswählen. Sie können die Seite auch finden, indem Sie im Suchfeld am oberen Rand des Azure-Portals „Dienstanbieter“ eingeben.

Beachten Sie, dass auf der Seite **Dienstanbieter** nur Informationen zu den Dienstanbietern angezeigt werden, die über die delegierte Azure-Ressourcenverwaltung Zugriff auf die Abonnements oder Ressourcengruppen des Kunden haben. Wenn ein Kunde mit zusätzlichen Dienstanbietern arbeitet, die nicht die delegierte Azure-Ressourcenverwaltung verwenden, um auf Ressourcen des Kunden zuzugreifen, werden Informationen zu diesen Dienstanbietern hier nicht angezeigt.

> [!NOTE]
> Dienstanbieter können Informationen zu ihren Kunden anzeigen, indem Sie im Azure-Portal zu **Meine Kunden** navigieren. Weitere Informationen finden Sie unter [Anzeigen und Verwalten von Kunden und delegierten Ressourcen](view-manage-customers.md).

## <a name="view-service-provider-details"></a>Anzeigen von Dienstanbieterdetails

Zum Anzeigen von Informationen über die Dienstanbieter, mit denen ein Kunde arbeitet, kann dieser im linken Bereich der Seite **Dienstanbieter** die Option **Anbieterangebote** auswählen.

Zu jedem Angebot eines Dienstanbieters werden dem Kunden der Name des Dienstanbieters und das dazugehörige Angebot angezeigt sowie der Name, den der Kunde während des Onboardingvorgangs eingegeben hat.

In der Spalte **Delegierungen** sieht der Kunde, wie viele Abonnements und/oder Ressourcengruppen an den Dienstanbieter für dieses Angebot delegiert wurden. Der Dienstanbieter ist in der Lage, auf diese Abonnements und/oder Ressourcengruppen entsprechend den im Angebot angegebenen Zugriffsebenen zuzugreifen und diese dementsprechend zu verwalten.

## <a name="delegate-resources"></a>Delegieren von Ressourcen

Bevor ein Dienstanbieter auf die Ressourcen eines Kunden zugreifen und diese verwalten kann, müssen diese delegiert werden. Wenn ein Kunde ein Angebot angenommen hat, aber noch keine Ressourcen delegiert hat, wird am oberen Rand des Abschnitts **Anbieterangebote** ein Hinweis angezeigt. In diesem wird dem Kunden mitgeteilt, dass er Maßnahmen ergreifen muss, bevor der Dienstanbieter auf eine der Ressourcen des Kunden zugreifen kann.

So delegieren Sie Abonnements oder Ressourcengruppen

1. Aktivieren Sie das Kontrollkästchen für die Zeile, die den Dienstanbieter, das Angebot und den Namen enthält. Wählen Sie dann am oberen Rand des Bildschirms **Ressourcen delegieren** aus.
1. Überprüfen Sie im Abschnitt **Angebotsdetails** der Seite **Ressourcen delegieren** die Details zum Dienstanbieter und Angebot. Um Rollenzuweisungen für das Angebot zu überprüfen, wählen Sie **Klicken Sie hier, um die Details des ausgewählten Angebots anzuzeigen** aus.
1. Wählen Sie im Abschnitt **Delegieren** den Befehl **Abonnements delegieren** oder **Ressourcengruppen delegieren** aus.
1. Wählen Sie die Abonnements und/oder Ressourcengruppen aus, die Sie für dieses Angebot delegieren möchten, und wählen Sie dann **Hinzufügen** aus.
1. Aktivieren Sie das Kontrollkästchen unten auf der Seite, um zu bestätigen, dass Sie diesem Dienstanbieter Zugriff auf die Ressourcen gewähren möchten, die Sie ausgewählt haben, und wählen Sie dann **Delegieren** aus.

## <a name="add-or-remove-service-provider-offers"></a>Hinzufügen oder Entfernen von Dienstanbieterangeboten

Ein Kunde kann ein neues Angebot eines Dienstanbieters über die Seite **Anbieterangebote** hinzufügen, indem er **Angebot** hinzufügen auswählt. Der Dienstanbieter muss ein Angebot für diesen Kunden veröffentlicht haben. Der Kunde kann das Angebot dann im Bildschirm **Private Angebote** auswählen und dann **Erstellen** auswählen.

Wenn der Kunde ein Angebot eines Dienstanbieters entfernen möchte, kann er das Papierkorbsymbol in der Zeile für dieses Angebot auswählen. Nach dem Bestätigen des Löschvorgangs hat dieser Dienstanbieter keinen Zugriff mehr auf die Kundenressourcen, die zuvor für dieses Angebot delegiert wurden.

## <a name="view-delegations"></a>Anzeigen von Delegierungen

Delegierungen stellen die Rollenzuweisungen dar, die dem Dienstanbieter Berechtigungen für die von einem Kunden delegierten Ressourcen erteilen. Um diese Informationen anzuzeigen, wählen Sie im linken Bereich der Seite **Dienstanbieter** die Option **Delegierungen** aus.

Mithilfe von Filtern am oberen Rand der Seite können Sie Ihre Delegierungsinformationen sortieren und gruppieren oder nach bestimmten Kunden, Angeboten oder Schlüsselwörtern filtern.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Azure Lighthouse](../overview.md).
- Erfahren Sie, wie Dienstanbieter [Kunden anzeigen und verwalten](view-manage-customers.md) können, indem sie im Azure-Portal zu **Meine Kunden** navigieren.