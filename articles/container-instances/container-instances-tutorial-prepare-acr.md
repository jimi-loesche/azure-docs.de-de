---
title: "Tutorial für Azure Container Instances – Vorbereiten der Azure Container Registry | Microsoft-Dokumentation"
description: "Tutorial für Azure Container Instances – Vorbereiten der Azure Container Registry"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Container, Microservices, Kubernetes, DC/OS, Azure
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 7ac85bffb9593923808c77f2240e6f0e841e74cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Bereitstellen und Verwenden von Azure Container Registry

Dies ist der zweite Teil eines dreiteiligen Tutorials. Im [vorherigen Schritt](./container-instances-tutorial-prepare-app.md) wurde ein Containerimage für eine einfache in [Node.js](http://nodejs.org) geschriebene Webanwendung erstellt. In diesem Tutorial wird dieses Image per Push in eine Azure Container Registry-Instanz übertragen. Wenn Sie das Containerimage noch nicht erstellt haben, kehren Sie zu [Tutorial 1 – Erstellen von Containerimages](./container-instances-tutorial-prepare-app.md) zurück. 

Azure Container Registry ist eine Azure-basierte, private Registrierung für Docker-Containerimages. In diesem Tutorial werden die Schritte für die Bereitstellung einer Azure Container Registry-Instanz und die Übertragung eines Containerimages in diese Instanz per Push erläutert. Folgende Schritte werden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen einer Azure Container Registry-Instanz
> * Markieren eines Containerimages für Azure Container Registry
> * Hochladen eines Images in Azure Container Registry

In nachfolgenden Tutorials stellen Sie den Container aus Ihrer privaten Registrierung für Azure Container Instances bereit.

## <a name="before-you-begin"></a>Voraussetzungen

Für dieses Tutorial müssen Sie mindestens Version 2.0.12 der Azure-Befehlszeilenschnittstelle ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu.

## <a name="deploy-azure-container-registry"></a>Bereitstellen von Azure Container Registry

Für die Bereitstellung einer Azure Container Registry-Instanz benötigen Sie zunächst eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist eine logische Sammlung, in der Azure-Ressourcen bereitgestellt und verwaltet werden.

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *eastus* erstellt.

```azurecli
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mit dem Befehl [az acr create](/cli/azure/acr#create) eine Azure Container Registry-Instanz. Der Name einer Containerregistrierung **muss eindeutig sein**. Im folgenden Beispiel verwenden wir den Namen *mycontainerregistry082*.

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Für den Rest dieses Tutorials verwenden wir `<acrname>` als Platzhalter für den von Ihnen gewählten Namen der Containerregistrierung.

## <a name="container-registry-login"></a>Anmeldung bei der Containerregistrierung

Sie müssen sich bei der ACR-Instanz anmelden, damit Sie Images per Push in sie übertragen können. Verwenden Sie den Befehl [az acr login](https://docs.microsoft.com/en-us/cli/azure/acr#az_acr_login), um den Vorgang abzuschließen. Sie müssen den eindeutigen Namen angeben, mit dem die Containerregistrierung bei ihrer Erstellung versehen wurde.

```azurecli
az acr login --name <acrName>
```

Nach Abschluss des Vorgangs wird eine Erfolgsmeldung zurückgegeben.

## <a name="tag-container-image"></a>Markieren von Containerimages

Um ein Containerimage aus einer privaten Registrierung bereitzustellen, muss das Image mit dem `loginServer`-Namen der Registrierung markiert werden.

Verwenden Sie den Befehl `docker images`, um eine Liste der aktuellen Images anzuzeigen.

```bash
docker images
```

Ausgabe:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Um den „loginServer“-Namen zu erhalten, führen Sie den folgenden Befehl aus.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Kennzeichnen Sie das Image *aci-tutorial-app* mit dem loginServer-Namen der Containerregistrierung. Fügen Sie zudem `:v1` am Ende des Imagenamens hinzu. Dieses Tag gibt die Imageversionsnummer an.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Führen Sie nach der Kennzeichnung den Befehl `docker images` aus, um den Vorgang zu überprüfen.

```bash
docker images
```

Ausgabe:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Übertragen des Images zu Azure Container Registry mithilfe von Push

Übertragen Sie das Image *aci-tutorial-app* per Push in die Registrierung.

Verwenden Sie das folgende Beispiel, und ersetzen Sie den loginServer-Namen der Containerregistrierung durch den loginServer-Namen aus Ihrer Umgebung.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

## <a name="list-images-in-azure-container-registry"></a>Auflisten der Images in Azure Container Registry

Führen Sie den Befehl [az acr repository list](/cli/azure/acr/repository#list) aus, um eine Liste der Images zurückzugeben, die per Push in Ihre Azure Container Registry-Instanz übertragen wurden. Aktualisieren Sie den Befehl mit dem Namen der Containerregistrierung.

```azurecli
az acr repository list --name <acrName> --output table
```

Ausgabe:

```azurecli
Result
----------------
aci-tutorial-app
```

Verwenden Sie dann den Befehl [az acr repository show-tags](/cli/azure/acr/repository#show-tags), um die Tags für ein bestimmtes Image anzuzeigen.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Ausgabe:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial wurde eine Azure Container Registry für die Verwendung mit Azure Container Instances vorbereitet, und das Containerimage wurde per Push übertragen. Die folgenden Schritte wurden ausgeführt:

> [!div class="checklist"]
> * Bereitstellen einer Azure Container Registry-Instanz
> * Markieren eines Containerimages für Azure Container Registry
> * Hochladen eines Images in Azure Container Registry

Fahren Sie mit dem nächsten Tutorial fort, um mehr über die Bereitstellung des Containers in Azure mithilfe von Azure Container Instances zu erfahren.

> [!div class="nextstepaction"]
> [Bereitstellen von Containern in Azure Container Instances](./container-instances-tutorial-deploy-app.md)
