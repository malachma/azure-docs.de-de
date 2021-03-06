---
title: Automatisieren des Erstellens und Patchens von Containerimages mit Azure Container Registry Tasks (ACR Tasks)
description: 'Einführung in ACR Tasks: eine Suite mit Features in Azure Container Registry für sichere, automatisierte Build- und Patchvorgänge für Containerimages und Verwaltung in der Cloud.'
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 09/05/2019
ms.author: danlep
ms.openlocfilehash: c62987031a73aa4840c1d036689a3c52fb4dc4a0
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2019
ms.locfileid: "70914663"
---
# <a name="automate-container-image-builds-and-maintenance-with-acr-tasks"></a>Automatisieren von Containerimage-Builds und Wartung mit ACR Tasks

Container bieten neue Virtualisierungsmöglichkeiten und trennen Anwendungs- und Entwicklerabhängigkeiten von Infrastruktur- und Betriebsanforderungen. Ein Aspekt, der allerdings weiterhin behandelt werden muss, ist das das Verwalten und Patchen dieser Anwendungsvirtualisierung im Laufe des Containerlebenszyklus.

## <a name="what-is-acr-tasks"></a>Was ist ACR Tasks?

**ACR Tasks** ist eine Suite von Funktionen in Azure Container Registry. Sie bietet cloudbasierte Containerimageerstellung für [Plattformen](#image-platforms) inklusive Linux, Windows und ARM. Außerdem kann sie [Betriebssystem- und Frameworkpatching](#automate-os-and-framework-patching) für Ihre Docker-Container automatisieren. ACR Tasks erweitert nicht nur Ihren „inneren“ Entwicklungszyklus für die Cloud mit bedarfsgesteuerten Containerimagebuilds, sondern ermöglicht auch automatisierte Builds, die durch Quellcodeupdates, Updates des Basisimages eines Containers oder Timer aktualisiert werden. Mit Triggern für Basisimageaktualisierungen können Sie z. B. Ihren Workflow für Betriebssystem- und Anwendungspatches automatisieren und so die Sicherheit von Umgebungen gewährleisten sowie die Prinzipien unveränderlicher Container einhalten.

## <a name="task-scenarios"></a>Taskszenarien

ACR Tasks unterstützt verschiedene Szenarien zum Erstellen und Verwalten von Containerimages und anderen Artefakten. Weitere Informationen finden Sie in den folgenden Abschnitten dieses Artikels.

* **[Schnelltask](#quick-task)** : Bedarfsgesteuertes Erstellen und Pushen eines einzelnen Containerimages in eine Containerregistrierung in Azure ohne lokale Docker Engine-Installation. Kurz: `docker build`, `docker push` in die Cloud.
* **Automatisch ausgelöste Tasks**: Aktivieren Sie einen oder mehrere *Trigger*, um ein Image zu erstellen:
  * **[Auslösen des Tasks beim Update des Quellcodes](#trigger-task-on-source-code-update)** 
  * **[Auslösen beim Basisimageupdate](#automate-os-and-framework-patching)** 
  * **[Auslösen nach Zeitplan](#schedule-a-task)** 
* **[Task mit mehreren Schritten](#multi-step-tasks)** : Erweitern der ACR Tasks-Funktion zum Erstellen und Pushen einzelner Images mit auf mehreren Containern basierenden Workflows mit mehreren Schritten. 

Jeder ACR Tasks-Instanz ist ein [Quellcodekontext](#context-locations) zugeordnet: der Speicherort eines Satzes von Quelldateien, der zum Erstellen eines Containerimages oder eines anderen Artefakts verwendet wird. Beispielkontexte sind ein Git-Repository oder ein lokales Dateisystem.

Tasks können auch [Laufzeitvariablen](container-registry-tasks-reference-yaml.md#run-variables) nutzen, sodass Sie Taskdefinitionen wiederverwenden und Tags für Images und Artefakte standardisieren können.

## <a name="quick-task"></a>Schnelltask

Der „interne“ Entwicklungszyklus, der iterative Prozess, der das Schreiben von Code sowie das Erstellen und Testen Ihrer Anwendung umfasst, bevor sie zur Quellcodeverwaltung committet wird, ist wirklich der Beginn der Lebenszyklusverwaltung des Containers.

Bevor Sie Ihre erste Zeile Code committen, kann das Feature [Schnelltask](container-registry-tutorial-quick-task.md) von ACR Tasks eine integrierte Entwicklungsumgebung bereitstellen, indem Ihre Containerimage-Buildvorgänge in Azure ausgelagert werden. Mit Schnelltasks können Sie Ihre automatisierten Builddefinitionen überprüfen und potenzielle Probleme abfangen, bevor Sie Ihren Code committen.

Der Befehl [az acr build][az-acr-build] der Azure CLI verwendet das vertraute `docker build`-Format. Er akzeptiert einen [Kontext](#context-locations) (Satz zu erstellender Dateien), sendet ihn an ACR Tasks und pusht das erstellte Image nach dessen Fertigstellung standardmäßig in die entsprechende Registrierung.

Eine Einführung finden Sie im Schnellstart: [Erstellen und Ausführen eines Containerimages](container-registry-quickstart-task-cli.md) in Azure Container Registry.  

ACR Tasks ist als Grundtyp für den Containerlebenszyklus konzipiert. Sie können ACR Tasks also beispielsweise in Ihre CI/CD-Lösung integrieren. Wenn Sie [az login][az-login] mit einem [Dienstprinzipal][az-login-service-principal] ausführen, kann Ihre CI/CD-Lösung mithilfe von Befehlen vom Typ [az acr build][az-acr-build] Imagebuildvorgänge initiieren.

Informationen zur Verwendung der Schnelltasks finden Sie im ersten ACR Tasks-Tutorial: [Erstellen von Containerimages in der Cloud mit Azure Container Registry Tasks](container-registry-tutorial-quick-task.md).

> [!TIP]
> Wenn Sie ein Image direkt aus dem Quellcode ohne Dockerfile-Datei erstellen und pushen möchten, nutzen Sie den [az acr pack build][az-acr-pack-build]-Befehl (Vorschau) der Azure Container Registry. Mit diesem Tool wird ein Image aus dem Quellcode der Anwendung mithilfe von [Cloud Native Buildpacks](https://buildpacks.io/) erstellt und gepusht.

## <a name="trigger-task-on-source-code-update"></a>Auslösen des Tasks beim Update des Quellcodes

Lösen Sie einen Containerimage-Buildvorgang oder Task in mehreren Schritten aus, wenn Code in ein Git-Repository in GitHub oder Azure DevOps committet oder ein Pull Request erstellt oder aktualisiert wird. Konfigurieren Sie z. B. einen Buildtask mithilfe des Azure CLI-Befehls [az acr task create][az-acr-task-create], indem Sie ein Git-Repository und optional einen Branch und eine Dockerfile-Datei angeben. Wenn Ihr Team Code im Repository aktualisiert, löst ein von ACR Tasks erstellter Webhook einen Buildvorgang für das im Repository definierte Containerimage aus. 

ACR Tasks unterstützt die folgenden Trigger, wenn Sie ein Git-Repository als Taskkontext festlegen:

| Trigger | Standardmäßig aktiviert |
| ------- | ------------------ |
| Commit | Ja |
| Pull Request | Nein |

Um den Trigger zu konfigurieren, stellen Sie dem Task ein persönliches Zugriffstoken (Personal Access Token, PAT) zur Verfügung, um den Webhook im GitHub- oder Azure DevOps-Repository festzulegen.

Informationen zum Auslösen von Buildvorgängen nach dem Committen von Quellcode finden Sie im zweiten ACR Tasks-Tutorial: [Automatisieren von Buildvorgängen für Containerimages mit Azure Container Registry Tasks](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>Automatisierung von Betriebssystem- und Frameworkpatching

Durch die Möglichkeit zur Erkennung von Basisimageaktualisierungen bietet ACR Tasks einen echten Mehrwert für Ihren Containererstellungsworkflow. Wenn das aktualisierte Basisimage in Ihre Registrierung gepusht oder ein Basisimage in einem öffentlichen Repository. wie Docker Hub, aktualisiert wird, kann ACR Tasks automatisch alle darauf basierenden Anwendungsimages erstellen.

Containerimages lassen sich grob in *Basisimages* und *Anwendungsimages* unterteilen. Basisimages enthalten neben anderen Anpassungen in der Regel das Betriebssystem und die Anwendungsframeworks, auf denen Ihre Anwendung basiert. Diese Basisimages basieren üblicherweise auf öffentlichen Upstreamimages, beispielsweise [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet] oder [Node.js][base-node]. Ein Basisimage kann mehreren Anwendungsimages zugrunde liegen.

Wenn ein Betriebssystem- oder App-Framework-Image durch die zuständige Upstream-Instanz aktualisiert wird (etwa mit einem wichtigen Sicherheitspatch für das Betriebssystem), müssen Sie auch Ihre Basisimages aktualisieren, um die Korrektur zu integrieren. Daraufhin müssen auch alle Anwendungsimages neu erstellt werden, um die Upstreamkorrekturen aus Ihrem Basisimage zu integrieren.

Da Basisimageabhängigkeiten bei der Erstellung eines Containerimages von ACR Tasks dynamisch erkannt werden, kann ACR Tasks auch erkennen, dass das Basisimage eines Anwendungsimages aktualisiert wurde. Mit einer einzelnen vorkonfigurierten [Buildaufgabe](container-registry-tutorial-base-image-update.md#create-a-task) kann ACR Tasks dann **automatisch alle Anwendungsimages neu erstellen**. Die automatische Erkennung und Neuerstellung von ACR Tasks spart Zeit, die ansonsten für die manuelle Nachverfolgung und Aktualisierung der einzelnen Anwendungsimages aufgewendet werden müsste, die auf Ihr aktualisiertes Basisimage verweisen.

Für Imageerstellungen aus einer Dockerfile-Datei verfolgt ein ACR Task ein Basisimageupdate, wenn sich das Basisimage an einem der folgenden Speicherorte befindet:

* Dieselbe Azure-Containerregistrierung, in der der Task ausgeführt wird
* Eine andere Azure-Containerregistrierung in derselben Region 
* Ein öffentliches Repository in Docker Hub
* Ein öffentliches Repository in Microsoft Container Registry

> [!NOTE]
> * Der Basisimageupdate-Trigger ist standardmäßig in einer ACR Tasks-Instanz aktiviert. 
> * Derzeit verfolgt ACR Tasks nur Basisimageupdates für Anwendungsimages (*Runtime*) nach. ACR Tasks verfolgt keine Basisimageupdates für Zwischenimages (*Buildzeit*) nach, die in mehrstufigen Dockerfile-Dateien verwendet werden. 

Weitere Informationen zu Betriebssystem- und Frameworkpatches finden Sie im dritten ACR Tasks-Tutorial: [Automatisieren von Buildvorgängen für Images nach der Aktualisierung des Basisimages mit Azure Container Registry Tasks](container-registry-tutorial-base-image-update.md).

## <a name="schedule-a-task"></a>Planen eines Tasks

Planen Sie optional einen Task, indem Sie einen oder mehrere *Zeitgebertrigger* einrichten, wenn Sie den Task erstellen oder aktualisieren. Das Planen eines Tasks eignet sich zum Ausführen von Containerworkloads nach einem definierten Zeitplan oder zum Ausführen von Wartungsvorgängen oder Tests für Images, die regelmäßig in Ihre Registrierung gepusht werden. Weitere Informationen finden Sie unter [Ausführen einer ACR-Aufgabe nach einem definierten Zeitplan](container-registry-tasks-scheduled.md).

## <a name="multi-step-tasks"></a>Mehrstufige Aufgaben

Mehrstufige Aufgaben bieten schrittbasierte Aufgabendefinition und -ausführung für Erstellen, Testen und Patchen von Containerimages in der Cloud. In einer [YAML-Datei](container-registry-tasks-reference-yaml.md) definierte Taskschritte legen einzelne Build- und Pushvorgänge für Containerimages oder andere Artefakte fest. Sie können auch die Ausführung eines oder mehrerer Container mit jedem Schritt definieren, wobei der Container als Ausführungsumgebung verwendet wird.

Beispielsweise können Sie eine mehrstufige Aufgaben erstellen, die die folgenden Vorgänge automatisiert:

1. Erstellen eines Webanwendungsimages
1. Ausführen des Webanwendungscontainers
1. Erstellen eines Webanwendungs-Testimages
1. Ausführen des Webanwendungs-Testcontainers, der Tests für den aktuell ausgeführten Anwendungscontainer durchführt
1. Wenn die Tests erfolgreich sind: Erstellen eines Helm-Chart-Archivpakets
1. Ausführen eines `helm upgrade` mithilfe des neuen Helm-Diagramm-Archivpakets

Mit mehrstufigen Aufgaben können Sie das Erstellen, Ausführen und Testen eines Images in mehrere individuelle Schritte aufteilen, wobei Abhängigkeiten zwischen den Schritten unterstützt werden. Mit mehrstufigen Aufgaben in ACR Tasks erhalten Sie eine bessere Kontrolle über die Workflows zur Erstellung von Images, Tests, Betriebssystem- und Frameworkpatches.

Weitere Informationen zu mehrstufigen Aufgaben finden Sie unter [Ausführen von mehrstufigen Build-, Test- und Patchaufgaben in ACR Tasks](container-registry-tasks-multi-step.md).

## <a name="context-locations"></a>Kontextspeicherorte

Die folgende Tabelle zeigt einige Beispiele von unterstützten Kontextspeicherorten für ACR Tasks:

| Kontextspeicherort | BESCHREIBUNG | Beispiel |
| ---------------- | ----------- | ------- |
| Lokales Dateisystem | Dateien in einem Verzeichnis auf dem lokalen Dateisystem. | `/home/user/projects/myapp` |
| GitHub-Masterbranch | Dateien im Masterbranch (oder einem anderen Standardbranch) eines GitHub-Repositorys.  | `https://github.com/gituser/myapp-repo.git` |
| GitHub-Branch | Bestimmter Branch eines GitHub-Repositorys.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub-Unterordner | Dateien in einem Unterordner in einem GitHub-Repository. Das Beispiel zeigt die Kombination der Branch- und Unterordnerspezifikation. | `https://github.com/gituser/myapp-repo.git#mybranch:myfolder` |
| Remotetarball | Dateien in einem komprimierten Archiv auf einem Remotewebserver. | `http://remoteserver/myapp.tar.gz` |

## <a name="image-platforms"></a>Imageplattformen

ACR Tasks erstellt standardmäßig Images für das Linux-Betriebssystem und die amd64-Architektur. Geben Sie das Tag `--platform` an, um Windows-Images oder Linux-Images für andere Architekturen zu erstellen. Geben Sie das Betriebssystem und optional eine unterstützte Architektur im Format „Betriebssystem/Architektur“ an (z. B. `--platform Linux/arm`). Geben Sie bei ARM-Architekturen optional eine Variante im Format „Betriebssystem/Architektur/Variante“ an (z. B. `--platform Linux/arm64/v8`):

| OS | Architecture|
| --- | ------- | 
| Linux | amd64<br/>ARM<br/>arm64<br/>386 |
| Windows | amd64 |

## <a name="view-task-logs"></a>Anzeigen von Taskprotokollen

Bei jeder Aufgabenausführung wird eine Protokollausgabe erzeugt, die Sie überprüfen können, um festzustellen, ob die Aufgabenschritte erfolgreich ausgeführt wurden. Wenn Sie den Befehl [az acr build](/cli/azure/acr#az-acr-build), [az acr run](/cli/azure/acr#az-acr-run) oder [az acr task run](/cli/azure/acr/task#az-acr-task-run) verwenden, um die Aufgabe auszulösen, wird die Protokollausgabe für die Ausführung der Aufgabe an die Konsole gestreamt und auch für einen späteren Abruf gespeichert. Wenn ein Task automatisch ausgelöst wird, z. B. durch einen Quellcodecommit oder ein Basisimageupdate, werden die Aufgabenprotokolle nur gespeichert. Zeigen Sie die Protokolle für eine Aufgabenausführung im Azure-Portal an, oder verwenden Sie den Befehl [az acr task logs](/cli/azure/acr/task#az-acr-task-logs).

Standardmäßig werden Daten und Protokolle für Aufgabenausführungen 30 Tage in einer Registrierung aufbewahrt und danach automatisch endgültig gelöscht. Wenn Sie die Daten für eine Aufgabenausführung archivieren möchten, aktivieren Sie die Archivierung mit dem Befehl [az acr task update-run](/cli/azure/acr/task#az-acr-task-update-run). Das folgende Beispiel aktiviert die Archivierung der Aufgabenausführung *cf11* in der Registrierung *myregistry*.

```azurecli
az acr task update-run --registry myregistry --run-id cf11 --no-archive false
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie bereit sind, die Erstellung und Wartung von Containerimages in der Cloud zu automatisieren, informieren Sie sich in der [ACR Tasks-Tutorialreihe](container-registry-tutorial-quick-task.md).

Installieren Sie optional die [Docker-Erweiterung für Visual Studio Code](https://code.visualstudio.com/docs/azure/docker) und die [Azure-Kontoerweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) für die Verwendung mit Ihren Azure-Containerregistrierungen. In Visual Studio Code können Sie Pull- und Pushvorgänge für Images in einer Azure-Containerregistrierung oder auch ACR Tasks ausführen.

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
