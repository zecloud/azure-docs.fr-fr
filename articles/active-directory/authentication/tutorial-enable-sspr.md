---
title: Activer la réinitialisation de mot de passe en libre-service Azure Active Directory
description: Dans ce tutoriel, vous allez apprendre à activer la réinitialisation de mot de passe en libre-service Azure Active Directory pour un groupe d’utilisateurs, et à tester le processus de réinitialisation de mot de passe.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 03/25/2021
ms.author: justinha
author: justinha
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 39eec4fb6e9907b36908a87c09aceabd0dd1a678
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106075164"
---
# <a name="tutorial-enable-users-to-unlock-their-account-or-reset-passwords-using-azure-active-directory-self-service-password-reset"></a>Tutoriel : Permettre aux utilisateurs de déverrouiller leur compte ou de réinitialiser des mots de passe à l’aide de la réinitialisation de mot de passe en libre-service Azure Active Directory

La réinitialisation de mot de passe en libre-service (SSPR) Azure Active Directory (Azure AD) permet aux utilisateurs de changer ou de réinitialiser leur mot de passe sans intervention d’un administrateur ou d’un agent du support technique. Si Azure AD verrouille le compte d’un utilisateur ou oublie son mot de passe, il peut suivre des invites pour se débloquer et reprendre son travail. Cette fonctionnalité réduit les appels au support technique et la perte de productivité quand l’utilisateur ne parvient pas à se connecter à son appareil ou à une application. Nous vous recommandons de regarder cette vidéo sur [la façon d’activer et de configurer SSPR dans Azure AD](https://www.youtube.com/watch?v=rA8TvhNcCvQ). Nous avons également une vidéo destinée aux administrateurs informatiques sur [la résolution des six messages d’erreur les plus courants pour les utilisateurs finaux avec SSPR](https://www.youtube.com/watch?v=9RPrNVLzT8I).

> [!IMPORTANT]
> Ce tutoriel montre comment un administrateur peut activer la réinitialisation de mot de passe en libre-service. Si vous êtes un utilisateur final déjà inscrit pour la réinitialisation de mot de passe en libre-service et que vous devez vous reconnecter avec votre compte, accédez à la page [Réinitialisation du mot de passe Microsoft Online](https://passwordreset.microsoftonline.com/).
>
> Si votre équipe informatique n’a pas activé la réinitialisation de votre propre mot de passe, contactez votre support technique pour obtenir une assistance supplémentaire.

Ce didacticiel vous montre comment effectuer les opérations suivantes :

> [!div class="checklist"]
> * Activer la réinitialisation de mot de passe en libre-service pour un groupe d’utilisateurs Azure AD
> * Configurer les méthodes d’authentification et les options d’inscription
> * Tester le processus SSPR en tant qu’utilisateur

## <a name="prerequisites"></a>Prérequis

Pour terminer ce tutoriel, vous avez besoin des ressources et des privilèges suivants :

* Un locataire Azure AD actif sur lequel au moins une licence Azure AD gratuite ou d’essai est activée. Dans le niveau G, SSPR ne fonctionne que pour les utilisateurs cloud dans Azure AD.
    * Dans les tutoriels suivants de cette série, vous avez besoin d’une licence Azure AD Premium P1 ou d’une licence d’essai est requise pour la réécriture locale du mot de passe.
    * Si nécessaire, [créez un compte Azure gratuitement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Un compte avec des privilèges d’*Administrateur général*.
* Un utilisateur non-administrateur avec un mot de passe que vous connaissez, par exemple *testuser*. Dans ce tutoriel, vous allez tester l’expérience SSPR de l’utilisateur final à l’aide de ce compte.
    * Si vous devez créer un utilisateur, consultez [Démarrage rapide : Ajouter de nouveaux utilisateurs à Azure Active Directory](../fundamentals/add-users-azure-active-directory.md).
* Un groupe dont l’utilisateur non-administrateur est membre, par exemple *SSPR-Test-Group*. Vous activez SSPR pour ce groupe dans ce tutoriel.
    * Si vous devez créer un groupe, consultez [Créer un groupe de base et ajouter des membres avec Azure Active Directory](../fundamentals/active-directory-groups-create-azure-portal.md).

## <a name="enable-self-service-password-reset"></a>Activer la réinitialisation du mot de passe libre-service

Azure AD vous permet d’activer l’option SSPR pour *Aucun* utilisateur, pour les utilisateurs *sélectionnés* ou pour *Tous* les utilisateurs. Cette capacité à indiquer la précision vous permet de choisir un sous-ensemble d’utilisateurs pour tester le processus d’inscription et le workflow SSPR. Une fois que vous êtes familiarisé avec le processus et que le moment est venu de communiquer les exigences avec un ensemble plus large d’utilisateurs, vous pouvez sélectionner un groupe d’utilisateurs à activer pour SSPR. Vous pouvez également activer SSPR pour tous les membres du locataire Azure AD.

> [!NOTE]
> Pour le moment, vous ne pouvez activer qu’un seul groupe Azure AD pour SSPR à l’aide du portail Azure. Dans le cadre d’un déploiement plus étendu de SSPR, Azure AD prend en charge les groupes imbriqués. Vérifiez que les licences appropriées sont attribuées aux utilisateurs des groupes que vous choisissez. Il n’existe actuellement aucun processus de validation pour ces exigences de licences.

Dans ce tutoriel, configurez SSPR pour un ensemble d’utilisateurs dans un groupe de test. Utilisez *SSPR-Test-Group* et fournissez votre propre groupe Azure AD en fonction des besoins :

1. Connectez-vous au [portail Azure](https://portal.azure.com) à l’aide d’un compte disposant d’autorisations d’*administrateur général*.
1. Recherchez et sélectionnez **Azure Active Directory**, puis sélectionnez **Réinitialisation de mot de passe** dans le menu de gauche.
1. Dans la page **Propriétés**, sous l’option *Réinitialisation de mot de passe en libre-service activée*, sélectionnez **Sélectionner un groupe**.
1. Recherchez et sélectionnez votre groupe Azure AD, par exemple *SSPR-Test-Group*, puis choisissez *Sélectionner*.

    [![Sélectionner un groupe sur le portail Azure pour activer la réinitialisation de mot de passe en libre-service](media/tutorial-enable-sspr/enable-sspr-for-group-cropped.png)](media/tutorial-enable-sspr/enable-sspr-for-group.png#lightbox)

1. Pour activer SSPR pour les utilisateurs sélectionnés, sélectionnez **Enregistrer**.

## <a name="select-authentication-methods-and-registration-options"></a>Sélectionner les méthodes d’authentification et les options d’inscription

Quand les utilisateurs doivent déverrouiller leur compte ou réinitialiser leur mot de passe, ils sont invités à fournir une autre méthode de confirmation. Ce facteur d’authentification supplémentaire vérifie qu’Azure AD a terminé uniquement les événements SSPR approuvés. Vous pouvez choisir les méthodes d’authentification à autoriser, en fonction des informations d’inscription fournies par l’utilisateur.

1. Dans le menu à gauche de la page **Méthodes d’authentification**, affectez la valeur *1* au **Nombre de méthodes à réinitialiser**.

    Pour améliorer la sécurité, vous pouvez augmenter le nombre de méthodes d’authentification requises pour SSPR.

1. Choisissez les **Méthodes disponibles pour les utilisateurs** que votre entreprise souhaite autoriser. Pour ce tutoriel, cochez les cases pour activer les méthodes suivantes :

    * *Notification sur l’application mobile*
    * *Code de l’application mobile*
    * *E-mail*
    * *Téléphone mobile*

    Vous pouvez activer des méthodes d’authentification supplémentaires, telles que *Téléphone professionnel* ou *Questions de sécurité*, pour répondre aux besoins de votre entreprise.

1. Pour appliquer les méthodes d’authentification, sélectionnez **Enregistrer**.

Pour pouvoir déverrouiller leur compte ou réinitialiser un mot de passe, les utilisateurs doivent d’abord inscrire leurs informations de contact. Azure AD utilise ces informations de contact pour les différentes méthodes d’authentification configurées au cours des étapes précédentes.

Un administrateur peut spécifier manuellement ces informations de contact, ou les utilisateurs peuvent accéder au portail d’inscription pour fournir les informations eux-mêmes. Dans ce tutoriel, configurez Azure AD pour inviter les utilisateurs à s’inscrire la prochaine fois qu’ils se connectent.

1. Dans le menu à gauche de la page **Inscription**, sélectionnez *Oui* pour **obliger les utilisateurs à s’inscrire durant la connexion**.
1. Définissez l’option **Nombre de jours avant que les utilisateurs ne soient invités à reconfirmer leurs informations d’authentification** sur *180*.

    Il est important que les informations de contact soient tenues à jour. Si les informations de contact sont obsolètes au démarrage d’un événement SSPR, l’utilisateur risque de ne pas pouvoir déverrouiller son compte ou réinitialiser son mot de passe.

1. Pour appliquer les paramètres d’inscription, sélectionnez **Enregistrer**.

## <a name="set-up-notifications-and-customizations"></a>Configurer les notifications et les personnalisations

Pour informer les utilisateurs de l’activité des comptes, vous pouvez configurer Azure AD pour envoyer des notifications par e-mail quand un événement SSPR se produit. Ces notifications peuvent couvrir les comptes d’utilisateur standard et les comptes d’administrateur. Pour les comptes d’administrateur, cette notification fournit une autre couche de sensibilisation quand un mot de passe de compte d’administrateur privilégié est réinitialisé à l’aide de SSPR. Azure AD avertit tous les administrateurs généraux quand un utilisateur utilise SSPR sur un compte d’administrateur.

1. Dans le menu à gauche de la page **Notifications**, configurez les options suivantes :

   * Définissez l’option **Notifier les utilisateurs lors des réinitialisations de mot de passe** sur *Oui*.
   * Définissez l’option **Notifier tous les administrateurs quand d’autres administrateurs réinitialisent leur mot de passe** sur *Oui*.

1. Pour appliquer les préférences de notification, sélectionnez **Enregistrer**.

Si les utilisateurs ont besoin d’aide supplémentaire avec le processus SSPR, vous pouvez personnaliser le lien « Contactez votre administrateur ». L’utilisateur peut sélectionner ce lien lors du processus d’inscription SSPR et quand il déverrouille son compte ou réinitialise son mot de passe. Pour vous assurer que les utilisateurs bénéficient du support nécessaire, nous vous recommandons vivement de fournir une adresse e-mail ou une URL de support technique personnalisée.

1. Dans le menu à gauche de la page **Personnalisation**, affectez la valeur *Oui* à **Personnaliser le lien du support technique**.
1. Dans le champ **Personnaliser le lien du support technique**, spécifiez une adresse e-mail ou une URL de page web où vos utilisateurs peuvent obtenir une aide supplémentaire de la part de votre organisation, par exemple *https:\//support.contoso.com/*
1. Pour appliquer le lien personnalisé, sélectionnez **Enregistrer**.

## <a name="test-self-service-password-reset"></a>Tester la réinitialisation de mot de passe en libre-service

Avec l’option SSPR activée et configurée, testez le processus SSPR avec un utilisateur qui fait partie du groupe que vous avez sélectionné dans la section précédente, par exemple *Test-SSPR-Group*. L’exemple suivant utilise le compte *testuser*. Fournissez votre propre compte d’utilisateur. Il fait partie du groupe que vous avez activé pour SSPR dans la première section de ce tutoriel.

> [!NOTE]
> Quand vous testez la réinitialisation de mot de passe en libre-service, utilisez un compte non-administrateur. Par défaut, Azure AD active la réinitialisation de mot de passe en libre-service pour les administrateurs. Ils doivent utiliser deux méthodes d’authentification pour réinitialiser leur mot de passe. Pour plus d’informations, consultez [Différences en matière de stratégie de réinitialisation par l’administrateur](concept-sspr-policy.md#administrator-reset-policy-differences).

1. Pour voir le processus d’inscription manuelle, ouvrez une nouvelle fenêtre de navigateur dans InPrivate ou en mode de navigation privée, puis accédez à *https:\//aka.ms/ssprsetup*. Azure AD dirige les utilisateurs vers ce portail d’inscription la prochaine fois qu’ils se connectent.
1. Connectez-vous avec un utilisateur test non-administrateur, par exemple *testuser*, et inscrivez vos informations de contact pour les méthodes d’authentification.
1. Une fois cela terminé, sélectionnez le bouton marqué **Les informations semblent correctes** et fermez la fenêtre du navigateur.
1. Ouvrez une nouvelle fenêtre de navigateur en mode de navigation privée ou InPrivate, et accédez à *https:\//aka.ms/sspr*.
1. Entrez les informations de compte de votre utilisateur test non-administrateurs, par exemple *testuser*, les caractères du test CAPTCHA, puis sélectionnez **Suivant**.

    ![Entrer les informations du compte d’utilisateur pour réinitialiser le mot de passe](media/tutorial-enable-sspr/password-reset-page.png)

1. Suivez les étapes de vérification pour réinitialiser votre mot de passe. Quand vous avez terminé, vous devez recevoir une notification par e-mail indiquant que votre mot de passe a été réinitialisé.

## <a name="clean-up-resources"></a>Nettoyer les ressources

Dans un autre tutoriel de cette série, vous configurerez la réécriture du mot de passe. Cette fonctionnalité réécrit les modifications de mot de passe de SSPR Azure AD vers un environnement AD local. Si vous souhaitez continuer avec cette série de tutoriels pour configurer la réécriture du mot de passe, ne désactivez pas SSPR maintenant.

Si vous ne souhaitez plus utiliser la fonctionnalité SSPR que vous avez configurée dans le cadre de ce tutoriel, définissez l’état de SSPR sur **Aucun** en effectuant les étapes suivantes :

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Recherchez et sélectionnez **Azure Active Directory**, puis sélectionnez **Réinitialisation de mot de passe** dans le menu de gauche.
1. Dans la page **Propriétés**, sous l’option *Réinitialisation de mot de passe en libre-service activée*, sélectionnez **Aucun**.
1. Pour appliquer la modification SSPR, sélectionnez **Enregistrer**.

## <a name="faqs"></a>FAQ

Cette section décrit les questions courantes des administrateurs et des utilisateurs finaux qui essaient SSPR :

- Pourquoi les utilisateurs fédérés attendent jusqu’à 2 minutes après avoir vu le message **Votre mot de passe a été réinitialisé** avant de pouvoir utiliser les mots de passe qui sont synchronisés en local ?

  Pour les utilisateurs fédérés dont les mots de passe sont synchronisés, la source d’autorité pour les mots de passe est locale. Par conséquent, SSPR met à jour uniquement les mots de passe locaux. La resynchronisation de hachage de mot de passe sur Azure AD est planifiée toutes les 2 minutes.

- Quand un utilisateur nouvellement créé, dont les données SSPR sont préremplies (telles que le téléphone et l’e-mail), visite la page d’inscription SSPR, le message **Ne perdez pas l’accès à votre compte !** apparaît comme titre de la page. Pourquoi d’autres utilisateurs dont les données SSPR sont préremplies ne voient pas le message ?

  Un utilisateur qui voit le message **Ne perdez pas l’accès à votre compte !** est membre des groupes d’inscription SSPR/combinés qui sont configurés pour le locataire. Les utilisateurs qui ne voient pas ce message ne font pas partie de ces groupes.

- Lorsque certains utilisateurs suivent le processus SSPR et réinitialisent leur mot de passe, pourquoi ne voient-ils pas l’indicateur du niveau de sécurité du mot de passe ?

  La réécriture du mot de passe synchronisé est activée pour les utilisateurs qui ne voient pas le niveau de sécurité du mot de passe faible/fort. Étant donné que SSPR ne peut pas déterminer la stratégie de mot de passe de l’environnement local du client, il ne peut pas valider la force ou la faiblesse du mot de passe. 

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez activé la réinitialisation de mot de passe Azure AD pour un groupe d’utilisateurs sélectionné. Vous avez appris à :

> [!div class="checklist"]
> * Activer la réinitialisation de mot de passe en libre-service pour un groupe d’utilisateurs Azure AD
> * Configurer les méthodes d’authentification et les options d’inscription
> * Tester le processus SSPR en tant qu’utilisateur

> [!div class="nextstepaction"]
> [Activer Azure AD Multi-Factor Authentication](./tutorial-enable-azure-mfa.md)
