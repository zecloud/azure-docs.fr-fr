---
title: Notes de publication de StorSimple 8000 Series Update 5.1
description: Décrit les nouvelles fonctionnalités, les problèmes et les solutions de contournement de StorSimple 8000 Series Update 5.1.
author: alkohli
ms.assetid: ''
ms.service: storsimple
ms.topic: conceptual
ms.date: 03/18/2021
ms.author: alkohli
ms.openlocfilehash: cdb971851ba678ce18f5a1c7954e5620740f3a4c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104657567"
---
# <a name="storsimple-8000-series-update-51-release-notes"></a>Notes de publication de StorSimple 8000 Series Update 5.1

## <a name="overview"></a>Vue d’ensemble

Les notes de publication suivantes décrivent les nouvelles fonctionnalités et identifient les problèmes critiques non résolus relatifs à StorSimple 8000 Series Update 5.1. Elles contiennent également une liste des mises à jour du logiciel StorSimple incluses dans cette version.

Update 5.1 peut être appliquée à n’importe quel appareil StorSimple exécutant Update 5. Si vous utilisez une version antérieure à la version 5, appliquez d’abord Update 5, puis Update 5.1. La version d’appareil associée à Update 5.1 est 6.3.9600.17885.

Lisez les informations contenues dans les notes de publication avant de déployer la mise à jour dans votre solution StorSimple.

> [!IMPORTANT]
>
> * Update 5.1 est une mise à jour obligatoire et doit être installée immédiatement. Pour plus d’informations, consultez [Comment appliquer Update 5.1](storsimple-8000-install-update-51.md).
> * Update 5.1 ne comporte que des mises à jour de sécurité. L’installation de cette mise à jour prend environ 30 minutes. Nous vous recommandons vivement d’appliquer Update 5.1 pour garantir le fonctionnement de votre appareil.
> * Pour les nouvelles versions, vous ne voyez pas immédiatement les mises à jour, car nous effectuons un déploiement échelonné des mises à jour. Revérifiez les mises à jour dans quelques jours, car celles-ci seront bientôt disponibles.

## <a name="whats-new-in-update-51"></a>Nouveautés d’Update 5.1

Update 5.1 comporte les principaux correctifs de bogues et perfectionnements suivants :

* **TLS 1.2** : cette mise à jour de StorSimple applique TLS 1.2 sur tous les clients. Il s’agit d’une mise à jour obligatoire pour tous les appareils StorSimple de la série 8000.

   Si l’avertissement suivant s’affiche, vous devez mettre à jour le logiciel sur l’appareil avant de continuer :

   Un ou plusieurs appareils StorSimple exécutent une version logicielle plus ancienne. La dernière mise à jour disponible pour TLS 1.2 est une mise à jour obligatoire qui doit être installée immédiatement sur ces appareils. Le protocole TLS 1.2 est utilisé pour toutes les communications sur le portail Azure et, sans cette mise à jour, l’appareil ne pourra pas communiquer avec le service StorSimple.

## <a name="known-issues-in-update-51-from-previous-releases"></a>Problèmes connus dans Update 5.1 issus des versions précédentes

Il n’existe aucun nouveau problème connu dans Update 5.1. Pour obtenir la liste des problèmes subsistant dans Update 5.1 issus des versions précédentes, consultez les [Notes de publication d’Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="storsimple-cloud-appliance-updates-in-update-51"></a>Mise à jour de StorSimple Cloud Appliance dans Update 5.1

Cette mise à jour ne peut pas s’appliquer à l’équipement cloud StorSimple (également connu sous le nom d’appareil virtuel). De nouvelles appliances cloud doivent être créées à l’aide de l’image Update 5.1. Pour plus d’informations sur la création d’une appliance StorSimple Cloud Appliance, consultez [Déployer et gérer une appliance StorSimple Cloud Appliance](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Étape suivante

Découvrez comment [installer Update 5.1](storsimple-8000-install-update-51.md) sur votre appareil StorSimple.
