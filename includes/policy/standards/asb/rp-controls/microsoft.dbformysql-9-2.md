---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: e2e12cdd06a21d1838ad939a2cebb908bf0452a4
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/14/2021
ms.locfileid: "107511525"
---
|Nom<br /><sub>(Portail Azure)</sub> |Description |Effet(s) |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La sauvegarde géoredondante doit être activée pour Azure Database pour MySQL](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F82339799-d096-41ae-8538-b108becf0970) |Azure Database pour MySQL vous permet de choisir l’option de redondance pour votre serveur de base de données. Vous pouvez choisir un stockage de sauvegarde géoredondant. Dans ce cas, les données sont non seulement stockées dans la région qui héberge votre serveur, mais elles sont aussi répliquées dans une région jumelée pour offrir une possibilité de récupération en cas de défaillance régionale. La configuration du stockage géoredondant pour la sauvegarde est uniquement possible lors de la création du serveur. |Audit, Désactivé |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMySQL_Audit.json) |
