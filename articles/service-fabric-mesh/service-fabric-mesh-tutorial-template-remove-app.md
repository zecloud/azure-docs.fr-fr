---
title: Tutoriel - Supprimer une application s’exécutant dans Azure Service Fabric Mesh
description: Dans ce tutoriel, vous allez découvrir comment enlever une application en cours d’exécution dans Service Fabric Mesh et supprimer des ressources.
author: georgewallace
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: gwallace
ms.custom: mvc, devcenter
ms.openlocfilehash: d449343fd00ff958470a71ecb3a37d585d7ff8ed
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99626609"
---
# <a name="tutorial-remove-an-application-and-resources"></a>Tutoriel : Supprimer une application et des ressources

> [!IMPORTANT]
> La préversion d’Azure Service Fabric Mesh a été mise hors service. Les nouveaux déploiements par le biais de l’API Service Fabric Mesh ne seront plus autorisés. La prise en charge des déploiements existants se poursuivra jusqu’au 28 avril 2021.
> 
> Pour plus détails, consultez [Mise hors service de la préversion d’Azure Service Fabric Mesh](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/).

Ce tutoriel est la quatrième partie de la série. Vous allez apprendre à supprimer une application en cours d’exécution qui a été [déployée sur Service Fabric Mesh](service-fabric-mesh-tutorial-template-deploy-app.md). 

Dans ce quatrième volet, vous apprenez à :

> [!div class="checklist"]
> * Supprimer une application en cours d’exécution dans Service Fabric Mesh
> * Supprimer les ressources d’application

Cette série de tutoriels vous montre comment effectuer les opérations suivantes :
> [!div class="checklist"]
> * [Déployer une application sur Service Fabric Mesh à l’aide d’un modèle](service-fabric-mesh-tutorial-template-deploy-app.md)
> * [Mettre à l’échelle une application en cours d’exécution dans Service Fabric Mesh](service-fabric-mesh-tutorial-template-scale-services.md)
> * [Mettre à niveau une application en cours d’exécution dans Service Fabric Mesh](service-fabric-mesh-tutorial-template-upgrade-app.md)
> * Supprimer une application

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Prérequis

Avant de commencer ce tutoriel :

* Si vous n’avez pas d’abonnement Azure, vous pouvez créer un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

* Ouvrez [Azure Cloud Shell](service-fabric-mesh-howto-setup-cli.md) ou [installez localement Azure CLI et l’interface CLI Service Fabric Mesh](service-fabric-mesh-howto-setup-cli.md#install-the-azure-service-fabric-mesh-cli).

## <a name="delete-the-resource-group-and-all-the-resources"></a>Supprimer le groupe de ressources et toutes les ressources

Lorsque vous n’en avez plus besoin, supprimez toutes les ressources créées. Vous avez déjà [créé un groupe de ressources](service-fabric-mesh-tutorial-template-deploy-app.md#create-a-container-registry) pour héberger l’instance Azure Container Registry et les ressources d’application Service Fabric Mesh.  Vous pouvez supprimer ce groupe de ressources, entraînant ainsi la suppression de toutes les ressources associées.

```azurecli
az group delete --resource-group myResourceGroup
```

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="individually-delete-the-resources"></a>Supprimer les ressources individuellement
Vous pouvez également supprimer l’instance ACR, l’application Service Fabric Mesh et les ressources réseau individuellement.

Pour supprimer l’instance ACR :

```azurecli
az acr delete --resource-group myResourceGroup --name myContainerRegistry
```

Pour supprimer l’application Service Fabric Mesh :

```azurecli
az mesh app delete --resource-group myResourceGroup --name todolistapp
```

Pour supprimer le réseau :
```azurecli
az mesh network delete --resource-group myResourceGroup --name todolistappNetwork
```

## <a name="next-steps"></a>Étapes suivantes

Dans cette partie du tutoriel, vous avez appris à :

> [!div class="checklist"]
> * Supprimer une application en cours d’exécution dans Service Fabric Mesh
> * Supprimer les ressources d’application
