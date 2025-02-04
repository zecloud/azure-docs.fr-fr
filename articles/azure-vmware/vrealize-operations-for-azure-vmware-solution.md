---
title: Configurer vRealize Operations pour Azure VMware Solution (AVS)
description: Découvrez comment configurer vRealize Operations pour le cloud privé de votre solution VMware Azure.
ms.topic: how-to
ms.date: 01/26/2021
ms.openlocfilehash: 8015bb61a7401b4c97807e0256e06d4967c39026
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104802486"
---
# <a name="set-up-vrealize-operations-for-azure-vmware-solution"></a>Configurer vRealize Operations pour Azure VMware Solution (AVS)


vRealize Operations Manager est une plateforme de gestion des opérations qui permet aux administrateurs de l’infrastructure VMware de surveiller les ressources système. Ces ressources système peuvent être des objets au niveau de l’application ou de l’infrastructure (tant physiques que virtuels). La plupart des administrateurs VMware ont utilisé vRealize Operations pour surveiller et gérer les composants de cloud privé VMware : vCenter, ESXi, NSX-T, vSAN et VMware HCX.  Chaque cloud privé Azure VMware Solution approvisionné contient un déploiement dédié de vCenter, NSX-T, vSAN et HCX. 

Commencez par passer en revue minutieusement les sections [Avant de commencer](#before-you-begin) et [Prérequis](#prerequisites). Ensuite, nous vous guiderons à travers les deux topologies de déploiement classiques :

> [!div class="checklist"]
> * [vRealize Operations en local gérant le déploiement d’Azure VMware Solution](#on-premises-vrealize-operations-managing-azure-vmware-solution-deployment)
> * [vRealize Operations en cours d’exécution sur le déploiement d’Azure VMware Solution](#vrealize-operations-running-on-azure-vmware-solution-deployment)

## <a name="before-you-begin"></a>Avant de commencer
* Consultez la [documentation du produit vRealize Operations Manager](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) pour en savoir plus sur le déploiement de vRealize Operations. 
* Consultez la [série de tutoriels](tutorial-network-checklist.md) de base du centre de données défini par logiciel (SDDC) Azure VMware Solution.
* Vous pouvez également consulter la documentation du produit [vRealize Operations Remote Controller](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-263F9219-E801-4383-8A59-E84F3D01ED6B.html) pour en savoir plus sur l’option de déploiement local de vRealize Operations gérant Azure VMware Solution. 


## <a name="prerequisites"></a>Prérequis
* [vRealize Operations Manager](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html) installé.
* Un VPN ou Azure ExpressRoute configuré entre le SDDC local et celui d’Azure VMware Solution.
* Un cloud privé Azure VMware Solution a été déployé dans Azure.



## <a name="on-premises-vrealize-operations-managing-azure-vmware-solution-deployment"></a>vRealize Operations en local gérant le déploiement d’Azure VMware Solution
La plupart des clients disposent d’un déploiement local de vRealize Operations pour gérer un ou plusieurs domaines vCenter locaux. Lorsqu’ils approvisionnent un cloud privé Azure VMware Solution, ils se connectent à leur environnement local avec leur cloud privé à l’aide d’une solution Azure ExpressRoute ou VPN de couche 3.  

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-1.png" alt-text="vRealize Operations en local gérant le déploiement d’Azure VMware Solution" border="false":::

Pour étendre les fonctionnalités des opérations vRealize au cloud privé Azure VMware Solution, vous créez une [instance d’adaptateur pour les ressources de cloud privé](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.config.doc/GUID-640AD750-301E-4D36-8293-1BFEB67E2600.html). Celle-ci collecte des données à partir du cloud privé Azure VMware Solution et les intègre aux opérations vRealize locales. L’instance vRealize Operations Manager locale peut se connecter directement au gestionnaire vCenter et NSX-T sur Azure VMware Solution. Facultativement, vous pouvez déployer un collecteur distant des opérations vRealize sur le cloud privé Azure VMware Solution. Le collecteur compresse et chiffre les données collectées à partir du cloud privé avant qu’elles ne soient envoyées via le réseau VPN ou ExpressRoute à vRealize Operations Manager s’exécutant localement. 

> [!TIP]
> Pour obtenir des instructions pas à pas sur l’installation de vRealize Operations Manager, reportez-vous à la [documentation de VMware](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.vapp.doc/GUID-7FFC61A0-7562-465C-A0DC-46D092533984.html). 



## <a name="vrealize-operations-running-on-azure-vmware-solution-deployment"></a>vRealize Operations en cours d’exécution sur un déploiement d’Azure VMware Solution

Une autre option consiste à déployer une instance de vRealize Operations Manager sur un cluster vSphere du cloud privé. 

>[!IMPORTANT]
>VMware ne prend pas en charge cette option pour l’instant.

:::image type="content" source="media/vrealize-operations-manager/vrealize-operations-deployment-option-2.png" alt-text="vRealize Operations en cours d’exécution sur un Azure VMware Solution" border="false":::

Après le déploiement de l’instance, vous pouvez configurer les opérations vRealize pour collecter des données à partir de vCenter, ESXi, NSX-T, vSAN et HCX. 



## <a name="known-limitations"></a>Limitations connues

- L’utilisateur **cloudadmin\@vsphere.local** d’Azure VMware Solution a des [privilèges limités](concepts-identity.md).  Les machines virtuelles sur Azure VMware Solution ne prennent pas en charge la collecte de mémoire dans l’invité à l’aide d’outils VMware.  Les utilisations actives et consommées de la mémoire continuent de fonctionner dans ce cas.
- L’optimisation de la charge de travail pour l’intention métier basée sur l’hôte ne fonctionne pas, car Azure VMware Solution gère les configurations de cluster, y compris les paramètres DRS.
- L’optimisation de la charge de travail pour le placement inter cluster au sein du SDDC avec l’intention métier basée sur le cluster est entièrement prise en charge avec vRealize Operations Manager 8.0 et versions supérieures. Toutefois, l’optimisation de la charge de travail n’a pas connaissance des pools de ressources et place les machines virtuelles au niveau du cluster. Un utilisateur peut corriger cela manuellement dans l’interface Azure VMware Solution vCenter Server.
- Vous ne pouvez pas vous connecter à vRealize Operations Manager à l’aide de vos informations d’identification Azure VMware Solution vCenter Server. 
- Azure VMware Solution ne prend pas en charge le plug-in vRealize Operations Manager.

Lors de la connexion d’Azure VMware Solution vCenter à vRealize Operations Manager à l’aide d’un compte vCenter Server Cloud, l’avertissement suivant s’affiche :

:::image type="content" source="./media/vrealize-operations-manager/warning-adapter-instance-creation-succeeded.png" alt-text="Avertissement - Réussite de la création de l’instance de l’adaptateur":::

L’avertissement s’affiche parce que l’utilisateur **cloudadmin\@vsphere.local** dans Azure VMware Solution ne dispose pas de privilèges suffisants pour effectuer toutes les actions vCenter Server requises pour l’inscription. Toutefois, les privilèges sont suffisants pour que l’instance de l’adaptateur effectue une collecte des données, comme indiqué ci-dessous :

:::image type="content" source="./media/vrealize-operations-manager/adapter-instance-to-perform-data-collection.png" alt-text="Instance d’adaptateur capable d’effectuer la collecte de données":::

Pour plus d’informations, consultez la section [Privilèges requis pour la configuration d’une instance d’adaptateur vCenter](https://docs.vmware.com/en/vRealize-Operations-Manager/8.1/com.vmware.vcom.core.doc/GUID-3BFFC92A-9902-4CF2-945E-EA453733B426.html).

<!-- LINKS - external -->


<!-- LINKS - internal -->




