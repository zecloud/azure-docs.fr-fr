---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 02118ff7bf6568ba928ecea3658db1aa23e5015a
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/14/2021
ms.locfileid: "107504859"
---
|Nom |Description |Stratégies |Version |
|---|---|---|---|
|[\[Préversion\] : Déployer - Configurer les prérequis pour activer les agents Azure Monitor et de sécurité Azure sur les machines virtuelles](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitoring_Prerequisites.json) |Configurez des machines de manière à installer automatiquement les agents Azure Monitor et Azure Security. Security Center collecte les événements provenant des agents, et les utilise pour communiquer des alertes de sécurité et des tâches personnalisées de sécurisation renforcée (recommandations). Créez un groupe de ressources et un espace de travail Log Analytics dans la même région que la machine pour stocker les enregistrements d’audit. Cette stratégie s’applique uniquement aux machines virtuelles de certaines régions. |5 |1.0.0-preview |
|[Activer Azure Monitor pour les groupes de machines virtuelles identiques](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VMSS.json) |Activez Azure Monitor pour les groupes de machines virtuelles identiques dans l’étendue spécifiée (groupe d’administration, abonnement ou groupe de ressources). Utilise l’espace de travail Log Analytics comme paramètre. Remarque : Si la valeur upgradePolicy du groupe identique est définie sur Manuel, vous devez appliquer l’extension à toutes les machines virtuelles du groupe en appelant une mise à niveau. Dans l’interface de ligne de commande, cela se traduirait par az vmss update-instances. |6 |1.0.1 |
|[Activer Azure Monitor pour machines virtuelles](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policySetDefinitions/Monitoring/AzureMonitor_VM.json) |Activez Azure Monitor pour les machines virtuelles dans l’étendue spécifiée (groupe d’administration, abonnement ou groupe de ressources). Utilise l’espace de travail Log Analytics comme paramètre. |10 |2.0.0 |
