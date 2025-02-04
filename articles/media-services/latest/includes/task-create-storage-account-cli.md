---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: f0d0322f6f5f14b94a67285fe8688d72c941b3a4
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105105839"
---
<!-- ### Create a storage account -->

Lorsque vous créez un compte Media Services, vous devez indiquer le nom d’une ressource de compte de stockage Azure. Le compte de stockage spécifié est lié à votre compte Media Services. Pour plus d’informations sur l’utilisation des comptes de stockage dans Media Services, consultez [Comptes de stockage](../storage-account-concept.md).

Vous devez avoir un compte de stockage **principal** et vous pouvez avoir n’importe quel nombre de comptes de stockage **secondaires** associés à votre compte Media Services. Media Services prend en charge les comptes **v2 à usage général** (GPv2) ou **v1 à usage général** (GPv1). Les comptes Blob uniquement ne sont pas autorisés en tant que comptes **principaux**. Pour plus d’informations sur les comptes de stockage, consultez [Options du compte de stockage Azure](../../../storage/common/storage-account-overview.md). 

Dans cet exemple, nous créons un compte v2 universel, LRS standard. Si vous voulez faire des expériences avec des comptes de stockage, utilisez `--sku Standard_LRS`. Cependant, lors de la sélection d’une référence SKU pour la production, envisagez `--sku Standard_RAGRS`, qui offre la réplication géographique pour la continuité de l’activité. Pour plus d’informations, consultez [Comptes de stockage](/cli/azure/storage/account).

La commande suivante crée un compte de stockage qui sera associé au compte Media Services. Dans le script ci-dessous, remplacez `storageaccountforams` par votre propre nom unique avec une longueur inférieure à 24 caractères. `amsResourceGroup` doit correspondre à la valeur que vous avez donnée au groupe de ressources à l’étape précédente.

```azurecli
az storage account create --name storageaccountforams --kind StorageV2 --sku Standard_LRS -l westus2 -g amsResourceGroup
```
