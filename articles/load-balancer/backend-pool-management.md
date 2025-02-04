---
title: Gestion du pool back-end
titleSuffix: Azure Load Balancer
description: Découvrez comment configurer et gérer le pool back-end d’un équilibreur de charge Azure.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 01/28/2021
ms.author: allensu
ms.openlocfilehash: c49a721a4db758965c9cf8d71f5d73b5754b6088
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654473"
---
# <a name="backend-pool-management"></a>Gestion du pool back-end
Le pool back-end est un composant essentiel de l’équilibreur de charge. Il définit le groupe de ressources qui va servir le trafic pour une règle d’équilibrage de charge donnée.

Il existe deux façons de configurer un pool back-end :
* Carte d’interface réseau (NIC)
* Combinaison de l’adresse IP et de l’ID de la ressource du réseau virtuel (VNET)

Configurez votre pool de back-ends par carte réseau quand vous utilisez des machines virtuelles et des groupes de machines virtuelles identiques existants. Cette méthode génère le lien le plus direct entre votre ressource et le pool back-end. 

Lors de la préallocation de votre pool de back-ends avec une plage d’adresses IP avec laquelle vous envisagez de créer ultérieurement des machines virtuelles et des groupes de machines virtuelles identiques, configurez votre pool de back-ends par une combinaison d’adresses IP et d’ID de réseau virtuel.

Vous pouvez configurer des pools back-end en fonction des adresses IP et des cartes d’interface réseau pour un même équilibreur de charge. Toutefois, vous ne pouvez pas créer un seul pool back-end qui mélange des adresses back-end ciblées en fonction des cartes d’interface réseau et des adresses IP au sein du même pool.

Les sections de configuration de cet article se concentrent sur :

* Azure PowerShell
* Azure CLI
* API REST
* Modèles Microsoft Azure Resource Manager 

Ces sections permettent de comprendre comment les pools back-end sont structurés pour chaque option de configuration.

## <a name="configuring-backend-pool-by-nic"></a>Configuration du pool back-end par carte réseau
Le pool back-end est créé dans le cadre de l’opération de l’équilibreur de charge. La propriété de configuration IP de la carte réseau est utilisée pour ajouter des membres de pool back-end.

Les exemples suivants se concentrent sur les opérations de création et de remplissage du pool back-end afin de mettre en évidence ce workflow et cette relation.

  >[!NOTE] 
  >Il est important de noter que les pools back-end configurés par le biais de l’interface réseau ne peuvent pas être mis à jour dans le cadre d’une opération sur un pool back-end. Tout ajout ou suppression de ressources back-end doivent se produire sur l’interface réseau de la ressource.

### <a name="powershell"></a>PowerShell
Créez un pool back-end :
 
```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"

$backendPool = 
New-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName  
```

Créez une interface réseau et ajoutez-la au pool back-end :

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"
$nicname = "myNic"
$location = "eastus"
$vnetname = <your-vnet-name>

$vnet = 
Get-AzVirtualNetwork -Name $vnetname -ResourceGroupName $resourceGroup

$nic = 
New-AzNetworkInterface -ResourceGroupName $resourceGroup -Location $location -Name $nicname -LoadBalancerBackendAddressPool $backendPoolName -Subnet $vnet.Subnets[0]
```

Récupérez les informations du pool back-end pour que l’équilibreur de charge confirme que cette interface réseau est ajoutée au pool back-end :

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"

$lb =
Get-AzLoadBalancer -ResourceGroupName $res
Get-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -BackendAddressPoolName $backendPoolName 
```

Créez une machine virtuelle et attachez l’interface réseau pour la placer dans le pool back-end :

```azurepowershell-interactive
# Create a username and password for the virtual machine
$cred = Get-Credential

# Create a virtual machine configuration
$vmname = "myVM1"
$vmsize = "Standard_DS1_v2"
$pubname = "MicrosoftWindowsServer"
$nicname = "myNic"
$off = "WindowsServer"
$sku = "2019-Datacenter"
$resourceGroup = "myResourceGroup"
$location = "eastus"

$nic =
Get-AzNetworkInterface -Name $nicname -ResourceGroupName $resourceGroup

$vmConfig = 
New-AzVMConfig -VMName $vmname -VMSize $vmsize | Set-AzVMOperatingSystem -Windows -ComputerName $vmname -Credential $cred | Set-AzVMSourceImage -PublisherName $pubname -Offer $off -Skus $sku -Version latest | Add-AzVMNetworkInterface -Id $nic.Id
 
# Create a virtual machine using the configuration
$vm1 = New-AzVM -ResourceGroupName $resourceGroup -Zone 1 -Location $location -VM $vmConfig
```

### <a name="cli"></a>Interface de ligne de commande
Créez le pool back-end :

```azurecli-interactive
az network lb address-pool create \
--resource-group myResourceGroup \
--lb-name myLB \
--name myBackendPool 
```

Créez une interface réseau et ajoutez-la au pool back-end :

```azurecli-interactive
az network nic create \
--resource-group myResourceGroup \
--name myNic \
--vnet-name myVnet \
--subnet mySubnet \
--network-security-group myNetworkSecurityGroup \
--lb-name myLB \
--lb-address-pools myBackEndPool
```

Récupérez le pool back-end pour confirmer que l’adresse IP a été correctement ajoutée :

```azurecli-interactive
az network lb address-pool show \
--resource-group myResourceGroup \
--lb-name myLb \
--name myBackendPool
```

Créez une machine virtuelle et attachez l’interface réseau pour la placer dans le pool back-end :

```azurecli-interactive
az vm create \
--resource-group myResourceGroup \
--name myVM \
--nics myNic \
--image UbuntuLTS \
--admin-username azureuser \
--generate-ssh-keys
```

### <a name="resource-manager-template"></a>Modèle Resource Manager

Suivez ce [modèle Resource Manager de démarrage rapide](https://github.com/Azure/azure-quickstart-templates/tree/master/101-load-balancer-standard-create/) pour déployer un équilibreur de charge et des machines virtuelles, puis ajouter les machines virtuelles au pool back-end par le biais d’une interface réseau.

Suivez ce [modèle Resource Manager de démarrage rapide](https://github.com/Azure/azure-quickstart-templates/tree/master/101-load-balancer-ip-configured-backend-pool) pour déployer un équilibreur de charge et des machines virtuelles, puis ajouter les machines virtuelles au pool back-end par le biais d’une adresse IP.


## <a name="configure-backend-pool-by-ip-address-and-virtual-network"></a>Configurer le pool back-end par adresse IP et réseau virtuel
Dans les scénarios avec des pools de back-ends préremplis, utilisez l’adresse IP et le réseau virtuel.

La gestion de tous les pools back-end s’effectue directement sur l’objet de pool back-end, comme indiqué dans les exemples ci-dessous.

### <a name="powershell"></a>PowerShell
Créez un pool back-end :

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$loadBalancerName = "myLoadBalancer"
$backendPoolName = "myBackendPool"
$vnetName = "myVnet"
$location = "eastus"
$nicName = "myNic"

$backendPool = New-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -Name $backendPoolName  
```

Mettez à jour le pool back-end avec une nouvelle adresse IP du réseau virtuel existant :
 
```azurepowershell-interactive
$virtualNetwork = 
Get-AzVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup 
 
$ip1 = New-AzLoadBalancerBackendAddressConfig -IpAddress "10.0.0.5" -Name "TestVNetRef" -VirtualNetwork $virtualNetwork  
 
$backendPool.LoadBalancerBackendAddresses.Add($ip1) 

Set-AzLoadBalancerBackendAddressPool -InputObject $backendPool
```

Récupérez les informations du pool back-end pour que l’équilibreur de charge confirme que les adresses back-end sont ajoutées au pool back-end :

```azurepowershell-interactive
Get-AzLoadBalancerBackendAddressPool -ResourceGroupName $resourceGroup -LoadBalancerName $loadBalancerName -Name $backendPoolName 
```
Créez une interface réseau et ajoutez-la au pool back-end. Définissez l’adresse IP sur l’une des adresses back-end :

```azurepowershell-interactive
$nic = 
New-AzNetworkInterface -ResourceGroupName $resourceGroup -Location $location -Name $nicName -PrivateIpAddress 10.0.0.4 -Subnet $virtualNetwork.Subnets[0]
```

Créez une machine virtuelle et attachez la carte réseau avec une adresse IP dans le pool back-end :
```azurepowershell-interactive
# Create a username and password for the virtual machine
$cred = Get-Credential

# Create a virtual machine configuration
$vmname = "myVM1"
$vmsize = "Standard_DS1_v2"
$pubname = "MicrosoftWindowsServer"
$nicname = "myNic"
$off = "WindowsServer"
$sku = "2019-Datacenter"
$resourceGroup = "myResourceGroup"
$location = "eastus"

$nic =
Get-AzNetworkInterface -Name $nicname -ResourceGroupName $resourceGroup

$vmConfig = 
New-AzVMConfig -VMName $vmname -VMSize $vmsize | Set-AzVMOperatingSystem -Windows -ComputerName $vmname -Credential $cred | Set-AzVMSourceImage -PublisherName $pubname -Offer $off -Skus $sku -Version latest | Add-AzVMNetworkInterface -Id $nic.Id

# Create a virtual machine using the configuration
$vm1 = New-AzVM -ResourceGroupName $resourceGroup -Zone 1 -Location $location -VM $vmConfig
```

### <a name="cli"></a>Interface de ligne de commande
À l’aide de l’interface CLI, vous pouvez remplir le pool back-end par le biais de paramètres de ligne de commande ou d’un fichier de configuration JSON. 

Créez et remplissez le pool back-end par le biais de paramètres de ligne de commande :

```azurecli-interactive
az network lb address-pool create \
--resource-group myResourceGroup \
--lb-name myLB \
--name myBackendPool \
--vnet {VNET resource ID} \
--backend-address name=addr1 ip-address=10.0.0.4 \
--backend-address name=addr2 ip-address=10.0.0.5
```

Créez et remplissez le pool back-end par le biais d’un fichier de configuration JSON :

```azurecli-interactive
az network lb address-pool create \
--resource-group myResourceGroup \
--lb-name myLB \
--name myBackendPool \
--vnet {VNET resource ID} \
--backend-address-config-file @config_file.json
```

Fichier de configuration JSON :
```JSON
        [
          {
            "name": "address1",
            "virtualNetwork": "/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}",
            "ipAddress": "10.0.0.4"
          },
          {
            "name": "address2",
            "virtualNetwork": "/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/virtualNetworks/{vnet-name}",
            "ipAddress": "10.0.0.5"
          }
        ]
```

Récupérez les informations du pool back-end pour que l’équilibreur de charge confirme que les adresses back-end sont ajoutées au pool back-end :

```azurecli-interactive
az network lb address-pool show \
--resource-group myResourceGroup \
--lb-name MyLb \
--name MyBackendPool
```

Créez une interface réseau et ajoutez-la au pool back-end. Définissez l’adresse IP sur l’une des adresses back-end :

```azurecli-interactive
az network nic create \
  --resource-group myResourceGroup \
  --name myNic \
  --vnet-name myVnet \
  --subnet mySubnet \
  --network-security-group myNetworkSecurityGroup \
  --lb-name myLB \
  --private-ip-address 10.0.0.4
```

Créez une machine virtuelle et attachez la carte réseau avec une adresse IP dans le pool back-end :

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --nics myNic \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```
 
### <a name="limitations"></a>Limites
Un pool de back-ends configuré par adresse IP présente les limites suivantes :
  * Peut uniquement être utilisé pour des équilibreurs de charge standard
  * Limite de 100 adresses IP dans le pool de back-ends
  * Les ressources back-end doivent être dans le même réseau virtuel que celui de l’équilibreur de charge.
  * Un équilibreur de charge avec un pool de back-ends basé sur IP ne peut pas fonctionner en tant que service Private Link
  * Cette fonctionnalité n’est actuellement pas prise en charge dans le portail Azure.
  * Les conteneurs ACI ne sont pas pris en charge par cette fonctionnalité
  * Vous ne pouvez pas placer des équilibreurs de charge ou des services, tels qu’Application Gateway, dans le pool principal de l’équilibreur de charge
  * Les règles NAT de trafic entrant ne peuvent pas être spécifiées par adresse IP

## <a name="next-steps"></a>Étapes suivantes
Dans cet article, vous avez découvert la gestion du pool back-end Azure Load Balancer et vous avez appris à configurer un pool back-end par adresse IP et réseau virtuel.

En savoir plus sur [Azure Load Balancer](load-balancer-overview.md).

Passez en revue l'[API REST](https://docs.microsoft.com/rest/api/load-balancer/loadbalancerbackendaddresspools/createorupdate) pour la gestion du pool principal basée sur des adresses IP.
