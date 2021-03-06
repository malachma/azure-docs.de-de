---
title: Bereitstellen in Azure Functions mit dem Jenkins-Azure Functions-Plug-In
description: Es wird beschrieben, wie Sie in Azure Functions mit dem Jenkins-Azure Functions-Plug-In Bereitstellungen durchführen.
ms.service: jenkins
keywords: Jenkins, Azure, DevOps, Java, Azure Functions
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 02/23/2019
ms.openlocfilehash: 1ee5a8d5f55422c9f8a0f20f3c6eb039f080dc2d
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68815738"
---
# <a name="deploy-to-azure-functions-using-the-jenkins-azure-functions-plugin"></a>Bereitstellen in Azure Functions mit dem Jenkins-Azure Functions-Plug-In

[Azure Functions](/azure/azure-functions/) ist ein serverloser Computedienst. Mit Azure Functions können Sie Code bedarfsgesteuert ausführen, ohne dass eine Infrastruktur für die Bereitstellung oder Verwaltung verwendet werden muss. In diesem Tutorial wird beschrieben, wie Sie eine Java-Funktion für Azure Functions mit dem Azure Functions-Plug-In bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

- **Azure-Abonnement**: Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.
- **Jenkins-Server**: Wenn Sie keinen Jenkins-Server installiert haben, hilft Ihnen der Artikel [Erstellen eines Jenkins-Servers in Azure](./install-jenkins-solution-template.md) weiter.

  > [!TIP]
  > Den Quellcode für dieses Tutorial finden Sie im [Visual Studio China-GitHub-Repository](https://github.com/VSChina/odd-or-even-function/blob/master/src/main/java/com/microsoft/azure/Function.java).

## <a name="create-a-java-function"></a>Erstellen einer Java-Funktion

Verwenden Sie zum Erstellen einer Java-Funktion mit dem Java-Runtimestapel entweder das [Azure-Portal](https://portal.azure.com) oder die [Azure CLI](/cli/azure/?view=azure-cli-latest).

Die folgenden Schritte veranschaulichen, wie Sie mit der Azure CLI eine Java-Funktion erstellen:

1. Erstellen Sie eine Ressourcengruppe, und ersetzen Sie den Platzhalter **&lt;resource_group>** durch den Namen Ihrer Ressourcengruppe.

    ```cli
    az group create --name <resource_group> --location eastus
    ```

1. Erstellen Sie ein Azure-Speicherkonto, und ersetzen Sie die Platzhalter durch die entsprechenden Werte.
 
    ```cli
    az storage account create --name <storage_account> --location eastus --resource-group <resource_group> --sku Standard_LRS    
    ```

1. Erstellen Sie die Test-Funktions-App, und ersetzen Sie die Platzhalter durch die entsprechenden Werte.

    ```cli
    az functionapp create --resource-group <resource_group> --consumption-plan-location eastus --name <function_app> --storage-account <storage_account>
    ```

## <a name="prepare-jenkins-server"></a>Vorbereiten des Jenkins-Servers

In den folgenden Schritten wird beschrieben, wie Sie den Jenkins-Server vorbereiten:

1. Stellen Sie einen [Jenkins-Server](https://aka.ms/jenkins-on-azure) bereit. Wenn Sie noch keine Instanz des installierten Jenkins-Servers installiert haben, hilft Ihnen die Anleitung im Artikel zum Thema [Erstellen eines Jenkins-Servers in Azure](./install-jenkins-solution-template.md) weiter.

1. Melden Sie sich per SSH an der Jenkins-Instanz an.

1. Installieren Sie auf der Jenkins-Instanz Maven, indem Sie den folgenden Befehl verwenden:

    ```terminal
    sudo apt install -y maven
    ```

1. Installieren Sie auf der Jenkins-Instanz die [Azure Functions Core Tools](/azure/azure-functions/functions-run-local), indem Sie an einer Eingabeaufforderung des Terminals die folgenden Befehle ausführen:

    ```terminal
    wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
    sudo dpkg -i packages-microsoft-prod.deb
    sudo apt-get update
    sudo apt-get install azure-functions-core-tools
    ```

1. Installieren Sie im Jenkins-Dashboard die folgenden Plug-Ins:

    - Azure Functions-Plug-In
    - EnvInject-Plug-In

1. Jenkins benötigt einen Azure-Dienstprinzipal zum Authentifizieren und Zugreifen auf Azure-Ressourcen. Eine Schritt-für-Schritt-Anleitung finden Sie unter [Tutorial: Bereitstellen über GitHub in Azure App Service mit Continuous Integration und Continuous Deployment von Jenkins](./tutorial-jenkins-deploy-web-app-azure-app-service.md).

1. Fügen Sie mit dem Azure-Dienstprinzipal in Jenkins den Typ „Microsoft Azure-Dienstprinzipal“ für Anmeldeinformationen hinzu. Informationen hierzu finden Sie im oben angegebenen Tutorial unter [Hinzufügen des Dienstprinzipals zu Jenkins](./tutorial-jenkins-deploy-web-app-azure-app-service.md#add-service-principal-to-jenkins).

## <a name="fork-the-sample-github-repo"></a>Forken des GitHub-Beispielrepositorys

1. [Melden Sie sich beim GitHub-Repository für die Beispiel-App für „ungerade“ oder „gerade“ an](https://github.com/VSChina/odd-or-even-function.git).

1. Wählen Sie in GitHub in der rechten oberen Ecke **Fork** aus.

1. Folgen Sie den Anweisungen, um Ihr GitHub-Konto auszuwählen und das Forken abzuschließen.

## <a name="create-a-jenkins-pipeline"></a>Erstellen einer Jenkins-Pipeline

In diesem Abschnitt erstellen Sie die [Jenkins-Pipeline](https://jenkins.io/doc/book/pipeline/).

1. Erstellen Sie im Jenkins-Dashboard eine Pipeline.

1. Aktivieren Sie **Prepare an environment for the run** (Umgebung für die Ausführung vorbereiten).

1. Fügen Sie unter **Properties Content** (Eigenschafteninhalt) die folgenden Umgebungsvariablen hinzu, und ersetzen Sie die Platzhalter durch die entsprechenden Werte für Ihre Umgebung:

    ```
    AZURE_CRED_ID=<service_principal_credential_id>
    RES_GROUP=<resource_group>
    FUNCTION_NAME=<function_name>
    ```
    
1. Wählen Sie im Abschnitt **Pipeline > Definition** die Option **Pipeline script from SCM** (Pipeline-Skript von SCM).

1. Geben Sie die URL Ihres GitHub-Forks und den Skriptpfad („doc/resources/jenkins/JenkinsFile“) für die Verwendung im [JenkinsFile-Beispiel](https://github.com/VSChina/odd-or-even-function/blob/master/doc/resources/jenkins/JenkinsFile) ein.

   ```
   node {
    stage('Init') {
        checkout scm
        }

    stage('Build') {
        sh 'mvn clean package'
        }

    stage('Publish') {
        azureFunctionAppPublish appName: env.FUNCTION_NAME, 
                                azureCredentialsId: env.AZURE_CRED_ID, 
                                filePath: '**/*.json,**/*.jar,bin/*,HttpTrigger-Java/*', 
                                resourceGroup: env.RES_GROUP, 
                                sourceDirectory: 'target/azure-functions/odd-or-even-function-sample'
        }
    }
    ```

## <a name="build-and-deploy"></a>Erstellen und Bereitstellen

Nun kann der Jenkins-Auftrag ausgeführt werden.

1. Rufen Sie zuerst den Autorisierungsschlüssel ab, indem Sie die Anleitung im Artikel [HTTP-Trigger und -Bindungen in Azure Functions](/azure/azure-functions/functions-bindings-http-webhook#authorization-keys) verwenden.

1. Geben Sie in Ihrem Browser die URL der App ein. Ersetzen Sie die Platzhalter durch die entsprechenden Werte, und geben Sie einen numerischen Wert für **&lt;input_number>** als Eingabe für die Java-Funktion an.

    ```
    https://<function_app>.azurewebsites.net/api/HttpTrigger-Java?code=<authorization_key>&number=<input_number>
    ```
1. Es werden Ergebnisse angezeigt, die der folgenden Beispielausgabe ähneln (zu Testzwecken wurde die ungerade Zahl 365 verwendet):

    ```output
    The number 365 is Odd.
    ```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Falls Sie diese Anwendung nicht weiterverwenden möchten, sollten Sie die von Ihnen erstellten Ressourcen wie folgt löschen:

```cli
az group delete -y --no-wait -n <resource_group>
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure Functions finden Sie unter:
> [!div class="nextstepaction"]
> [Dokumentation zu Azure Functions](/azure/azure-functions/)
