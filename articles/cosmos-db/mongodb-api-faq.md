---
title: Forum aux questions sur l’API d’Azure Cosmos DB pour MongoDB
description: Obtenez des réponses aux questions fréquemment posées sur l’API d’Azure Cosmos DB pour MongoDB
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: sngun
ms.openlocfilehash: 08899018d03209dab09f61d4dd74feceee03b246
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98019008"
---
# <a name="frequently-asked-questions-about-the-azure-cosmos-dbs-api-for-mongodb"></a>Forum aux questions sur l’API d’Azure Cosmos DB pour MongoDB
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

L’API Azure Cosmos DB pour MongoDB est une couche de compatibilité avec le protocole de transmission qui permet aux applications de communiquer facilement et en toute transparence avec le moteur de base de données Azure Cosmos natif, au moyen des Kits de développement logiciel (SDK) et des pilotes existants et pris en charge par la communauté pour MongoDB. Les développeurs peuvent désormais utiliser des chaînes d’outils et compétences MongoDB existantes pour créer des applications qui tirent parti d’Azure Cosmos DB. Les développeurs bénéficient des fonctionnalités uniques d’Azure Cosmos DB, entre autres la diffusion mondiale avec réplication des écritures dans plusieurs régions, l’indexation automatique, la maintenance de sauvegarde, les contrats SLA avec garantie financière, etc.

## <a name="how-do-i-connect-to-my-database"></a>Comment se connecter à une base de données ?

Pour se connecter à une base de données Cosmos avec l’API Azure Cosmos DB pour MongoDB, la solution la plus rapide consiste à passer par le [portail Azure](https://portal.azure.com). Accédez à votre compte, puis, dans le menu de navigation à gauche, cliquez sur **Démarrage rapide**. Démarrage rapide est le meilleur moyen d’obtenir des extraits de code pour vous connecter à votre base de données.

Azure Cosmos DB applique des normes et des exigences strictes en matière de sécurité. Les comptes Azure Cosmos DB nécessitant une authentification et une communication sécurisée via TLS, veillez à utiliser TLSv1.2.

Pour plus d’informations, consultez [Se connecter à une base de données Cosmos avec l’API Azure Cosmos DB pour MongoDB](connect-mongodb-account.md).

## <a name="error-codes-while-using-azure-cosmos-dbs-api-for-mongodb"></a>Vous rencontrez des codes d’erreur lors de l’utilisation de l’API d’Azure Cosmos DB pour MongoDB ?

Parallèlement aux codes d’erreur MongoDB habituels, l’API Azure Cosmos DB pour MongoDB présente ses propres codes d’erreur. Ceux-ci se trouvent dans le [Guide de résolution des problèmes](mongodb-troubleshoot.md).

## <a name="supported-drivers"></a>Pilotes pris en charge

### <a name="is-the-simba-driver-for-mongodb-supported-for-use-with-azure-cosmos-dbs-api-for-mongodb"></a>Est-ce que le pilote Simba pour MongoDB est pris en charge dans l’utilisation de l’API Azure Cosmos DB pour MongoDB ?

Oui, vous pouvez utiliser le pilote Mongo ODBC de Simba avec l’API Azure Cosmos DB pour MongoDB.

## <a name="next-steps"></a>Étapes suivantes

* [Générer une application web .NET à l’aide de l’API d’Azure Cosmos DB pour MongoDB](create-mongodb-dotnet.md)
* [Créer une application console avec Java et l’API MongoDB dans Azure Cosmos DB](create-mongodb-java.md)
