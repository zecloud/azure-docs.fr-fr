---
title: 'Tutoriel : Intégration de l’authentification unique (SSO) Azure Active Directory avec Appinux | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Appinux.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/28/2020
ms.author: jeedes
ms.openlocfilehash: 728e0347079fc412aadfc88f9f1dabf07d309fc8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98736020"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-appinux"></a>Tutoriel : Intégration de l’authentification unique (SSO) Azure Active Directory avec Appinux

Ce tutoriel explique comment intégrer Appinux avec Azure Active Directory (Azure AD). Quand vous intégrez Appinux avec Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à Appinux.
* Permettre à vos utilisateurs de se connecter automatiquement à Appinux avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.


## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Abonnement Appinux pour lequel l’authentification unique (SSO) est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* Appinux prend en charge l’authentification unique (SSO) initiée par le **fournisseur de services**.

* Appinux prend en charge l’attribution d’utilisateurs **juste-à-temps**.

## <a name="adding-appinux-from-the-gallery"></a>Ajout d’Appinux depuis la galerie

Pour configurer l’intégration d’Appinux à Azure AD, vous devez ajouter Appinux, disponible dans la galerie, à votre liste d’applications SaaS managées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Appinux** dans la zone de recherche.
1. Sélectionnez **Appinux** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.


## <a name="configure-and-test-azure-ad-sso-for-appinux"></a>Configurer et tester l’authentification unique Azure AD pour Appinux

Configurez et testez l’authentification unique Azure AD avec Appinux à l’aide d’un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Appinux associé.

Pour configurer et tester l’authentification unique Azure AD auprès d’Appinux, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Appinux](#configure-appinux-sso)** – pour configurer les paramètres d’authentification unique côté application.
    1. **[Appinux](#create-appinux-test-user)** – pour avoir un équivalent de B.Simon dans Appinux qui soit lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Appinux**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, entrez les valeurs pour les champs suivants :

    a. Dans la zone de texte **URL de connexion**, saisissez une URL au format suivant : `https://<Appinux_SUBDOMAIN>.appinux.com`

    b. Dans la zone de texte **Identificateur (ID d’entité)** , saisissez une URL au format suivant : `https://<Appinux_SUBDOMAIN>.appinux.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de connexion et l’identificateur réels. Pour obtenir ces valeurs, contactez l’[équipe de support client Appinux](https://support.appinux.com/). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

1. L’application Appinux attend les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à votre configuration des attributs de jeton SAML. La capture d’écran suivante montre la liste des attributs par défaut. Cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue Attributs d’utilisateur.

    ![image](common/edit-attribute.png)

1. En plus de ce qui précède, l’application Appinux s’attend à ce que quelques attributs supplémentaires, indiqués ci-dessous, soient repassés dans la réponse SAML. Ces attributs sont également préremplis, mais vous pouvez les examiner pour voir s’ils répondent à vos besoins.

    | **Nom** | **Espace de noms** | **Attribut source**|
    | ---------|---------------| --------- |
    | `givenname` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.givenname` |
    | `surname` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.surname` |
    | `emailaddress` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.mail` |
    | `name` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.userprincipalname` |
    | `UserType` | `http://bcv.appinux.com/claims` | `Provide the value as per your organization` |
    | `Tag` | `http://appinux.com/Tag` | `Provide the value as per your organization` |
    | `Role` | `http://schemas.microsoft.com/ws/2008/06/identity/claims/role` | `user.assignedroles` |
    | `email` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/email` | `user.mail` |
    | `wanshort` | `http://appinux.com/windowsaccountname2` | `extractmailprefix([userprincipalname])` |
    | `nameidentifier` | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` | `user.employeeid` |

    > [!NOTE]
    > Appinux attend des rôles pour les utilisateurs affectés à l’application. Veuillez configurer ces rôles dans Azure AD pour pouvoir affecter les rôles appropriés aux utilisateurs. Pour comprendre comment configurer des rôles dans Azure AD, consultez [cette page](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui--preview).

1. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, recherchez **XML de métadonnées de fédération** et sélectionnez **Télécharger** pour télécharger le certificat et l’enregistrer sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

1. Dans la section **Configurer Appinux**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

Dans cette section, vous allez créer un utilisateur de test appelé B. Simon dans le portail Azure.

1. Dans le volet gauche du Portail Azure, sélectionnez **Azure Active Directory**, **Utilisateurs**, puis **Tous les utilisateurs**.
1. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.
1. Dans les propriétés **Utilisateur**, effectuez les étapes suivantes :
   1. Dans le champ **Nom**, entrez `B.Simon`.  
   1. Dans le champ **Nom de l’utilisateur**, entrez username@companydomain.extension. Par exemple : `B.Simon@contoso.com`.
   1. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ **Mot de passe**.
   1. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Appinux.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Appinux**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous avez configuré les rôles comme indiqué ci-dessus, vous pouvez en sélectionner un dans la liste déroulante **Sélectionner un rôle**.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-appinux-sso"></a>Configurer l’authentification unique Appinux

Pour configurer l’authentification unique côté **Appinux**, vous devez envoyer le **XML des métadonnées de fédération** téléchargé et les URL copiées dans le portail Azure à l’[équipe du support technique Appinux](https://support.appinux.com/). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-appinux-test-user"></a>Créer un utilisateur de test Appinux

Dans cette section, un utilisateur appelé Britta Simon est créé dans Appinux. Appinux prend en charge l’attribution d’utilisateurs juste-à-temps, qui est activée par défaut. Vous n’avez aucune opération à effectuer dans cette section. S’il n’existe pas encore d’utilisateurs dans Appinux, un nouvel utilisateur est créé après l’authentification.

> [!Note]
> Si vous devez créer un utilisateur manuellement, contactez [l’équipe de support technique Appinux](https://support.appinux.com).

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à Appinux, à partir de laquelle vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion à Appinux pour y lancer le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Quand vous cliquez sur la vignette Appinux dans Mes applications, vous êtes redirigé vers l’URL de connexion à Appinux. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).


## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré Appinux, vous pouvez appliquer le contrôle de session qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).
