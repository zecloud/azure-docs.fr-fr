---
title: Configurer une instance et l’authentification (portail)
titleSuffix: Azure Digital Twins
description: Découvrez comment configurer une instance du service Azure Digital Twins à l’aide du portail Azure
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.custom: contperf-fy21q2
ms.openlocfilehash: 7247aa45ae164cd4f2eb5f210e0ec6ec91a0a25f
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2021
ms.locfileid: "107104063"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-portal"></a>Configurer une instance Azure Digital Twins et l’authentification (portail)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Cet article explique comment **configurer une nouvelle instance Azure Digital Twins**, notamment la création de l’instance et la configuration de l’authentification. À l’issue de cet article, vous aurez une instance Azure Digital Twins prête pour la programmation.

Cette version de cet article suit ces étapes manuellement, une par une, à l’aide du portail Azure. Le portail Azure est une console web unifiée qui offre une alternative aux outils en ligne de commande.
* Pour suivre ces étapes manuellement à l’aide de CLI, consultez la version CLI de cet article : [*Guide pratique : Configurer une instance et l’authentification (CLI)*](how-to-set-up-instance-cli.md).
* Pour exécuter une configuration automatisée à l’aide d’un exemple de script de déploiement, consultez la version avec script de cet article : [*Guide pratique : Configurer une instance et l’authentification (procédure scriptée)*](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps.md](../../includes/digital-twins-setup-steps.md)]
[!INCLUDE [digital-twins-setup-permissions.md](../../includes/digital-twins-setup-permissions.md)]

## <a name="create-the-azure-digital-twins-instance"></a>Créer l’instance Azure Digital Twins

Dans cette section, vous allez **créer une nouvelle instance d’Azure Digital Twins** à l’aide du [portail Azure](https://ms.portal.azure.com/). Accédez au portail et connectez-vous avec vos informations d’identification.

Une fois dans le portail, commencez par sélectionner _Créer une ressource_ dans le menu de la page d’accueil des services Azure.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-resource.png" alt-text="Sélection de l’option « créer une ressource » dans la page d’accueil du portail Azure":::

Recherchez *Azure Digital Twins* dans la zone de recherche, puis choisissez le service **Azure Digital Twins** dans les résultats. Sélectionnez le bouton _Créer_ pour créer une instance du service.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins.png" alt-text="Sélection de l’option « Créer » dans la page du service Azure Digital Twins":::

Sur la page **Créer une ressource**, renseignez les valeurs ci-dessous :
* **Abonnement**: Abonnement Azure que vous utilisez.
  - **Groupe de ressources** : Groupe de ressources dans lequel déployer l’instance. Si vous n’avez pas de groupe de ressources existant à l’esprit, vous pouvez en créer un ici en sélectionnant le lien *Créer* et en entrant un nom pour le nouveau groupe de ressources.
* **Emplacement** : Région prenant en charge Azure Digital Twins pour le déploiement. Pour plus d’informations sur la prise en charge régionale, visitez [*Produits Azure disponibles par région (Azure Digital Twins)*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
* **Nom de la ressource** : Nom de votre instance Azure Digital Twins. Si votre abonnement a une autre instance Azure Digital Twins dans la région qui utilise déjà le nom spécifié, vous êtes invité à choisir un autre nom.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins-2.png" alt-text="Remplissage des valeurs décrites pour créer une ressource Azure Digital Twins":::

Lorsque vous avez terminé, vous pouvez sélectionner **Vérifier + créer** si vous ne souhaitez pas configurer d’autres paramètres pour votre instance. Une page de résumé s’affiche, dans laquelle vous pouvez consulter les détails de l’instance que vous avez entrés, avant d’appuyer sur **Créer**. 

Si vous ne souhaitez pas configurer d’autres détails pour votre instance, la section suivante décrit les autres onglets d’installation.

### <a name="additional-setup-options"></a>Options d’installation supplémentaires

Voici les options supplémentaires que vous pouvez configurer lors de l’installation en utilisant les autres onglets du processus **Créer une ressource**.

* **Réseau** : Dans cet onglet, vous pouvez activer des points de terminaison privés avec [Azure Private Link](../private-link/private-link-overview.md) afin d’éliminer l’exposition au réseau public pour votre instance. Pour obtenir des instructions, consultez [*Procédure : Activer l’accès privé avec Private Link (préversion)*](./how-to-enable-private-link-portal.md#add-a-private-endpoint-during-instance-creation).
* **Avancé** : Dans cet onglet, vous pouvez activer une [identité managée par le système](../active-directory/managed-identities-azure-resources/overview.md) pour votre instance, qui peut être utilisée lors du transfert d’événements vers des [points de terminaison](concepts-route-events.md). Pour obtenir des instructions, consultez [*Procédure : Activer des identités managées pour les événements de routage (préversion)*](./how-to-enable-managed-identities-portal.md#add-a-system-managed-identity-during-instance-creation).
* **Étiquettes** : Dans cet onglet, vous pouvez ajouter des balises à votre instance pour vous aider à les organiser parmi vos ressources Azure. Pour plus d’informations sur les balises de ressources Azure, consultez [*Baliser les ressources, les groupes de ressources et les abonnements pour l’organisation logique*](../azure-resource-manager/management/tag-resources.md).

### <a name="verify-success-and-collect-important-values"></a>Vérifier la réussite de l’exécution et collecter les valeurs importantes

Après avoir terminé la configuration de votre instance et appuyé sur **Créer**, vous pouvez afficher l’état du déploiement de votre instance dans vos notifications Azure le long de la barre d’icônes du portail. La notification indique quand le déploiement a réussi, et vous pourrez sélectionner le bouton _Accéder à la ressource_ pour afficher votre instance créée.

:::image type="content" source="media/how-to-set-up-instance/portal/notifications-deployment.png" alt-text="Vue des notifications Azure montrant un déploiement réussi et mettant en surbrillance le bouton « Accéder à la ressource »":::

Autrement, si le déploiement échoue, la notification indique pourquoi. Lisez le conseil du message d’erreur, puis réessayez de créer l’instance.

>[!TIP]
>Une fois votre instance créée, vous pouvez revenir à sa page à tout moment en recherchant son nom dans la barre de recherche du portail Azure.

À partir de la page *Vue d’ensemble* de l’instance, notez son *Nom*, son *Groupe de ressources* et son *Nom d’hôte*. Il s’agit de toutes les valeurs importantes dont vous pouvez avoir besoin quand vous continuez à travailler avec votre instance Azure Digital Twins. Si d’autres utilisateurs doivent programmer pour l’instance, vous devez partager ces valeurs avec eux.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="Mise en surbrillance des valeurs importantes de la page de présentation de l’instance":::

Vous disposez maintenant d’une instance Azure Digital Twins opérationnelle. Ensuite, vous allez accorder les autorisations utilisateur Azure appropriées pour la gérer.

## <a name="set-up-user-access-permissions"></a>Configurer les autorisations d’accès utilisateur

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

Commencez par ouvrir la page de votre instance Azure Digital Twins dans le portail Azure. Dans le menu de l’instance, sélectionnez *Contrôle d’accès (IAM)* . Sélectionnez le bouton **+ Ajouter** pour ajouter une nouvelle attribution de rôle.

:::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-1.png" alt-text="Sélection de l’ajout d’une attribution de rôle à partir de la page « Contrôle d’accès (IAM) »":::

Sur la page *Ajouter une attribution de rôle* ci-dessous, renseignez les valeurs (elles doivent être complétées par un utilisateur disposant des [autorisations suffisantes](#prerequisites-permission-requirements) dans l’abonnement Azure) :
* **Rôle** : Sélectionnez *Propriétaire des données Azure Digital Twins* dans le menu déroulant.
* **Attribuer l’accès à** : Utilisez *Utilisateur, groupe ou principal de service*
* **Select** : Recherchez le nom ou l’adresse e-mail de l’utilisateur à attribuer. Lorsque vous sélectionnez le résultat, l’utilisateur s’affiche dans une section *Membres sélectionnés*.

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-2.png" alt-text="Remplissage des champs répertoriés dans la boîte de dialogue « Ajouter une attribution de rôle »":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Lorsque vous avez fini d’entrer les détails, cliquez sur le bouton *Enregistrer*.

### <a name="verify-success"></a>Vérifier la réussite de l’exécution

Vous pouvez afficher l’attribution de rôle que vous avez configurée sous *Contrôle d’accès (IAM) > Attributions de rôle*. L’utilisateur doit s’afficher dans la liste avec le rôle *Propriétaire des données Azure Digital Twins*. 

:::image type="content" source="media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Vue des attributions de rôles pour une instance Azure Digital Twins dans le portail Azure":::

Vous disposez maintenant d’une instance Azure Digital Twins prête à l’emploi et des autorisations pour la gérer.

## <a name="next-steps"></a>Étapes suivantes

Testez les appels d’API REST individuels sur votre instance à l’aide des commandes CLI d’Azure Digital Twins : 
* [az dt reference](/cli/azure/dt)
* [*Guide pratique : Utiliser l’interface CLI d’Azure Digital Twins*](how-to-use-cli.md)

Vous pouvez également découvrir comment connecter une application cliente à votre instance avec un code d’authentification :
* [*Guide pratique : Écrire le code d’authentification de l’application*](how-to-authenticate-client.md)