---
title: 'Didacticiel : Intégration d’Azure Active Directory à Zscaler Two | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Zscaler Two.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/18/2020
ms.author: jeedes
ms.openlocfilehash: 8c3d76f24cce860538dc3bd5dcf29b59708bdb5d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98726219"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a>Didacticiel : Intégration d’Azure Active Directory à Zscaler Two

L’objectif de ce tutoriel est de montrer comment intégrer Zscaler Two à Azure Active Directory (Azure AD). Quand vous intégrez Zscaler Two à Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à Zscaler Two.
* Permettre à vos utilisateurs de se connecter automatiquement à Zscaler Two avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à Zscaler Two, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement Zscaler Two pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Zscaler Two prend en charge l’authentification unique initiée par le **fournisseur de services**

* Zscaler Two prend en charge l’attribution d’utilisateurs **juste-à-temps**

## <a name="adding-zscaler-two-from-the-gallery"></a>Ajouter Zscaler Two à partir de la galerie

Pour configurer l’intégration de Zscaler Two à Azure AD, vous devez ajouter Zscaler Two, disponible dans la galerie, à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Zscaler Two** dans la zone de recherche.
1. Sélectionnez **Zscaler Two** dans le volet des résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-zscaler-two"></a>Configurer et tester l’authentification unique (SSO) Azure AD pour Zscaler Two

Configurez et testez l’authentification unique Azure AD auprès de Zscaler Two à l’aide d’un utilisateur de test nommé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Zscaler Two associé.

Pour configurer et tester l’authentification unique Azure AD auprès de Zscaler Two, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Zscaler Two](#configure-zscaler-two-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test Zscaler Two](#create-zscaler-two-test-user)** pour avoir un équivalent de B.Simon dans Zscaler Two qui est lié à la représentation de l’utilisateur Azure AD.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Zscaler Three**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, entrez les valeurs pour les champs suivants :

    Dans la zone de texte **URL de connexion**, entrez l’URL utilisée par vos utilisateurs pour se connecter à l’application Zscaler Two.

    > [!NOTE]
    > Remplacez la valeur par l’URL de connexion. Pour obtenir cette valeur, contactez [l’équipe du support Zscaler Two](https://www.zscaler.com/company/contact). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

5. Votre application Zscaler Two s’attend à recevoir les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à votre configuration d’attributs du jeton SAML. La capture d’écran suivante montre la liste des attributs par défaut. Cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Attributs d’utilisateur**.

    ![Capture d’écran montrant User Attributes avec l’icône Edit sélectionnée.](common/edit-attribute.png)

6. En plus de ce qui précède, l’application Zscaler Two s’attend à ce que quelques attributs supplémentaires soient repassés dans la réponse SAML. Dans la section **Revendications des utilisateurs** de la boîte de dialogue **Attributs utilisateur**, effectuez les étapes suivantes pour ajouter le jeton SAML comme indiqué dans le tableau ci-dessous :
    
    | Nom | Attribut source |
    | ---------| ------------ |
    | memberOf | user.assignedroles |

    a. Cliquez sur le bouton **Ajouter une nouvelle revendication** pour ouvrir la boîte de dialogue **Gérer les revendications des utilisateurs**.

    ![Capture d’écran montrant User claims avec l’option permettant d’ajouter une nouvelle revendication.](common/new-save-attribute.png)

    ![Capture d’écran montrant la boîte de dialogue Manage user claims où vous pouvez entrer les valeurs décrites.](common/new-attribute-details.png)

    b. Dans la zone de texte **Attribut**, indiquez le nom d’attribut pour cette ligne.

    c. Laissez le champ **Espace de noms** vide.

    d. Sélectionnez Source comme **Attribut**.

    e. Dans la liste **Attribut de la source**, tapez la valeur d’attribut indiquée pour cette ligne.
    
    f. Cliquez sur **Enregistrer**.

    > [!NOTE]
    > Cliquez [ici](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui--preview) pour savoir comment configurer un rôle dans Azure AD.

7. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le **Certificat (Base64)** en fonction des options définies par rapport à vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/certificatebase64.png)

8. Dans la section **Configurer Zscaler Two**, copiez la ou les URL appropriées en fonction de vos besoins.

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

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Zscaler Two.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **Zscaler Two**.
2. Dans la liste des applications, sélectionnez **Zscaler Two**.
3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.
4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez l’utilisateur tel que **Britta Simon** dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

    ![Capture d’écran montrant la boîte de dialogue Utilisateurs et groupes où vous pouvez sélectionner un utilisateur.](./media/zscaler-two-tutorial/tutorial_zscalertwo_users.png)

6. Dans la boîte de dialogue **Sélectionner un rôle**, choisissez le rôle d’utilisateur approprié dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

    ![Capture d’écran montrant la boîte de dialogue Sélectionner un rôle où vous pouvez sélectionner un rôle d’utilisateur.](./media/zscaler-two-tutorial/tutorial_zscalertwo_roles.png)

7. Dans la boîte de dialogue **Ajouter une attribution**, sélectionnez le bouton **Attribuer**.

    ![Capture d’écran montrant la boîte de dialogue Ajouter une attribution où vous pouvez sélectionner Attribuer.](./media/zscaler-two-tutorial/tutorial_zscalertwo_assign.png)

## <a name="configure-zscaler-two-sso"></a>Configurer l’authentification unique Zscaler Two

1. Pour automatiser la configuration dans Zscaler Two, vous devez installer l’**extension de navigateur My Apps Secure Sign-in** en cliquant sur **Installer l’extension**.

    ![Extension My apps](common/install-myappssecure-extension.png)

2. Après l’ajout de l’extension au navigateur, cliquez sur **Configurer Zscaler Two** pour être dirigé vers l’application Zscaler Two. À partir de là, indiquez les informations d’identification de l’administrateur pour vous connecter à Zscaler Two. Cette extension de navigateur configure automatiquement l’application et automatise les étapes 3 à 6.

    ![Configurer l’authentification unique](common/setup-sso.png)

3. Si vous souhaitez configurer manuellement Zscaler Two, ouvrez une nouvelle fenêtre de navigateur web, connectez-vous à votre site d’entreprise Zscaler Two en tant qu’administrateur, puis effectuez les étapes suivantes :

4. Sélectionnez **Administration > Authentification > Paramètres d'authentification** et procédez comme suit :
   
    ![Capture d’écran montrant le site Zscaler One avec les étapes décrites.](./media/zscaler-two-tutorial/ic800206.png "Administration")

    a. Sous Type d'authentification, choisissez **SAML**.

    b. Cliquez sur **Configure SAML**.

5. Dans la fenêtre **Modifier les paramètres SAML**, procédez comme suit, puis cliquez sur Enregistrer.  
            
    ![Gérer les utilisateurs et l’authentification](./media/zscaler-two-tutorial/ic800208.png "Manage Users & Authentication")
    
    a. Dans la zone de texte **URL du portail SAML**, collez l'**URL de connexion** que vous avez copiée sur le portail Azure.

    b. Dans la zone de texte **Attribut de l'ID de connexion**, entrez **NameID**.

    c. Cliquez sur **Charger** pour charger le certificat de signature SAML Azure que vous avez téléchargé depuis le portail Azure dans le **Certificat SSL public**.

    d. Activez/désactivez **Activer l'approvisionnement automatique SAML**.

    e. Dans la zone de texte **Attribut du nom d'affichage de l'utilisateur**, entrez **displayName** si vous souhaitez activer l'approvisionnement automatique SAML pour les attributs displayName.

    f. Dans la zone de texte **Attribut du nom de groupe**, entrez **memberOf** si vous souhaitez activer l'approvisionnement automatique SAML pour les attributs memberOf.

    g. Dans **Attribut du nom du service**, entrez **department** si vous souhaitez activer l'approvisionnement automatique SAML pour les attributs department.

    h. Cliquez sur **Enregistrer**.

6. Dans la page **Configure User Authentication** , procédez comme suit :

    ![Capture d’écran montrant la boîte de dialogue Configure User Authentication avec l’option Activate sélectionnée.](./media/zscaler-two-tutorial/ic800207.png)

    a. Passez la souris sur le menu **Activation** en bas à gauche.

    b. Cliquez sur **Activer**.

## <a name="configuring-proxy-settings&quot;></a>Configuration des paramètres de proxy
### <a name=&quot;to-configure-the-proxy-settings-in-internet-explorer&quot;></a>Pour configurer les paramètres de proxy dans Internet Explorer

1. Démarrez **Internet Explorer**.

2. Pour ouvrir la boîte de dialogue **Options Internet**, sélectionnez **Options Internet** dans le menu **Outils**.   
    
     ![Options Internet](./media/zscaler-two-tutorial/ic769492.png &quot;Options Internet")

3. Cliquez sur l’onglet **Connexions** .   
  
     ![Connexions](./media/zscaler-two-tutorial/ic769493.png "Connexions")

4. Cliquez sur **Paramètres réseau** pour ouvrir la boîte de dialogue **Paramètres réseau**.

5. Dans la section Serveur proxy, procédez comme suit :   
   
    ![Serveur proxy](./media/zscaler-two-tutorial/ic769494.png "Serveur proxy")

    a. Sélectionnez **Utiliser un serveur proxy pour le réseau local**.

    b. Dans la zone de texte Adresse, entrez **gateway.Zscaler Two.net**.

    c. Dans la zone de texte Port, tapez **80**.

    d. Sélectionnez **Ne pas utiliser de serveur proxy pour les adresses locales**.

    e. Cliquez sur **OK** pour fermer la boîte de dialogue **Paramètres du réseau local**.

6. Cliquez sur **OK** pour fermer la boîte de dialogue **Options Internet**.


### <a name="create-zscaler-two-test-user"></a>Créer un utilisateur de test Zscaler Two

Dans cette section, un utilisateur appelé Britta Simon est créé dans Zscaler Two. Zscaler Two prend en charge l’attribution d’utilisateurs juste-à-temps, qui est activée par défaut. Vous n’avez aucune opération à effectuer dans cette section. S’il n’existe pas encore d’utilisateurs dans Zscaler Two, un utilisateur est créé après l’authentification.

>[!Note]
>Si vous devez créer un utilisateur manuellement, contactez l’[équipe du support Zscaler Two](https://www.zscaler.com/company/contact).

### <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à Zscaler Two, où vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion à Zscaler Two pour y lancer le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Lorsque vous cliquez sur la vignette Zscaler Two dans Mes applications, vous êtes redirigé vers l’URL de connexion à Zscaler Two. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).


## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré Zscaler Two, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).
