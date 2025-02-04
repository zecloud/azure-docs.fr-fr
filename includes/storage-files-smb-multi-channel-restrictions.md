---
title: Fichier include
description: Fichier Include
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/16/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 06db7bcb5698f152dd5062762fdb3d59ae326e22
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102603312"
---
SMB Multichannel pour les partages de fichiers Azure présente actuellement les restrictions suivantes :
- Peut uniquement être utilisé avec les comptes FileStorage localement redondants.
- Pris en charge uniquement pour les clients Windows. 
- Le nombre maximal de canaux est de quatre.
- SMB Direct n’est pas pris en charge.
- Les points de terminaison privés pour les comptes de stockage ne sont pas pris en charge.
- Pour les comptes de stockage dotés de [l’authentification basée sur l’identité](../articles/storage/files/storage-files-active-directory-overview.md) Active Directory Domain Services (AD DS) ou Azure AD DS locale pour Azure Files, les clients SMB ne peuvent pas utiliser l’Explorateur de fichiers Windows pour configurer des autorisations NTFS sur les répertoires et les fichiers.
    - Utilisez plutôt l’outil Windows [icacls](/windows-server/administration/windows-commands/icacls) ou la commande [Set-ACL](/powershell/module/microsoft.powershell.security/set-acl) pour configurer les autorisations.

