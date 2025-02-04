---
ms.openlocfilehash: a393cf978318c39081805cae7e38c3386ff156f5
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/11/2021
ms.locfileid: "103010697"
---
| Langage/framework | Projet sur<br/>GitHub                                                                                                    | Package                                                                      | Bien démarrer<br/>démarré                             | Connexion des utilisateurs                                         | Accès aux API web                                                 | Disponibilité générale *ou*<br/>Préversion publique<sup>1</sup> |
|----------------------|--------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|:-----------------------------------------------:|:-----------------------------------------------------:|:---------------------------------------------------------------:|:------------------------------------------------------------:|
| Angular              | [MSAL Angular v2](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)<sup>2</sup>         | [@azure/msal-angular](https://www.npmjs.com/package/@azure/msal-angular)     | —                                               | ![La bibliothèque peut demander des jetons d’ID pour la connexion de l’utilisateur.][y] | ![La bibliothèque peut demander des jetons d’accès pour les API web protégées.][y] | Préversion publique                                               |
| Angular              | [MSAL Angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/msal-angular-v1/lib/msal-angular)<sup>3</sup> | [@azure/msal-angular](https://www.npmjs.com/package/@azure/msal-angular)     |[Didacticiel](../articles/active-directory/develop/tutorial-v2-angular.md)| ![La bibliothèque peut demander des jetons d’ID pour la connexion de l’utilisateur.][y] | ![La bibliothèque peut demander des jetons d’accès pour les API web protégées.][y] | GA                                                           |
| AngularJS            | [MSAL AngularJS](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs)<sup>3</sup>         | [@azure/msal-angularjs](https://www.npmjs.com/package/@azure/msal-angularjs) | —                                               | ![La bibliothèque peut demander des jetons d’ID pour la connexion de l’utilisateur.][y] | ![La bibliothèque peut demander des jetons d’accès pour les API web protégées.][y] | Préversion publique                                               |
| JavaScript           | [MSAL.js v2](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser)<sup>2</sup>              | [@azure/msal-browser](https://www.npmjs.com/package/@azure/msal-browser)     | [Didacticiel](../articles/active-directory/develop/tutorial-v2-javascript-auth-code.md) | ![La bibliothèque peut demander des jetons d’ID pour la connexion de l’utilisateur.][y] | ![La bibliothèque peut demander des jetons d’accès pour les API web protégées.][y] | GA                                                           |
|JavaScript|[MSAL.js 1.0](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-core)<sup>3</sup> | [@azure/msal-core](https://www.npmjs.com/package/@azure/msal-core)    | [Didacticiel](../articles/active-directory/develop/tutorial-v2-javascript-spa.md)| ![La bibliothèque peut demander des jetons d’ID pour la connexion de l’utilisateur.][y] | ![La bibliothèque peut demander des jetons d’accès pour les API web protégées.][y] | GA                                                           |
| React                | [MSAL React](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-react)<sup>2</sup>                 | [@azure/msal-react](https://www.npmjs.com/package/@azure/msal-react)         | —                                               | ![La bibliothèque peut demander des jetons d’ID pour la connexion de l’utilisateur.][y] | ![La bibliothèque peut demander des jetons d’accès pour les API web protégées.][y] | Préversion publique                                               |
<!--
| Vue | [Vue MSAL]( https://github.com/mvertopoulos/vue-msal) | [vue-msal]( https://www.npmjs.com/package/vue-msal) | ![X indicating no.][n] | ![Green check mark.][y] | ![Green check mark.][y] | -- |
-->

<sup>1</sup> [Les conditions d’utilisation supplémentaires des préversions Microsoft Azure][preview-tos] s’appliquent aux bibliothèques en *préversion publique*.

<sup>2</sup> [Flux de code d’authentification][auth-code-flow] avec PCKE uniquement (recommandé). 

<sup>3</sup> [Flux d’octroi implicite][implicit-flow] uniquement.

<!--Image references-->

[y]: ../articles/active-directory/develop/media/common/yes.png
[n]: ../articles/active-directory/develop/media/common/no.png

<!--Reference-style links -->
[AAD-App-Model-V2-Overview]: v2-overview.md
[Microsoft-SDL]: https://www.microsoft.com/securityengineering/sdl/
[preview-tos]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[auth-code-flow]: ../articles/active-directory/develop/v2-oauth2-auth-code-flow.md
[implicit-flow]: ../articles/active-directory/develop/v2-oauth2-implicit-grant-flow.md