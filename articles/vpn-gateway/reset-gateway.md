---
title: Réinitialiser une passerelle VPN ou une connexion pour rétablir un tunnel IPsec
titleSuffix: Azure VPN Gateway
description: Réinitialisez une connexion ou une passerelle VPN pour rétablir des tunnels IPsec.
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 02/22/2021
ms.author: cherylmc
ms.openlocfilehash: adc2ffd63d73baaddce00324787df61061ea69dc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101726635"
---
# <a name="reset-a-vpn-gateway-or-a-connection"></a>Réinitialiser une passerelle VPN ou une connexion

La réinitialisation d’une passerelle VPN ou d’une connexion de passerelle Azure est utile si vous perdez la connectivité VPN entre différents locaux sur un ou plusieurs tunnels VPN site à site. Dans ce cas, vos périphériques VPN sur site fonctionnent tous correctement, mais ils ne sont pas en mesure d’établir des tunnels IPsec avec les passerelles VPN Azure. Cet article vous aide à réinitialiser une passerelle VPN ou une connexion de passerelle.

## <a name="what-happens-during-a-reset"></a>Ce qui se passe pendant une réinitialisation

### <a name="gateway-reset"></a>Réinitialisation de passerelle

Une passerelle VPN se compose de deux instances de machines virtuelles s’exécutant dans une configuration active/de secours. Lorsque vous réinitialisez la passerelle, elle redémarre celle-ci et lui réapplique les configurations entre différents locaux. La passerelle conserve l’adresse IP publique qu’elle a déjà. Cela signifie que vous n’avez pas à mettre à jour la configuration du routeur VPN avec une nouvelle adresse IP publique pour la passerelle VPN Azure.

Quand vous émettez la commande pour réinitialiser la passerelle, l’instance actuellement active de la passerelle VPN Azure est immédiatement redémarrée. Un bref laps de temps s’écoule pendant le basculement de l’instance active (en cours de redémarrage) vers l’instance de secours. Cet intervalle doit être inférieur à une minute.

Si la connexion n’est pas restaurée après le premier redémarrage, exécutez de nouveau la commande pour redémarrer la deuxième instance de machine virtuelle (la nouvelle passerelle active). Si les deux redémarrages sont demandés à la suite, il s’écoulera un délai un peu plus long pendant lequel les deux instances de machine virtuelle machine virtuelles (active/veille) sont en cours de redémarrage. En conséquence, l’intervalle de connectivité VPN permettant aux machines virtuelles de terminer les redémarrages sera un peu plus long, jusqu’à 30 à 45 minutes.

Après deux redémarrages, si vous continuez de rencontrer des problèmes de connectivité entre différents locaux, ouvrez une demande de support à partir du portail Azure.

### <a name="connection-reset"></a>Réinitialisation de la connexion

Lorsque vous choisissez de réinitialiser une connexion, la passerelle ne redémarre pas. Seule la connexion sélectionnée est réinitialisée et restaurée.

## <a name="reset-a-connection"></a>Réinitialiser une connexion

Vous pouvez réinitialiser une connexion facilement à l’aide du portail Azure.

1. Accédez à la **Connexion** que vous souhaitez réinitialiser. Vous pouvez trouver la ressource de connexion en la localisant dans **Toutes les ressources**, ou en accédant à **« Nom de la passerelle » -> Connexions -> « nom de la connexion »**
1. Sur la page **Connexion**, sélectionnez **Réinitialiser** dans le menu de gauche.
1. Sur la page **Réinitialiser**, cliquez sur **Réinitialiser** pour réinitialiser la connexion.

   :::image type="content" source="./media/reset-gateway/reset-connection.png" alt-text="Capture d’écran montrant une réinitialisation.":::

## <a name="reset-a-vpn-gateway"></a>Réinitialiser une passerelle VPN

Avant de réinitialiser votre passerelle, vérifiez les éléments clés répertoriés ci-dessous pour chaque tunnel VPN IPsec Site à Site (S2S). Toute incohérence dans les éléments entraîne la déconnexion des tunnels VPN S2S. Une vérification et une correction des configurations pour vos passerelles locales et vos passerelles VPN Azure vous évitent des redémarrages et des perturbations sur les autres connexions en cours sur les passerelles.

Avant de réinitialiser votre passerelle, vérifiez les points suivants :

* Les adresses IP Internet (VIP) de la passerelle VPN Azure et de la passerelle VPN locale sont correctement configurées dans Azure et dans les stratégies VPN sur site.
* La clé prépartagée doit être la même sur la passerelle VPN Azure et la passerelle VPN locale.
* Si vous appliquez une configuration IPsec/IKE spécifique, telle que le chiffrement, des algorithmes de hachage et PFS (Perfect Forward Secrecy), vérifiez que les passerelles VPN Azure et locale ont les mêmes configurations.

### <a name="azure-portal"></a><a name="portal"></a>Portail Azure

Vous pouvez réinitialiser une passerelle VPN Resource Manager à l’aide du portail Azure. Si vous souhaitez réinitialiser une passerelle classique, consultez les étapes PowerShell relatives au [modèle de déploiement Classique](#resetclassic).

[!INCLUDE [portal steps](../../includes/vpn-gateway-reset-gw-portal-include.md)]

### <a name="powershell"></a><a name="ps"></a>PowerShell

#### <a name="resource-manager-deployment-model"></a>Modèle de déploiement de Resource Manager

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

La cmdlet permettant de réinitialiser une passerelle est **Reset-AzVirtualNetworkGateway**. Avant d’effectuer une réinitialisation, vérifiez que vous disposez de la dernière version des [cmdlets PowerShell Az](/powershell/module/az.network). L’exemple suivant réinitialise une passerelle de réseau virtuel nommée VNet1GW dans le groupe de ressources TestRG1 :

```powershell
$gw = Get-AzVirtualNetworkGateway -Name VNet1GW -ResourceGroupName TestRG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gw
```

Résultat :

Quand vous recevez un résultat de retour, vous pouvez supposer que la réinitialisation de la passerelle a réussi. Toutefois, rien dans le résultat de retour ne l’indique explicitement. Si vous voulez examiner en détail l’historique pour voir exactement à quel moment la réinitialisation de la passerelle s’est produite, vous pouvez afficher ces informations dans le [portail Azure](https://portal.azure.com). Dans le portail, accédez à **« GatewayName » -> Resource Health**.

#### <a name="classic-deployment-model"></a><a name="resetclassic"></a>Modèle de déploiement classique

La cmdlet permettant de réinitialiser une passerelle est **Reset-AzureVNetGateway**. Les cmdlet Azure PowerShell pour le management des services doit être installé localement sur votre bureau. Vous ne pouvez pas utiliser Azure Cloud Shell. Avant d’effectuer une réinitialisation, vérifiez que vous disposez de la dernière version des [cmdlets PowerShell Service Management (SM)](/powershell/azure/servicemanagement/install-azure-ps#azure-service-management-cmdlets). Lorsque vous utilisez cette commande, assurez-vous que vous utilisez le nom complet du réseau virtuel. Les réseaux virtuels classiques ayant été créés à l’aide du portail ont un nom long requis pour PowerShell. Vous pouvez afficher le nom long à l’aide de « Get-AzureVNetConfig -ExportToFile C:\Myfoldername\NetworkConfig.xml ».

L’exemple suivant réinitialise la passerelle pour un réseau virtuel nommé « Group TestRG1 TestVNet1» (qui apparaît simplement comme « TestVNet1 » dans le portail) :

```powershell
Reset-AzureVNetGateway –VnetName 'Group TestRG1 TestVNet1'
```

Résultat :

```powershell
Error          :
HttpStatusCode : OK
Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
Status         : Successful
RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
StatusCode     : OK
```

### <a name="azure-cli"></a><a name="cli"></a>Interface CLI Azure

Pour réinitialiser la passerelle, utilisez la commande [az network vnet-gateway reset](/cli/azure/network/vnet-gateway). L’exemple suivant réinitialise une passerelle de réseau virtuel nommée VNet5GW dans le groupe de ressources TestRG5 :

```azurecli
az network vnet-gateway reset -n VNet5GW -g TestRG5
```

Résultat :

Quand vous recevez un résultat de retour, vous pouvez supposer que la réinitialisation de la passerelle a réussi. Toutefois, rien dans le résultat de retour ne l’indique explicitement. Si vous voulez examiner en détail l’historique pour voir exactement à quel moment la réinitialisation de la passerelle s’est produite, vous pouvez afficher ces informations dans le [portail Azure](https://portal.azure.com). Dans le portail, accédez à **« GatewayName » -> Resource Health**.
