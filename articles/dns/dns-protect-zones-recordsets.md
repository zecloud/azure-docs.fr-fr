---
title: Protection des zones DNS et des enregistrements - Azure DNS
description: Dans ce parcours d’apprentissage, commencez à protéger des zones DNS et des jeux d’enregistrement dans Microsoft Azure DNS.
services: dns
author: asudbring
ms.service: dns
ms.topic: how-to
ms.date: 2/20/2020
ms.author: allensu
ms.openlocfilehash: 9d65e024e9efa3ad2bcb1c70d44360c8bd0de384
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2021
ms.locfileid: "107785849"
---
# <a name="how-to-protect-dns-zones-and-records"></a>Comment protéger les enregistrements et zones DNS

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Les enregistrements et zones DNS sont des ressources critiques. La suppression d’une zone DNS ou d’un enregistrement DNS peut entraîner une interruption de service. Il est important que les zones et les enregistrements DNS soient protégés contre toute modification non autorisée ou accidentelle.

Cet article explique comment Azure DNS permet de protéger vos enregistrements et zones DNS contre de telles modifications.  Nous appliquons deux puissantes fonctionnalités de sécurité d’Azure Resource Manager : le [contrôle d’accès en fonction du rôle Azure (RBAC)](../role-based-access-control/overview.md) et les [verrous de ressources](../azure-resource-manager/management/lock-resources.md).

## <a name="azure-role-based-access-control"></a>Contrôle d'accès en fonction du rôle Azure

Le contrôle d’accès en fonction du rôle Azure (Azure RBAC) permet une gestion des accès affinée pour les utilisateurs, les groupes et les ressources Azure. Avec Azure RBAC, vous pouvez accorder le niveau d’accès dont les utilisateurs ont besoin. Pour plus d’informations sur la gestion des droits d’accès avec le contrôle Azure RBAC, voir [Qu’est-ce que le contrôle d’accès en fonction du rôle Azure (Azure RBAC) ?](../role-based-access-control/overview.md).

### <a name="the-dns-zone-contributor-role"></a>Rôle Contributeur de zone DNS

Le rôle Collaborateur de zone DNS est un rôle intégré fourni par Azure pour gérer les ressources DNS privées. Ce rôle est appliqué à un utilisateur ou à un groupe, et permet de gérer les ressources DNS.

Le groupe de ressources *myResourceGroup* contient cinq zones pour Contoso Corporation. L’octroi des autorisations Contributeur de zone DNS pour ce groupe de ressources à l’administrateur DNS lui donne un contrôle total sur ces zones DNS. De cette façon, il n’accorde pas d’autorisations non nécessaires. L’administrateur DNS ne peut pas créer ni arrêter des machines virtuelles.

La façon la plus simple d’attribuer des autorisations Azure RBAC consiste à utiliser [le portail Azure](../role-based-access-control/role-assignments-portal.md).  

Ouvrez **Contrôle d’accès (IAM)** pour le groupe de ressources, sélectionnez **Ajouter**, puis sélectionnez le rôle **Collaborateur de zone DNS**. Sélectionnez les utilisateurs ou les groupes auxquels accorder des autorisations.

![Azure RBAC au niveau groupe de ressources via le portail Azure](./media/dns-protect-zones-recordsets/rbac1.png)

Vous pouvez également [accorder des autorisations à l’aide d’Azure PowerShell](../role-based-access-control/role-assignments-powershell.md) :

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group

$usr = "<user email address>"
$rol = "DNS Zone Contributor"
$rsg = "<resource group name>"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -ResourceGroupName $rsg
```

La commande équivalente est également [disponible via l’interface de ligne de commande Azure](../role-based-access-control/role-assignments-cli.md) :

```azurecli
# Grant 'DNS Zone Contributor' permissions to all zones in a resource group

az role assignment create \
--assignee "<user email address>" \
--role "DNS Zone Contributor" \
--resource-group "<resource group name>"
```

### <a name="zone-level-azure-rbac"></a>Azure RBAC au niveau de la zone

Vous pouvez appliquer les règles RBAC d’Azure à un abonnement, à un groupe de ressources ou à une ressource. Cette ressource peut correspondre à une zone DNS ou à un jeu d’enregistrements.

Par exemple, le groupe de ressources *myResourceGroup* contient la zone *contoso.com* et la sous-zone *customers.contoso.com*. Des enregistrements CNAMe sont créés pour chaque compte client. Le compte d’administrateur utilisé pour gérer les enregistrements CNAME se voit attribuer des autorisations lui permettant de créer des enregistrements dans la zone *customers.contoso.com*. Le compte peut gérer uniquement *customers.contoso.com*.

Vous pouvez accorder les autorisations Azure RBAC au niveau zone via le portail Azure.  Ouvrez **Contrôle d’accès (IAM)** pour la zone, sélectionnez **Ajouter**, sélectionnez le rôle **Contributeur de zone DNS**, puis sélectionnez les utilisateurs ou groupes auxquels accorder les autorisations.

![Azure RBAC au niveau zone DNS via le portail Azure](./media/dns-protect-zones-recordsets/rbac2.png)

Vous pouvez également [accorder des autorisations à l’aide d’Azure PowerShell](../role-based-access-control/role-assignments-powershell.md) :

```azurepowershell
# Grant 'DNS Zone Contributor' permissions to a specific zone

$usr = "<user email address>"
$rol = "DNS Zone Contributor"
$rsg = "<resource group name>"
$zon = "<zone name>"
$typ = "Microsoft.Network/DNSZones"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -ResourceGroupName $rsg -ResourceName $zon -ResourceType $typ
```

La commande équivalente est également [disponible via l’interface de ligne de commande Azure](../role-based-access-control/role-assignments-cli.md) :

```azurecli
# Grant 'DNS Zone Contributor' permissions to a specific zone

az role assignment create \
--assignee <user email address> \
--role "DNS Zone Contributor" \
--scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/DnsZones/<zone name>/"
```

### <a name="record-set-level-azure-rbac"></a>Azure RBAC au niveau du jeu d’enregistrements

Les autorisations sont appliquées au niveau du jeu d’enregistrements.  L’utilisateur peut contrôler les entrées dont il a besoin et ne peut apporter aucune autre modification.

Vous pouvez configurer les autorisations Azure RBAC au niveau du jeu d’enregistrements via le portail Azure à l’aide du bouton **Contrôle d’accès (IAM)** situé dans la page du jeu d’enregistrements :

![Azure RBAC au niveau jeu d’enregistrements via le portail Azure](./media/dns-protect-zones-recordsets/rbac3.png)

Vous pouvez également accorder les autorisations Azure RBAC au niveau jeu d’enregistrements en utilisant [Azure PowerShell](../role-based-access-control/role-assignments-powershell.md) :

```azurepowershell
# Grant permissions to a specific record set

$usr = "<user email address>"
$rol = "DNS Zone Contributor"
$sco = 
"/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"

New-AzRoleAssignment -SignInName $usr -RoleDefinitionName $rol -Scope $sco
```

La commande équivalente est également [disponible via l’interface de ligne de commande Azure](../role-based-access-control/role-assignments-cli.md) :

```azurecli
# Grant permissions to a specific record set

az role assignment create \
--assignee "<user email address>" \
--role "DNS Zone Contributor" \
--scope "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/dnszones/<zone name>/<record type>/<record name>"
```

### <a name="custom-roles"></a>Rôles personnalisés

Le rôle intégré Contributeur de zone DNS donne un contrôle total sur une ressource DNS. Vous pouvez créer vos propres rôles Azure personnalisés afin de permettre un contrôle plus précis.

Le compte utilisé pour gérer ces enregistrements CNAME reçoit les autorisations permettant de gérer uniquement les enregistrements CNAME. Le compte ne peut pas modifier les enregistrements appartenant à d’autres types. Le compte ne peut pas effectuer d’opérations au niveau d’une zone, comme la suppression d’une zone.

L’exemple suivant montre une définition de rôle personnalisé pour la gestion des enregistrements CNAME uniquement :

```json
{
    "Name": "DNS CNAME Contributor",
    "Id": "",
    "IsCustom": true,
    "Description": "Can manage DNS CNAME records only.",
    "Actions": [
        "Microsoft.Network/dnsZones/CNAME/*",
        "Microsoft.Network/dnsZones/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
    ],
    "NotActions": [
    ],
    "AssignableScopes": [
        "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
}
```

La propriété Actions définit les autorisations spécifiques de DNS suivantes :

* `Microsoft.Network/dnsZones/CNAME/*` accorde un contrôle total sur les enregistrements CNAME.
* `Microsoft.Network/dnsZones/read` accorde l’autorisation de lire les zones DNS, mais ne pas de les modifier, ce qui permet de voir la zone dans laquelle l’enregistrement CNAME est créé.

Les Actions restantes sont copiées à partir du [rôle intégré Contributeur de Zone DNS](../role-based-access-control/built-in-roles.md#dns-zone-contributor).

> [!NOTE]
> Le recours à un rôle Azure personnalisé pour empêcher la suppression de jeux d’enregistrements tout en autorisant leur mise à jour ne constitue pas un contrôle efficace. Cela empêche la suppression de jeux d’enregistrements, mais pas leur modification.  Les modifications autorisées incluent l’ajout et la suppression d’enregistrements du jeu d’enregistrements, y compris la suppression de tous les enregistrements pour ne laisser qu’un jeu d’enregistrements vide. Sur le plan de la résolution DNS, cela produit le même effet que la suppression du jeu d’enregistrements.

Il n’est pas possible de définir des rôles personnalisés via le portail Azure. Vous pouvez créer un rôle personnalisé basé sur cette définition de rôle en utilisant Azure PowerShell :

```azurepowershell
# Create new role definition based on input file
New-AzRoleDefinition -InputFile <file path>
```

Vous pouvez également le faire via l’interface de ligne de commande Azure :

```azurecli
# Create new role definition based on input file
az role create -inputfile <file path>
```

Ensuite, vous pouvez attribuer le rôle de la même façon qu’un rôle intégré, en procédant de la manière décrite précédemment dans cet article.

Pour plus d’informations sur la façon de créer, gérer et attribuer des rôles personnalisés, consultez [Rôles personnalisés Azure](../role-based-access-control/custom-roles.md).

## <a name="resource-locks"></a>Verrous de ressources

Azure Resource Manager prend en charge un autre type de contrôle de sécurité : la possibilité de verrouiller des ressources. Un verrou de ressource est appliqué à une ressource et le verrouillage de celle-ci s’étend à l’ensemble des utilisateurs et des rôles qui l’utilisent. Pour plus d’informations, consultez [Verrouiller des ressources avec Azure Resource Manager](../azure-resource-manager/management/lock-resources.md).

Il existe deux types de verrou de ressource : **CanNotDelete** et **ReadOnly**. Ces types de verrous peuvent être appliqués à une zone DNS privée ou à un jeu d’enregistrements. Les sections suivantes décrivent quelques scénarios courants et la manière de les prendre en charge à l’aide de verrous de ressources.

### <a name="protecting-against-all-changes"></a>Protection contre toute modification

Pour empêcher toute modification, appliquez à la zone un verrou ReadOnly. Le verrou empêche toute création, modification ou suppression de jeux d’enregistrements.

Vous pouvez créer des verrous de ressources au niveau zone via le portail Azure.  Dans la page des zones DNS, sélectionnez **Verrous**, puis **+Ajouter** :

![Verrous de ressources au niveau zone via le portail Azure](./media/dns-protect-zones-recordsets/locks1.png)

Vous pouvez également créer des verrous de ressources au niveau d’une zone via [Azure PowerShell](/powershell/module/az.resources/new-azresourcelock) :

```azurepowershell
# Lock a DNS zone

$lvl = "<lock level>"
$lnm = "<lock name>"
$rsc = "<zone name>"
$rty = "Microsoft.Network/DNSZones"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rsc -ResourceType $rty -ResourceGroupName $rsg
```

La commande équivalente est également [disponible via l’interface de ligne de commande Azure](/cli/azure/lock#az_lock_create) :

```azurecli
# Lock a DNS zone

az lock create \
--lock-type "<lock level>" \
--name "<lock name>" \
--resource-name "<zone name>" \
--namespace "Microsoft.Network" \
--resource-type "DnsZones" \
--resource-group "<resource group name>"
```

### <a name="protecting-individual-records"></a>Protection d’enregistrements spécifiques

Pour empêcher la modification d’un jeu d’enregistrements DNS, appliquez-lui un verrou ReadOnly.

> [!NOTE]
> L’application d’un verrou CanNotDelete à un jeu d’enregistrements n’est pas un contrôle efficace. Cela empêche la suppression du jeu d’enregistrements, mais pas sa modification.  Les modifications autorisées incluent l’ajout et la suppression d’enregistrements du jeu d’enregistrements, y compris la suppression de tous les enregistrements pour ne laisser qu’un jeu d’enregistrements vide. Sur le plan de la résolution DNS, cela produit le même effet que la suppression du jeu d’enregistrements.

Vous pouvez actuellement configurer des verrous de ressources au niveau jeu d’enregistrements uniquement à l’aide d’Azure PowerShell.  Il n’est pas possible de le faire via le portail Azure ou Azure CLI.

```azurepowershell
# Lock a DNS record set

$lvl = "<lock level>"
$lnm = "<lock name>"
$rsc = "<zone name>/<record set name>"
$rty = "Microsoft.Network/DNSZones/<record type>"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rsc -ResourceType $rty -ResourceGroupName $rsg
```

### <a name="protecting-against-zone-deletion"></a>Protection contre la suppression de zone

Lors de la suppression d’une zone dans Azure DNS, tous les jeux d’enregistrements contenus dans celle-ci sont supprimés.  Cette opération ne peut pas être annulée. La suppression accidentelle d’une zone critique peut avoir une incidence considérable.  Il est important de protéger les zones contre toute suppression accidentelle.

L’application d’un verrou CanNotDelete à une zone empêche la suppression de celle-ci. Les ressources enfants héritent des verrous. Le verrou empêche la suppression de tous les jeux d’enregistrements d’une zone. Comme décrit dans la remarque ci-dessus, ce procédé est inefficace dans la mesure où il reste toujours possible de supprimer des enregistrements à partir des jeux d’enregistrements existants.

Une autre solution est d’appliquer un verrou CanNotDelete à un jeu d’enregistrements d’une zone, comme le jeu d’enregistrements SOA. Une zone ne peut pas être supprimée sans que les jeux d’enregistrements qu’elle contient ne soient eux aussi supprimés. Ce verrou empêche la suppression d’une zone, tout en permettant de modifier librement les jeux d’enregistrements qu’elle contient. Lorsqu’un utilisateur tente de supprimer une zone, Azure Resource Manager détecte cette tentative. Étant donné que la suppression de la zone entraînerait également la suppression du jeu d’enregistrements SOA, Azure Resource Manager bloque cette tentative, car les enregistrements SOA sont verrouillés.  Aucun jeu d’enregistrements n’est supprimé.

La commande PowerShell suivante crée un verrou CanNotDelete sur l’enregistrement SOA de la zone donnée :

```azurepowershell
# Protect against zone delete with CanNotDelete lock on the record set

$lvl = "CanNotDelete"
$lnm = "<lock name>"
$rsc = "<zone name>/@"
$rty = "Microsoft.Network/DNSZones/SOA"
$rsg = "<resource group name>"

New-AzResourceLock -LockLevel $lvl -LockName $lnm -ResourceName $rsc -ResourceType $rty -ResourceGroupName $rsg
```

Pour empêcher la suppression accidentelle d’une zone, vous pouvez également utiliser un rôle personnalisé. Ce rôle garantit que les comptes utilisés pour gérer vos zones ne disposeront pas d’autorisations permettant de supprimer des zones. 

Lorsque vous avez besoin de supprimer une zone, vous pouvez effectuer une suppression en deux étapes :

 - Tout d’abord, attribuez des autorisations de suppression de zone.
 - Ensuite, accordez des autorisations pour supprimer la zone en question.

Le rôle personnalisé fonctionne pour toutes les zones auxquelles ces comptes accèdent. Les comptes disposant d’autorisations de suppression de zone, tels que celui du propriétaire de l’abonnement, peuvent malgré tout supprimer accidentellement une zone.

Il est possible d’utiliser les deux approches (verrous de ressources et rôles personnalisés) en même temps pour une défense en profondeur des zones DNS.

## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’informations sur Azure RBAC, consultez [Qu’est-ce que le contrôle d’accès en fonction du rôle Azure (Azure RBAC) ?](../role-based-access-control/overview.md).
* Pour plus d’informations sur l’utilisation des verrous de ressources, voir [Verrouiller des ressources avec Azure Resource Manager](../azure-resource-manager/management/lock-resources.md).
