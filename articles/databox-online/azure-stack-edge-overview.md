---
title: Présentation de Microsoft Azure Stack Edge Pro | Microsoft Docs
description: Décrit Azure Stack Edge Pro, une solution de stockage qui utilise un appareil physique pour le transfert vers Azure via le réseau.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 03/15/2021
ms.author: alkohli
ms.openlocfilehash: e6cd1f8a1f7d1777e786ab91637b4065a2c5e850
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104585942"
---
# <a name="what-is-azure-stack-edge-pro-with-fpga"></a>Qu’est-ce qu’Azure Stack Edge Pro avec FPGA ?

[!INCLUDE [Azure Stack Edge Pro FPGA end-of-life](../../includes/azure-stack-edge-fpga-eol.md)]

Azure Stack Edge Pro avec FPGA est un appareil de computing en périphérie qui prend en charge l’IA et présente des fonctionnalités de transfert de données via le réseau. Cet article vous fournit une vue d’ensemble de la solution Azure Stack Edge Pro avec FPGA, ses avantages, ses principales fonctionnalités et les scénarios de déploiement.

Azure Stack Edge Pro avec FPGA est une solution de matériel en tant que service. Microsoft fournit un appareil géré dans le cloud avec un FPGA (Field Programmable Gate Array) intégré, qui permet une inférence accélérée de l’IA et possède toutes les fonctionnalités d’une passerelle de stockage réseau.

Azure Data Box Edge a été renommé Azure Stack Edge.

## <a name="use-cases"></a>Cas d'utilisation

Voici différents scénarios d’utilisation d’Azure Stack Edge Pro pour l’inférence rapide de Machine Learning (ML) à la périphérie et le prétraitement des données avant leur envoi à Azure.

- **Inférence avec Azure Machine Learning** : Azure Stack Edge Pro vous permet d’exécuter des modèles Machine Learning (ML) pour obtenir des résultats rapides qui peuvent être traités avant l’envoi des données vers le cloud. Le jeu de données complet peut également être transféré pour continuer à reformer et améliorer vos modèles ML. Pour plus d’informations sur l’utilisation des modèles accélérés matériellement d’Azure ML sur un appareil Azure Stack Edge Pro, consultez [Déployer des modèles accélérés matériellement Azure ML sur Azure Stack Edge Pro](../machine-learning/how-to-deploy-fpga-web-service.md#deploy-to-a-local-edge-server).

- **Prétraiter les données** : transformez les données avant de les envoyer à Azure pour créer un jeu de données plus actionnable. Le prétraitement peut servir à : 

    - Agréger les données.
    - Modifiez les données, par exemple, pour supprimer des données personnelles.
    - Fractionnez les données pour optimiser le stockage et la bande passante, ainsi que pour les analyser de façon plus approfondie.
    - Analyser et réagir aux événements IoT. 

- **Transférer des données vers Azure via le réseau** : utilisez Azure Stack Edge Pro pour transférer rapidement et facilement des données vers Azure pour le calcul et l’analyse à des fins d’archivage. 

## <a name="key-capabilities"></a>Fonctionnalités clés

Azure Stack Edge Pro intègre les fonctionnalités suivantes :

|Fonctionnalité |Description  |
|---------|---------|
|Inférence accélérée avec l’intelligence artificielle| Activée par le FPGA intégré.|
|Informatique       |Permet l’analyse, le traitement et le filtrage des données.|
|Hautes performances | Transferts de données et calcul haute performance.|
|Accès aux données     | Accès direct aux données à partir d’objets blob Stockage Azure et Azure Files à l’aide des API cloud pour le traitement d’autres données dans le cloud. Un cache local sur l’appareil est utilisé pour accéder rapidement aux fichiers les plus récemment utilisés.|
|Géré par le cloud     |L’appareil et le service sont gérés via le portail Azure.  |
|Chargement hors connexion     | Un mode déconnecté prend en charge les scénarios de chargement hors connexion.|
|Protocoles pris en charge     | Prise en charge des protocoles SMB et NFS standard pour l’ingestion des données. <br> Pour plus d’informations sur les versions prises en charge, consultez [Conditions requises pour Azure Stack Edge Pro](azure-stack-edge-system-requirements.md).|
|Actualisation des données     | Possibilité d’actualiser les fichiers locaux avec la dernière version issue du cloud.|
|Chiffrement    | Prise en charge de BitLocker pour chiffrer les données localement et transférer de façon sécurisée des données vers le cloud via *https*.|
|Limitation de bande passante| Limiter l’utilisation de la bande passante pendant les heures de pointe.|
|ExpressRoute | Sécurité accrue avec ExpressRoute. Utilisez la configuration de peering avec laquelle le trafic des appareils locaux vers les points de terminaison de stockage cloud transite par ExpressRoute. Pour plus d’informations, voir [Appairage ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="components"></a>Components

La solution Azure Stack Edge Pro se compose d’une ressource Azure Stack Edge, d’un appareil physique Azure Stack Edge Pro et d’une interface utilisateur web locale.

* **Appareil physique Azure Stack Edge Pro** : un serveur 1U monté en rack, fourni par Microsoft qui peut être configuré pour envoyer des données vers Azure.
    
* **Ressource Azure Stack Edge** : ressource du portail Azure, qui vous permet de gérer un appareil Azure Stack Edge Pro à partir d’une interface web accessible depuis divers emplacements géographiques. Utilisez la ressource Azure Stack Edge pour créer et gérer des ressources, afficher et gérer les appareils et les alertes, et gérer les partages.
  
   <!--[The Azure Stack Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

   Alors qu’Azure Stack Edge Pro approche sa fin de vie, aucune commande n’est prise pour les nouveaux appareils Azure Stack Edge Pro. Si vous êtes un nouveau client, nous vous recommandons d’explorer à l’aide des appareils Azure Stack Edge Pro avec GPU pour vos charges de travail. Pour plus d’informations, consultez [Qu’est-ce qu’Azure Stack Edge Pro avec GPU](azure-stack-edge-gpu-overview.md). Pour plus d’informations sur la commande d’un appareil Azure Stack Edge Pro avec GPU, accédez à [Créer une ressource pour Azure Stack Edge Pro avec GPU](azure-stack-edge-gpu-deploy-prep.md?tabs=azure-portal#create-a-new-resource).

   Si vous êtes un client existant, vous pouvez toujours créer une nouvelle ressource Azure Stack Edge Pro si vous devez remplacer ou réinitialiser votre appareil Azure Stack Edge Pro existant. Pour plus d’instructions, consultez [Créer une commande pour votre appareil Azure Stack Edge Pro](azure-stack-edge-deploy-prep.md#create-new-resource-for-existing-device).

* **Interface utilisateur web locale Azure Stack Edge Pro** : utilisez l’interface utilisateur web locale pour exécuter des diagnostics, arrêter et redémarrer l’appareil Azure Stack Edge Pro, afficher les journaux d’activité de copie, et contactez le support Microsoft pour créer une demande de service.

    <!--![The Azure Stack Edge Pro local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Pour plus d’informations sur l’utilisation de l’interface utilisateur basée sur le web, voir [Utiliser l’interface utilisateur basée sur le web pour gérer votre Azure Stack Edge Pro](azure-stack-edge-manage-access-power-connectivity-mode.md).

## <a name="region-availability"></a>Disponibilité des régions

L’appareil physique Azure Stack Edge Pro, la ressource Azure et le compte de stockage cible vers lequel vous transférez des données n’ont pas besoin de se trouver dans la même région.

- **Disponibilité des ressources** : pour obtenir la liste de toutes les régions où la ressource Azure Stack Edge est disponible, consultez [Disponibilité des produits Azure par région](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Le déploiement d’Azure Stack Edge Pro est également possible dans Azure Government Cloud. Pour plus d’informations, consultez [Présentation d’Azure Government](../azure-government/documentation-government-welcome.md).
    
- **Comptes de stockage de destination** : les comptes de stockage qui stockent les données sont disponibles dans toutes les régions Azure. Pour garantir des performances optimales, les régions où des comptes de stockage stockent les données Azure Stack Edge Pro doivent se trouver à proximité de l’appareil. Un compte de stockage situé dans un emplacement éloigné entraîne des temps de latence longs et une baisse des performances.

Le service Azure Stack Edge est un service non régional. Pour plus d'informations, consultez [Régions et zones de disponibilité dans Azure](https://docs.microsoft.com/azure/availability-zones/az-overview). Le service Azure Stack Edge ne dépend pas d’une région Azure spécifique, ce qui le rend résilient aux pannes aux niveaux de la zone et de la région.

## <a name="next-steps"></a>Étapes suivantes

- Relire les [Conditions requises pour le système Azure Stack Edge Pro](azure-stack-edge-system-requirements.md).
- Comprendre les [limites d’Azure Stack Edge Pro](azure-stack-edge-limits.md).
- Déployer [Azure Stack Edge Pro](azure-stack-edge-deploy-prep.md) sur le portail Azure.
