---
title: include file
description: include file
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 07/31/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: 154d71c9cbc109834a5854b46c3e6584dcefa7eb
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2019
ms.locfileid: "68968871"
---
### <a name="policy-for-users-in-your-directory"></a>Richtlinie: Für Benutzer in Ihrem Verzeichnis

Gehen Sie folgendermaßen vor, wenn Sie möchten, dass Ihre Richtlinie für Benutzer gilt, die sich in Ihrem Verzeichnis befinden und dieses Zugriffspaket anfordern können.  **Benutzer in Ihrem Verzeichnis** bezieht sich sowohl auf interne Benutzer als auch auf externe Benutzer, die zum Verzeichnis eingeladen wurden – entweder, indem sie mit einem anderen Zugriffspaket eine Berechtigung angefordert haben oder indem sie über Azure AD B2B eingeladen wurden. Beim Definieren der Richtlinie können Sie einzelne Benutzer oder Gruppen von Benutzern angeben. Beispielsweise verfügt Ihre Organisation möglicherweise bereits über eine Gruppe wie **Alle Mitarbeiter**.  Wenn diese Gruppe in die Richtlinie für Benutzer eingefügt wird, die Zugriff anfordern können, können alle Mitglieder dieser Gruppe Zugriff anfordern.

1. Wählen Sie im Abschnitt **Benutzer, die Zugriff anfordern können** die Option **Für in Ihrem Verzeichnis befindliche Benutzer** aus.

    Beachten Sie, dass die Einstellung **Für Benutzer in Ihrem Verzeichnis** sowohl Mitgliedsbenutzer als auch Gastbenutzer umfasst, die Ihrem Verzeichnis hinzugefügt wurden. Wenn Sie nur Mitgliederbenutzer und keine Gastbenutzer einschließen möchten, wählen Sie **Für Benutzer in Ihrem Verzeichnis** und dann eine Gruppe von Mitgliedsbenutzern aus. Bei Bedarf können Sie eine dynamische Gruppe Ihrer Mitgliedsbenutzer erstellen (user.userType -eq „Mitglied“). Weitere Informationen finden Sie unter [Regeln für eine dynamische Mitgliedschaft für Gruppen in Azure Active Directory](../articles/active-directory/users-groups-roles/groups-dynamic-membership.md).

1. Klicken Sie im Abschnitt **Benutzer und Gruppen auswählen** auf **Benutzer und Gruppen hinzufügen**.

1. Wählen Sie im Bereich „Benutzer und Gruppen auswählen“ die Benutzer und Gruppen aus, die Sie hinzufügen möchten.

    ![Zugriffspaket – Richtlinie – Benutzer und Gruppen auswählen](./media/active-directory-entitlement-management-policy/policy-select-users-groups.png)

1. Klicken Sie auf **Auswählen**, um die Benutzer und Gruppen hinzuzufügen.

1. Fahren Sie mit dem Abschnitt [Richtlinie: Anforderung](#policy-request) fort.

### <a name="policy-for-users-not-in-your-directory"></a>Richtlinie: Für Benutzer, die sich nicht in Ihrem Verzeichnis befinden

Gehen Sie folgendermaßen vor, wenn Sie möchten, dass Ihre Richtlinie für Benutzer gilt, die sich nicht in Ihrem Verzeichnis befinden und dieses Zugriffspaket anfordern können. **Benutzer, die sich nicht in Ihrem Verzeichnis befinden** bezieht sich auf Benutzer in einem anderen Azure AD-Verzeichnis, die bisher noch nicht zu Ihrem Verzeichnis eingeladen wurden. Derzeit können Sie nur Benutzer aus Organisationen mit Azure AD hinzufügen. Verzeichnisse müssen konfiguriert werden, damit sie in den **Einschränkungseinstellungen für die Zusammenarbeit in Organisationsbeziehungen** zugelassen werden.

> [!NOTE]
> Für einen Benutzer, der sich noch nicht in Ihrem Verzeichnis befindet und dessen Anforderung genehmigt oder automatisch genehmigt wird, wird ein externes Gastbenutzerkonto erstellt. Der Gast wird eingeladen, erhält jedoch keine Einladungs-E-Mail. Stattdessen erhält er eine E-Mail, wenn seine Zugriffspaketzuweisung bereitgestellt wird. Wenn dieser Gastbenutzer zu einem späteren Zeitpunkt keine Zugriffspaketzuweisungen mehr besitzt, weil die letzte Zuweisung abgelaufen ist oder abgebrochen wurde, wird das Gastbenutzerkonto standardmäßig für die Anmeldung blockiert und anschließend gelöscht. Wenn Gastbenutzer dauerhaft in Ihrem Verzeichnis bleiben sollen, auch wenn sie keine Zugriffspaketzuweisungen haben, können Sie die Einstellungen für Ihre Berechtigungsverwaltungskonfiguration ändern.

1. Wählen Sie im Abschnitt **Benutzer, die Zugriff anfordern können** die Option **Für Benutzer, die sich nicht in Ihrem Verzeichnis befinden** aus.

1. Klicken Sie im Abschnitt **Externes Azure AD-Verzeichnis auswählen** auf **Verzeichnisse hinzufügen**.

1. Geben Sie einen Domänennamen ein, und suchen Sie nach einem Azure AD-Verzeichnis mit diesem Domänennamen.

1. Stellen Sie anhand des angegebenen Verzeichnisnamens und der ursprünglichen Domäne sicher, dass es sich um das richtige Verzeichnis handelt.

    > [!NOTE]
    > Alle Benutzer aus dem Verzeichnis werden dieses Zugriffspaket anfordern können. Hierzu gehören Benutzer aus allen Unterdomänen, die diesem Verzeichnis zugeordnet sind, nicht nur der Domäne, die in der Suche verwendet wird.

    ![Zugriffspaket – Richtlinie – Verzeichnisse auswählen](./media/active-directory-entitlement-management-policy/policy-select-directories.png)

1. Klicken Sie auf **Hinzufügen**, um das Verzeichnis hinzuzufügen.

1. Wiederholen Sie diesen Schritt, um weitere Verzeichnisse hinzufügen.

1. Nachdem Sie alle Verzeichnisse hinzugefügt haben, die Sie in die Richtlinie einbeziehen möchten, klicken Sie auf **Auswählen**.

1. Fahren Sie mit dem Abschnitt [Richtlinie: Anforderung](#policy-request) fort.

### <a name="policy-none-administrator-direct-assignments-only"></a>Richtlinie: Keine (nur direkte Administratorzuweisungen)

Gehen Sie folgendermaßen vor, wenn Ihre Richtlinie Zugriffsanforderungen umgehen und Administratoren ermöglichen soll, bestimmte Benutzer direkt dem Zugriffspaket zuzuweisen. Benutzer müssen das Zugriffspaket nicht anfordern. Sie können weiterhin Ablaufeinstellungen festlegen, aber es gibt keine Anforderungseinstellungen.

1. Wählen Sie im Abschnitt **Benutzer, die Zugriff anfordern können** die Option **Keine (nur direkte Zuweisungen eines Administrators)** aus.

    Nach der Erstellung des Zugriffspakets können Sie direkt bestimmte interne und externe Benutzer dem Zugriffspaket zuweisen. Wenn Sie einen externen Benutzer angeben, wird ein Gastbenutzerkonto in Ihrem Verzeichnis erstellt.

1. Fahren Sie mit dem Abschnitt [Richtlinie: Ablauf](#policy-expiration) fort.

### <a name="policy-request"></a>Richtlinie: Anforderung

Legen Sie im Abschnitt „Anforderung“ Genehmigungseinstellungen fest, wenn Benutzer das Zugriffspaket anfordern.

1. Um die Genehmigung für Anforderungen der ausgewählten Benutzer anzufordern, setzen Sie **Genehmigung anfordern** auf **Ja**. Um Anforderungen automatisch genehmigen zu lassen, legen Sie **Nein** fest.

1. Wenn Sie die Genehmigung anfordern, klicken Sie im Abschnitt **Genehmigende Personen auswählen** auf **Genehmigende Personen hinzufügen**.

1. Wählen Sie im Bereich „Genehmigende Personen auswählen“ einen oder mehrere Benutzer und/oder Gruppen als genehmigende Personen aus.

    Nur eine der ausgewählten genehmigenden Personen muss eine Anforderung genehmigen. Genehmigung durch alle genehmigenden Personen ist nicht erforderlich. Die Genehmigungsentscheidung basiert darauf, welche genehmigende Person die Anforderung zuerst überprüft.

    ![Zugriffspaket – Richtlinie – genehmigende Personen auswählen](./media/active-directory-entitlement-management-policy/policy-select-approvers.png)

1. Klicken Sie auf **Auswählen**, um die genehmigende Person hinzuzufügen.

1. Klicken Sie auf **Erweiterte Anforderungseinstellungen anzeigen**, um zusätzliche Einstellungen anzuzeigen.

    ![Zugriffspaket – Richtlinie – Verzeichnisse auswählen](./media/active-directory-entitlement-management-policy/policy-advanced-request.png)

1. Um von Benutzern eine Begründung zum Anfordern des Zugriffspakets anzufordern, setzen Sie **Begründung erforderlich** auf **Ja**.

1. Um von genehmigenden Personen eine Begründung zum Genehmigen einer Anforderung des Zugriffspakets anzufordern, setzen Sie **Begründung der genehmigenden Person erforderlich** auf **Ja**.

1. Geben Sie im Feld **Timeout für Genehmigungsanforderung (Tage)** den Zeitraum an, in dem genehmigende Personen eine Anforderung überprüfen müssen. Wenn keine genehmigende Person sie in dieser Anzahl von Tagen überprüft, läuft die Anforderung ab, und der Benutzer muss eine andere Anforderung für das Zugriffspaket senden.

### <a name="policy-expiration"></a>Richtlinie: Ablauf

Geben Sie im Abschnitt „Ablauf“ an, wann die Zuweisung eines Benutzers zum Zugriffspaket abläuft.

1. Legen Sie im Abschnitt **Ablauf** die Option **Zugriffspaket läuft ab** auf **An Datum**, **Anzahl Tage** oder **Nie** fest.

    Wählen Sie für **An Datum** ein Ablaufdatum in der Zukunft aus.

    Geben Sie für **Anzahl Tage** eine Zahl zwischen 0 und 3.660 Tagen an.

    Basierend auf Ihrer Auswahl läuft die Zuweisung eines Benutzers zum Zugriffspaket an einem bestimmten Datum, nach einer bestimmten Anzahl von Tagen nach der Genehmigung oder nie ab.

1. Klicken Sie auf **Erweiterte Ablaufeinstellungen anzeigen**, um zusätzliche Einstellungen anzuzeigen.

1. Um Benutzern zu erlauben, ihre Zuweisungen zu erweitern, setzen Sie **Benutzern die Verlängerung des Zugriffs erlauben** auf **Ja**.

    Wenn Verlängerungen in der Richtlinie zulässig sind, erhält der Benutzer 14 Tage und auch 1 Tag vor Ablauf seiner Zugriffspaketzuweisung eine E-Mail, die ihn auffordert, die Zuweisung verlängern zu lassen.

    ![Zugriffspaket – Richtlinie – Ablaufeinstellungen](./media/active-directory-entitlement-management-policy/policy-expiration.png)

### <a name="policy-enable-policy"></a>Richtlinie: Richtlinie aktivieren

1. Wenn Sie das Zugriffspaket dem Benutzer in der Richtlinie sofort zur Verfügung stellen möchten, klicken Sie auf **Ja**, um die Richtlinie zu aktivieren.

    Sie können sie in der Zukunft immer aktivieren, nachdem Sie das Erstellen des Zugriffspakets abgeschlossen haben.

    ![Zugriffspaket – Richtlinie – Richtlinieneinstellung aktivieren](./media/active-directory-entitlement-management-policy/policy-enable.png)

1. Klicken Sie auf **Weiter** oder **Erstellen**.
