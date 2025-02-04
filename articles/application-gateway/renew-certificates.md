---
title: Renouveler un certificat Azure Application Gateway
description: Découvrez comment renouveler un certificat associé à un écouteur de passerelle Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: how-to
ms.date: 01/20/2021
ms.author: victorh
ms.openlocfilehash: f0c06a94498f4d2481a6e953b959d766c60415fb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98622178"
---
# <a name="renew-application-gateway-certificates"></a>Renouveler des certificats Application Gateway

À un moment donné, vous devrez renouveler vos certificats si vous avez configuré votre passerelle Application Gateway pour le chiffrement TLS/SSL.

Vous pouvez renouveler un certificat associé à un écouteur à l’aide du Portail Azure, d’Azure PowerShell ou d’Azure CLI :

## <a name="azure-portal"></a>Portail Azure

Pour renouveler un certificat d’écouteur à partir du portail, accédez à vos écouteurs Application Gateway. Sélectionnez l’écouteur dont le certificat doit être renouvelé, puis **Renouveler ou modifier le certificat sélectionné**.

:::image type="content" source="media/renew-certificate/ssl-cert.png" alt-text="Renouveler le certificat":::

Chargez votre nouveau certificat PFX, donnez-lui un nom, tapez le mot de passe, puis sélectionnez **Enregistrer**.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Pour renouveler votre certificat à l’aide d’Azure PowerShell, utilisez le script suivant :

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Étapes suivantes

Pour découvrir comment configurer le déchargement TLS avec la passerelle Azure Application Gateway, consultez la [configuration du déchargement TLS](./create-ssl-portal.md)