---
title: Versions prises en charge - Azure Database pour PostgreSQL - Serveur flexible
description: Décrit les versions principales et mineures de PostgreSQL prises en charge dans Azure Database pour PostgreSQL - Serveur flexible.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/03/2021
ms.openlocfilehash: 5fda7ebb5a72dd9bbfab0ba72511540cf141563f
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105608848"
---
# <a name="supported-postgresql-major-versions-in-azure-database-for-postgresql---flexible-server"></a>Versions principales PostgreSQL prises en charge dans Azure Database pour PostgreSQL - Serveur flexible

> [!IMPORTANT]
> Azure Database pour PostgreSQL - Serveur flexible est en préversion

Azure Database pour PostgreSQL - Serveur flexible prend actuellement en charge les versions principales suivantes :

## <a name="postgresql-version-12"></a>PostgreSQL version 12

La version mineure actuelle est 12.5. Consultez la [documentation PostgreSQL](https://www.postgresql.org/docs/12/static/release-12-4.html) pour en savoir plus sur les améliorations et les correctifs de cette version mineure.

## <a name="postgresql-version-11"></a>PostgreSQL Version 11

La version mineure actuelle est 11.10. Consultez la [documentation PostgreSQL](https://www.postgresql.org/docs/11/static/release-11-9.html) pour en savoir plus sur les améliorations et les correctifs de cette version mineure.

## <a name="postgresql-version-10-and-older"></a>PostgreSQL version 10 et antérieures

Nous ne prenons pas en charge PostgreSQL version 10 et antérieures pour Azure Database pour PostgreSQL - Serveur flexible. Utilisez l’option de déploiement [Serveur unique](../concepts-supported-versions.md) si vous avez besoin de versions antérieures.

## <a name="managing-upgrades"></a>Gestion des mises à niveau

Le projet PostgreSQL publie régulièrement des versions mineures pour corriger les bogues signalés. Azure Database pour PostgreSQL répare automatiquement les serveurs avec des versions mineures pendant les déploiements mensuels du service.

L’automatisation de la mise à niveau des versions principales n’est pas encore prise en charge. Par exemple, il n’existe actuellement pas de mise à niveau automatique de PostgreSQL 11 vers PostgreSQL 12.<!-- To upgrade to the next major version, create a [database dump and restore](howto-migrate-using-dump-and-restore.md) to a server that was created with the new engine version.-->

<!--
## Next steps

For information on supported PostgreSQL extensions, see [the extensions document](concepts-extensions.md).
-->