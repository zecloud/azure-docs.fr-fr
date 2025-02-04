---
title: Concepts – Surveiller et réparer des clouds privés Azure VMware Solution
description: Découvrez comment Azure VMware Solution surveille et répare les serveurs VMware ESXi sur un cloud privé Azure VMware Solution.
ms.topic: conceptual
ms.custom: contperf-fy21q2
ms.date: 02/16/2021
ms.openlocfilehash: 59319b5598be9770e82b9676a28444648230a019
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "100633136"
---
# <a name="monitor-and-repair-azure-vmware-solution-private-clouds"></a>Surveiller et réparer des clouds privés Azure VMware Solution

Azure VMware Solution surveille continuellement les serveurs VMware ESXi sur un cloud privé Azure VMware Solution. 

## <a name="what-azure-vmware-solution-monitors"></a>Éléments surveillés par Azure VMware Solution

Azure VMware Solution supervise les conditions suivantes sur l’hôte :  

- État des processeurs 
- État de la mémoire 
- État des connexions et de l’alimentation 
- État du ventilateur matériel 
- Perte de connectivité réseau 
- État de la carte système matérielle 
- Des erreurs se sont produites sur le ou les disques d’un hôte vSAN 
- Tension du matériel 
- État de la température du matériel 
- État de l’alimentation du matériel 
- État du stockage 
- Échec de connexion 

> [!NOTE]
> Les administrateurs de locataires Azure VMware Solution ne doivent ni modifier ni supprimer les alarmes VMware vCenter définies ci-dessus, car celles-ci sont gérées par le plan de contrôle Azure VMware Solution sur vCenter. Ces alarmes sont utilisées par l’analyse de la solution VMware Azure pour déclencher le processus de correction de l’hôte Azure VMware Solution.

## <a name="azure-vmware-solution-host-remediation"></a>Correction de l’hôte Azure VMware Solution  

Quand Azure VMware Solution détecte une dégradation ou une défaillance sur un nœud Azure VMware Solution, il déclenche un processus de correction de l’hôte. La correction de l’hôte implique le remplacement du nœud défaillant par un nouveau nœud sain.  

Le processus de correction de l’hôte commence par l’ajout d’un nouveau nœud sain dans le cluster. Ensuite, lorsque cela est possible, l’hôte défectueux est placé en mode de maintenance VMware vSphere. VMware vMotion déplace les machines virtuelles de l’hôte défaillant vers d’autres serveurs disponibles dans le cluster, permettant ainsi une migration dynamique des charges de travail sans temps d’arrêt. Si l’hôte défaillant ne peut pas être placé en mode de maintenance, l’hôte est supprimé du cluster.

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous avez abordé la façon dont Azure VMware Solution surveille et répare les clouds privés, vous pouvez en apprendre davantage sur les sujets suivants :

- [Mises à niveau de clouds privés Azure VMware Solution](concepts-upgrades.md).
- [Comment activer la ressource Azure VMware Solution](enable-azure-vmware-solution.md).
