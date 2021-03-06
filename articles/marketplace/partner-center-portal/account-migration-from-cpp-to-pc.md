---
title: Kontomigration vom Cloud-Partnerportal zu Partner Center – Kommerzieller Marketplace für Azure
description: Hier erfahren Sie, wie Sie Ihr Konto von CPP zu Partner Center migrieren. – Kommerzieller Marketplace für Azure
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: 8c7680896507cd3738fa3bce0d30a516d08509c4
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383284"
---
# <a name="account-migration-from-cloud-partner-portal-to-partner-center"></a>Kontomigration vom Cloud-Partnerportal zu Partner Center

Wenn Ihre Angebote vom Cloud-Partnerportal (CPP) zu Partner Center (PC) migriert werden, werden sie für die Bearbeitung in CPP gesperrt. An diesem Punkt müssen Ihre Kontoeinstellungen zu Partner Center migriert werden. Sowohl Ihre Kontoeinstellungen als auch Ihre Angebote können in Partner Center verwaltet werden.

## <a name="account-migration-process"></a>Kontomigrationsprozess

Wenn Angebote von CPP migriert werden, ist Ihr Konto für die Migration konfiguriert. 
 
Wenn Sie ein Benutzer mit der Rolle „Besitzer“ in CPP für ein bestimmtes Konto sind, wird ein gelbes Banner auf der Seite mit Ihrem Herausgeberprofil angezeigt. Sie werden aufgefordert, Ihre Kontoeinstellungen in Partner Center zu verschieben. 

Klicken Sie auf das Banner, um den Prozess der Kontomigration einzuleiten. Es werden folgende Eingaben von Ihnen erwartet:

### <a name="work-email-address"></a>E-Mail-Adresse (geschäftlich)

In den meisten Fällen melden Sie sich mit der E-Mail-Adresse an, die Sie zum Anmelden bei CPP verwenden. In bestimmten Fällen muss eine andere E-Mail-Adresse verwendet werden:

* Microsoft-Konto: Wenn das CPP-Konto ein Microsoft-Konto ist, müssen Sie eine gültige geschäftliche E-Mail-Adresse eingeben, die dem Mandanten zugeordnet ist, für den die MPN-ID registriert ist.

* Mandantenkonflikt: Wenn Ihre geschäftliche E-Mail-Adresse nicht zu dem Mandanten gehört, dem die Microsoft Partner Network-ID in Ihrem CPP-Konto zugeordnet ist, wird ein Fehler angezeigt. Um diesen Fehler zu umgehen, geben Sie eine E-Mail-Adresse ein, die dem Mandanten zugeordnet ist. In einer Fehlermeldung ist der Name des Mandanten angegeben.

### <a name="sign-up-for-microsoft-partner-network-program"></a>Registrieren für das Microsoft Partner Network-Programm

Falls Ihr CPP-Konto keine Microsoft Partner Network-ID aufweist oder die ID ungültig ist, müssen Sie sich im Rahmen des Aktivierungsprozesses für das Microsoft Partner Network-Programm registrieren.

## <a name="after-account-migration-is-complete"></a>Nach Abschluss der Kontomigration

Die Migration muss nur ein Mal für ein bestimmtes Konto ausgeführt werden. Nachdem ein bestimmter Partner die Migration für das Konto abgeschlossen hat, wird allen Besitzern folgendes Verhalten auf der Seite mit ihrem Herausgeberprofil angezeigt:

1. Es wird die Seite mit den Partnereinstellungen in Microsoft Partner Network angezeigt, auf der Sie nun Kontoeinstellungen verwalten können. 
2. Ein gelbes Banner auf Ihrer Herausgeberprofilseite in CPP wird Benutzern mit der Rolle „Besitzer“ angezeigt und fordert sie auf, ihre Kontoeinstellungen in Partner Center zu verwalten.
3. Die Seite mit den Kontoeinstellungen in CPP wechselt daraufhin in den schreibgeschützten Modus.

## <a name="move-dynamics-365-based-solutions-to-partner-center"></a>Verschieben von Dynamics 365-basierten Lösungen in Partner Center

Wenn Sie Dynamics 365 for Customer Engagement- oder Dynamics 365 for Finance and Operations-Lösungen im GTM-Portal für One Commercial Partner erstellt haben, **sollten diese Lösungen nun in Partner Center verwaltet werden**.

**Wenn Sie Ihre Lösungen nicht bis zum 31. August 2019 verschoben haben**, führen Sie die folgenden Schritte so bald wie möglich aus. Bis dahin gilt:
- ISVs haben keinen Zugriff auf Marketingvorteile
- Co-Selling priorisiert verliert den Status
- Für Cloud Embed besteht nach dem 15. Oktober 2019 keine Compliance

> [!NOTE]
> Wenn Ihr MPN-Mitgliedschaftskonto ursprünglich in Partner Membership Center (PMC) erstellt wurde, melden Sie sich bei [Partner Center](https://partner.microsoft.com/pcv/accountsettings/connectedpartnerprofile) an, um zu bestätigen, dass Ihr Konto migriert wurde, bevor Sie die folgenden Schritte ausführen. Wenn ein Profilbildschirm mit Ihrer MPN-ID angezeigt wird, können Sie fortfahren. Wenn das nicht der Fall ist, müssen Sie mit der Kontomigration beginnen, indem Sie den Anweisungen im [Partner Membership Center](https://partners.microsoft.com/partnerprogram/Welcome.aspx) folgen. Wenn Sie hierbei Hilfe benötigen, besuchen Sie den [Support](https://partner.microsoft.com/support?issueid=100-0077).

1. Rufen Sie die [Seite mit dem Überblick über den kommerziellen Marketplace in Partner Center](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) auf. Wenn im linken Navigationsbereich „Kommerzieller Marketplace“ angezeigt wird, sind Sie registriert und sollten mit dem nächsten Schritt fortfahren. Wenn nicht, [registrieren Sie sich jetzt beim kommerziellen Marketplace](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/azureisv).
2. Vergewissern Sie sich, dass Ihre Angebote in AppSource vorhanden sind, indem Sie [nach Ihren Angeboten suchen](https://appsource.microsoft.com/). Sind Ihre Angebote bereits in AppSource vorhanden, fahren Sie mit dem nächsten Schritt fort. Für jedes Angebot, das nicht in AppSource enthalten ist, erstellen Sie ein [neues Dynamics 365 for Customer Engagement-Angebot](create-new-customer-engagement-offer.md) oder ein [neues Dynamics 365 for Operations-Angebot](create-new-operations-offer.md).
3. Überprüfen Sie Ihre Registrierung im Business Applications ISV Connect-Programm:
  
   * Stellen Sie sicher, dass Sie auf der Seite [Vereinbarungen](https://partner.microsoft.com/dashboard/account/agreements) in Partner Center das **Business Applications ISV Addendum** akzeptiert haben, um sich für das Programm zu registrieren.
   * Geben Sie auf der Seite [Kontoeinstellungen](https://partner.microsoft.com/dashboard/account/v3/accountsettings/billingprofile) Ihre Abrechnungsinformationen an.

4. Übermitteln Sie jedes neue und bestehende Angebot zur Zertifizierung, auch wenn Ihre Angebote bereits zuvor zertifiziert wurden. Wenn Sie dazu berechtigt sind, können Sie während dieses Prozesses die Teilnahme am Premium-Tarif beantragen. Wenn Ihr Angebot zuvor zertifiziert war, **müssen Sie die erneute Zertifizierung der App bis zum 15. Oktober 2019 durchführen.** Die Zertifizierung oder erneute Zertifizierung erfordert, dass Ihre App die neueste Version unserer Business Applications-Plattform unterstützt.
5. Wechseln Sie zum [GTM-Portal für One Commercial Partner](https://msgtm.azurewebsites.net/en-US/Profile/SignIn), und fügen Sie Ihre URL für die AppSource-Auflistung im Abschnitt mit Marketplace-Links hinzu. Wenn Sie Hilfe bei diesem Schritt benötigen, senden Sie uns eine E-Mail unter cosell@microsoft.com.

## <a name="next-steps"></a>Nächste Schritte

- [Verwalten Ihres Kontos im kommerziellen Marketplace in Partner Center](./manage-account.md) 
