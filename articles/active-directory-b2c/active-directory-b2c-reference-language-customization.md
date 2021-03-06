---
title: Sprachanpassung in Azure Active Directory B2C
description: Erfahren Sie mehr über das Anpassen der Oberfläche für die Sprache in Ihren Benutzerflows.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: bced7a4b994172a1a2076149d6f25adb39c99b54
ms.sourcegitcommit: fe50db9c686d14eec75819f52a8e8d30d8ea725b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2019
ms.locfileid: "69015565"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Sprachanpassung in Azure Active Directory B2C

Mit der Sprachanpassung in Azure Active Directory B2C (Azure AD B2C) kann Ihr Benutzerflow verschiedene Sprachen abdecken, um Ihre Kundenanforderungen zu erfüllen. Microsoft stellt Übersetzungen für [36 Sprachen](#supported-languages) bereit. Sie können aber auch eigene Übersetzungen für beliebige Sprachen bereitstellen. Auch wenn Ihre Benutzeroberfläche nur für eine einzelne Sprache bestimmt ist, können Sie beliebigen Text auf den Seiten anpassen.

## <a name="how-language-customization-works"></a>Funktionsweise der Sprachanpassung

Mit der Sprachanpassung können Sie die Sprachen auswählen, in denen Ihr Benutzerflow verfügbar ist. Nach Aktivierung des Features können Sie über Ihre Anwendung den Abfragezeichenfolgen-Parameter `ui_locales` angeben. Wenn Sie Azure AD B2C aufrufen, wird Ihre Seite in das von Ihnen angegebene Gebietsschema übersetzt. Mit dieser Art von Konfiguration haben Sie die vollständige Kontrolle über die Sprachen Ihres Benutzerflows, und die Spracheinstellungen des Kundenbrowsers können ignoriert werden.

Möglicherweise müssen Sie aber gar nicht so genau steuern, welche Sprachen Ihren Kunden angezeigt werden. Wenn Sie den Parameter `ui_locales` nicht angeben, wird die Benutzeroberfläche des Kunden durch die Einstellungen des Browsers vorgegeben. Sie können weiterhin steuern, in welche Sprachen Ihr Benutzerflow übersetzt wird, indem Sie ihn als unterstützte Sprache hinzufügen. Wenn der Browser eines Kunden mit einer Sprache konfiguriert ist, die Sie nicht unterstützen möchten, wird stattdessen die Sprache angezeigt, die Sie als Standardeinstellung für die unterstützte Kultur ausgewählt haben.

* **In „ui-locales“ angegebene Sprache**: Nach Aktivierung der Sprachanpassung wird Ihr Benutzerflow in die hier angegebene Sprache übersetzt.
* **Vom Browser angeforderte Sprache**: Ohne Angabe des Parameters `ui_locales` wird Ihr Benutzerflow in die vom Browser angeforderte Sprache übersetzt, *sofern diese Sprache unterstützt wird*.
* **Standardsprache der Richtlinie**: Wenn vom Browser keine oder eine nicht unterstützte Sprache angegeben wird, wird der Benutzerflow in die Standardsprache der Richtlinie übersetzt.

> [!NOTE]
> Falls Sie benutzerdefinierte Benutzerattribute verwenden, müssen Sie Ihre eigenen Übersetzungen angeben. Weitere Informationen finden Sie unter [Anpassen von Zeichenfolgen](#customize-your-strings).

## <a name="support-requested-languages-for-ui_locales"></a>Unterstützen von angeforderten Sprachen für „ui_locales“

Bei Richtlinien, die vor der allgemeinen Verfügbarkeit der Sprachanpassung erstellt wurden, muss das Feature zunächst aktiviert werden. Bei später erstellten Richtlinien und Benutzerflows ist die Sprachanpassung standardmäßig aktiviert.

Wenn Sie die Sprachanpassung für einen Benutzerflow aktivieren, können Sie die Sprache des Benutzerflows durch Hinzufügen des Parameters `ui_locales` steuern.

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten **Benutzerflows** aus.
1. Klicken Sie auf den Benutzerflow, den Sie für Übersetzungen aktivieren möchten.
1. Wählen Sie **Sprachen** aus.
1. Klicken Sie auf **Sprachanpassung aktivieren**.

## <a name="select-which-languages-in-your-user-flow-are-enabled"></a>Auswählen der im Benutzerflow zu aktivierenden Sprachen

Aktivieren Sie einen Satz an Sprachen, in die Ihr Benutzerflow übersetzt werden soll, wenn der Browser die Anforderung ohne den `ui_locales`-Parameter stellt.

1. Stellen Sie anhand der oben genannten Anweisungen sicher, dass die Sprachanpassung für Ihren Benutzerflow aktiviert ist.
1. Wählen Sie auf der Seite **Sprachen** für den Benutzerflow eine Sprache aus, die Sie unterstützen möchten.
1. Legen Sie im Bereich „Eigenschaften“ die Option **Aktiviert** auf **Ja** fest.
1. Klicken Sie oben im Bereich „Eigenschaften“ auf **Speichern**.

>[!NOTE]
>Ohne Angabe des Parameters `ui_locales` wird die Seite nur in die Browsersprache des Kunden übersetzt, wenn sie aktiviert ist.
>

## <a name="customize-your-strings"></a>Anpassen von Zeichenfolgen

Mit der Sprachanpassung können Sie alle Zeichenfolgen Ihres Benutzerflows anpassen.

1. Stellen Sie anhand der oben genannten Anweisungen sicher, dass die Sprachanpassung für Ihren Benutzerflow aktiviert ist.
1. Wählen Sie auf der Seite **Sprachen** für den Benutzerflow die Sprache aus, die Sie anpassen möchten.
1. Wählen Sie unter **Ressourcendateien auf Seitenebene** die Seite aus, die Sie bearbeiten möchten.
1. Klicken Sie auf **Standardeinstellungen herunterladen** (oder **Außerkraftsetzungen herunterladen**, wenn Sie diese Sprache zuvor bearbeitet haben).

Bei diesen Schritten erhalten Sie eine JSON-Datei, die Sie nutzen können, um mit dem Bearbeiten Ihrer Zeichenfolgen zu beginnen.

### <a name="change-any-string-on-the-page"></a>Ändern einer beliebigen Zeichenfolge auf der Seite

1. Öffnen Sie die JSON-Datei, die Sie im Rahmen der obigen Anleitung heruntergeladen haben, in einem JSON-Editor.
1. Suchen Sie nach dem Element, das Sie ändern möchten. Sie können nach der `StringId` der gewünschten Zeichenfolge oder nach dem Attribut `Value` suchen, das Sie ändern möchten.
1. Aktualisieren Sie das `Value`-Attribut mit den Daten, die angezeigt werden sollen.
1. Ändern Sie `Override` für jede Zeichenfolge, die Sie ändern möchten, in `true`.
1. Speichern Sie die Datei, und laden Sie Ihre Änderungen hoch. (Das Steuerelement zum Hochladen befindet sich dort, wo Sie auch die JSON-Datei heruntergeladen haben.)

> [!IMPORTANT]
> Stellen Sie beim Überschreiben einer Zeichenfolge sicher, dass Sie den Wert für `Override` auf `true` festlegen. Wenn der Wert nicht geändert wird, wird der Eintrag ignoriert.

### <a name="change-extension-attributes"></a>Ändern von Erweiterungsattributen

Wenn Sie die Zeichenfolge für ein benutzerdefiniertes Benutzerattribut ändern oder dem JSON-Code eine Zeichenfolge hinzufügen möchten, verwenden Sie das folgende Format:

```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Ersetzen Sie `<ExtensionAttribute>` durch den Namen Ihres benutzerdefinierten Benutzerattributs.

Ersetzen Sie `<ExtensionAttributeValue>` durch die neue Zeichenfolge, die angezeigt werden soll.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Angeben einer Liste mit Werten mithilfe von „LocalizedCollections“

Wenn Sie eine feste Liste mit Werten für Antworten bereitstellen möchten, müssen Sie ein Attribut vom Typ `LocalizedCollections` erstellen. Bei `LocalizedCollections` handelt es sich um ein Array mit Paaren aus `Name` und `Value`. Die Reihenfolge der Elemente ist dieselbe, in der sie angezeigt werden. Verwenden Sie zum Hinzufügen von `LocalizedCollections` das folgende Format:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType",
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId` ist das Benutzerattribut, für das dieses Attribut vom Typ `LocalizedCollections` eine Antwort darstellt.
* `Name` ist der Wert, der dem Benutzer angezeigt wird.
* `Value` wird im Anspruch zurückgegeben, wenn diese Option ausgewählt wird.

### <a name="upload-your-changes"></a>Hochladen von Änderungen

1. Kehren Sie nach dem Ändern der JSON-Datei wieder zu Ihrem B2C-Mandanten zurück.
1. Wählen Sie **Benutzerflows** aus, und klicken Sie auf den Benutzerflow, den Sie für Übersetzungen aktivieren möchten.
1. Wählen Sie **Sprachen** aus.
1. Wählen Sie die gewünschte Zielsprache für die Übersetzung aus.
1. Wählen Sie die Seite aus, auf der Sie Übersetzungen bereitstellen möchten.
1. Klicken Sie auf das Ordnersymbol, und wählen Sie die hochzuladende JSON-Datei aus.

Die Änderungen werden automatisch in Ihrem Benutzerflow gespeichert.

## <a name="customize-the-page-ui-by-using-language-customization"></a>Anpassen der Benutzeroberfläche der Seite mithilfe der Sprachanpassung

Es gibt zwei Möglichkeiten zum Lokalisieren Ihrer HTML-Inhalte. Eine davon ist die Aktivierung der [Sprachanpassung](active-directory-b2c-reference-language-customization.md). Wenn Sie dieses Feature aktivieren, kann Azure AD B2C den Open ID Connect-Parameter `ui-locales` an Ihren Endpunkt weiterleiten. Ihr Inhaltsserver kann diesen Parameter verwenden, um benutzerdefinierte HTML-Seiten bereitzustellen, die sprachspezifisch sind.

Alternativ können Sie auf der Grundlage des verwendeten Gebietsschemas Inhalte aus unterschiedlichen Quellen abrufen. In Ihrem CORS-fähigen Endpunkt können Sie eine Ordnerstruktur zum Hosten von Inhalten für bestimmte Sprachen einrichten. Wenn Sie den Platzhalterwert `{Culture:RFC5646}` verwenden, wird der passende Inhalt abgerufen. Ein Beispiel: Angenommen, Ihr benutzerdefinierter Seiten-URI sieht wie folgt aus:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```

Sie können die Seite in `fr` laden. HTML- und CSS-Inhalte der Seite werden dann aus der folgenden Quelle abgerufen:

```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-languages"></a>Hinzufügen benutzerdefinierter Sprachen

Sie können auch Sprachen hinzufügen, für die Microsoft aktuell keine Übersetzungen bereitstellt. Sie müssen in diesem Fall die Übersetzungen für alle Zeichenfolgen im Benutzerflow bereitstellen. Sprach- und Gebietsschema-Codes sind auf die begrenzt, die im ISO-Standard 639-1 festgelegt sind.

1. Wählen Sie in Ihrem Azure AD B2C-Mandanten **Benutzerflows** aus.
2. Klicken Sie auf den Benutzerflow, für den Sie benutzerdefinierte Sprachen hinzufügen möchten, und klicken Sie dann auf **Sprachen**.
3. Wählen Sie im oberen Bereich der Seite **Benutzerdefinierte Sprache hinzuzufügen** aus.
4. Geben Sie im Kontextbereich mithilfe eines gültigen Gebietsschemacodes die Sprache an, für die Sie Übersetzungen bereitstellen.
5. Für jede Seite können Sie eine Reihe von Außerkraftsetzungen für Englisch herunterladen und Übersetzungen erstellen.
6. Wenn Sie mit der Bearbeitung der JSON-Dateien fertig sind, können Sie sie für die einzelnen Seiten hochladen.
7. Nachdem Sie auf **Aktivieren** geklickt haben, kann Ihr Benutzerflow diese Sprache für Ihre Benutzer anzeigen.
8. Speichern Sie die Sprache.

>[!IMPORTANT]
>Sie müssen benutzerdefinierte Sprachen aktivieren oder Außerkraftsetzungen dafür hochladen, bevor Sie speichern können.

## <a name="additional-information"></a>Zusätzliche Informationen

### <a name="page-ui-customization-labels-as-overrides"></a>Bezeichnungen der Seite für die Benutzeroberflächenanpassung als Außerkraftsetzungen

Wenn Sie die Sprachanpassung aktivieren, werden Ihre bisherigen Bearbeitungen für Bezeichnungen, die Sie auf der Seite für die Benutzeroberflächenanpassung vorgenommen haben, in einer JSON-Datei für Englisch (en) gespeichert. Sie können Ihre Bezeichnungen und anderen Zeichenfolgen weiterhin ändern, indem Sie Sprachressourcen in die Sprachanpassung hochladen.

### <a name="up-to-date-translations"></a>Aktuelle Übersetzungen

Microsoft ist bemüht, Ihnen möglichst aktuelle Übersetzungen zur Verfügung zu stellen. Die Übersetzungen werden kontinuierlich optimiert und auf ihre Konformität überprüft. Microsoft ermittelt Fehler und Änderungen für die globale Terminologie und nimmt geeignete Aktualisierungen in Ihrem Benutzerflow vor.

### <a name="support-for-right-to-left-languages"></a>Unterstützung für von rechts nach links geschriebene Sprachen

Von rechts nach links geschriebene Sprachen werden von Microsoft derzeit nicht unterstützt. Dies erreichen Sie, indem Sie benutzerdefinierte Gebietsschemas verwenden und mithilfe von CSS ändern, wie Zeichenfolgen angezeigt werden. Sollten Sie dieses Feature benötigen, stimmen Sie im [Azure-Feedback](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag) dafür ab.

### <a name="social-identity-provider-translations"></a>Übersetzungen für den Fall „Soziales Netzwerk als Identitätsanbieter“

Für Anmeldungen per sozialem Netzwerk stellt Microsoft den OIDC-Parameter `ui_locales` bereit. Einige soziale Netzwerke, die als Identitätsanbieter fungieren, erkennen diesen jedoch nicht an. Dazu zählen auch Facebook und Google.

### <a name="browser-behavior"></a>Browserverhalten

Von Chrome und Firefox wird jeweils die eigene festgelegte Sprache angefordert. Wenn es sich dabei um eine unterstützte Sprache handelt, wird sie vor der Standardsprache angezeigt. Für Microsoft Edge wird derzeit keine Sprache angefordert, sondern es wird gleich die Standardsprache verwendet.

## <a name="supported-languages"></a>Unterstützte Sprachen

Azure AD B2C bietet Unterstützung für die folgenden Sprachen. Sprachen für Benutzerflows werden von Azure AD B2C bereitgestellt. Die Sprachen für Benachrichtigungen von Multi-Factor Authentication (MFA) werden von [Azure MFA](../active-directory/authentication/concept-mfa-howitworks.md) bereitgestellt.

| Sprache              | Sprachcode | Benutzerabläufe         | MFA-Benachrichtigungen  |
|-----------------------| :-----------: | :----------------: | :----------------: |
| Arabisch                | ar            | :x:                | :heavy_check_mark: |
| Bulgarisch             | bg            | :x:                | :heavy_check_mark: |
| Bengalisch                | bn            | :heavy_check_mark: | :x:                |
| Katalanisch               | ca            | :x:                | :heavy_check_mark: |
| Tschechisch                 | cs            | :heavy_check_mark: | :heavy_check_mark: |
| Dänisch                | da            | :heavy_check_mark: | :heavy_check_mark: |
| Deutsch                | de            | :heavy_check_mark: | :heavy_check_mark: |
| Griechisch                 | el            | :heavy_check_mark: | :heavy_check_mark: |
| Englisch               | en            | :heavy_check_mark: | :heavy_check_mark: |
| Spanisch               | es            | :heavy_check_mark: | :heavy_check_mark: |
| Estnisch              | et            | :x:                | :heavy_check_mark: |
| Baskisch                | eu            | :x:                | :heavy_check_mark: |
| Finnisch               | fi            | :heavy_check_mark: | :heavy_check_mark: |
| Französisch                | fr            | :heavy_check_mark: | :heavy_check_mark: |
| Galizisch              | gl            | :x:                | :heavy_check_mark: |
| Gujarati              | gu            | :heavy_check_mark: | :x:                |
| Hebräisch                | he            | :x:                | :heavy_check_mark: |
| Hindi                 | hi            | :heavy_check_mark: | :heavy_check_mark: |
| Kroatisch              | hr            | :heavy_check_mark: | :heavy_check_mark: |
| Ungarisch             | hu            | :heavy_check_mark: | :heavy_check_mark: |
| Indonesisch            | id            | :x:                | :heavy_check_mark: |
| Italienisch               | it            | :heavy_check_mark: | :heavy_check_mark: |
| Japanisch              | ja            | :heavy_check_mark: | :heavy_check_mark: |
| Kasachisch                | kk            | :x:                | :heavy_check_mark: |
| Kannada               | kn            | :heavy_check_mark: | :x:                |
| Koreanisch                | ko            | :heavy_check_mark: | :heavy_check_mark: |
| Litauisch            | lt            | :x:                | :heavy_check_mark: |
| Lettisch               | lv            | :x:                | :heavy_check_mark: |
| Malayalam             | ml            | :heavy_check_mark: | :x:                |
| Marathi               | mr            | :heavy_check_mark: | :x:                |
| Malaiisch                 | ms            | :heavy_check_mark: | :heavy_check_mark: |
| Norwegisch Bokmål      | nb            | :heavy_check_mark: | :x:                |
| Niederländisch                 | nl            | :heavy_check_mark: | :heavy_check_mark: |
| Norwegisch             | no            | :x:                | :heavy_check_mark: |
| Pandschabi               | pa            | :heavy_check_mark: | :x:                |
| Polnisch                | pl            | :heavy_check_mark: | :heavy_check_mark: |
| Portugiesisch (Brasilien)   | pt-br         | :heavy_check_mark: | :heavy_check_mark: |
| Portugiesisch (Portugal) | pt-pt         | :heavy_check_mark: | :heavy_check_mark: |
| Rumänisch              | ro            | :heavy_check_mark: | :heavy_check_mark: |
| Russisch               | ru            | :heavy_check_mark: | :heavy_check_mark: |
| Slowakisch                | sk            | :heavy_check_mark: | :heavy_check_mark: |
| Slowenisch             | sl            | :x:                | :heavy_check_mark: |
| Serbisch (Kyrillisch)    | sr-cryl-cs    | :x:                | :heavy_check_mark: |
| Serbisch (Lateinisch)       | sr-latn-cs    | :x:                | :heavy_check_mark: |
| Schwedisch               | sv            | :heavy_check_mark: | :heavy_check_mark: |
| Tamilisch                 | ta            | :heavy_check_mark: | :x:                |
| Telugu                | te            | :heavy_check_mark: | :x:                |
| Thailändisch                  | th            | :heavy_check_mark: | :heavy_check_mark: |
| Türkisch               | tr            | :heavy_check_mark: | :heavy_check_mark: |
| Ukrainisch             | uk            | :x:                | :heavy_check_mark: |
| Vietnamesisch            | vi            | :x:                | :heavy_check_mark: |
| Chinesisch (vereinfacht)  | zh-hans       | :heavy_check_mark: | :heavy_check_mark: |
| Chinesisch (traditionell) | zh-hant       | :heavy_check_mark: | :heavy_check_mark: |
