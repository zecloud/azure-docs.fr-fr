---
author: PatrickFarley
ms.author: pafarley
ms.service: cognitive-services
ms.date: 09/15/2020
ms.openlocfilehash: 2b14c6e679a25ad5f37a9cb10bce78616126990b
ms.sourcegitcommit: 5fd1f72a96f4f343543072eadd7cdec52e86511e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/01/2021
ms.locfileid: "106113218"
---
Bien démarrer avec la bibliothèque de client Custom Vision pour .NET Suivez les étapes ci-après pour installer le package et essayer l’exemple de code visant à créer un modèle de détection d’objets. Vous allez créer un projet, ajouter des étiquettes, entraîner le projet sur des exemples d’images et utiliser l’URL de point de terminaison de prédiction du projet pour le tester programmatiquement. Utilisez cet exemple comme modèle pour générer votre propre application de reconnaissance d’image.

> [!NOTE]
> Si vous souhaitez créer et entraîner un modèle de détection d’objets _sans_ écrire de code, consultez à la place les [instructions basées sur le navigateur](../../get-started-build-detector.md).

Utilisez la bibliothèque de client Custom Vision pour .NET pour :

* Créer un projet de service Custom Vision
* Ajouter des balises au projet
* Charger et étiqueter des images
* Entraîner le projet
* Publier l’itération actuelle
* Tester le point de terminaison de prédiction

[Documentation de référence](/dotnet/api/overview/azure/cognitiveservices/client/customvision) | Code source de la bibliothèque [(entraînement)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.CustomVision.Training) [(prédiction)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Vision.CustomVision.Prediction) | Package (NuGet) [(entraînement)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/) [(prédiction)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/) | [Exemples](/samples/browse/?products=azure&term=vision&terms=vision)

## <a name="prerequisites"></a>Prérequis

* Abonnement Azure - [En créer un gratuitement](https://azure.microsoft.com/free/cognitive-services/)
* L’[IDE Visual Studio](https://visualstudio.microsoft.com/vs/) ou la version actuelle de [.NET Core](https://dotnet.microsoft.com/download/dotnet-core).
* Une fois que vous avez votre abonnement Azure, <a href="https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision"  title="Créer une ressource Custom Vision"  target="_blank">créez une ressource Custom Vision </a> dans le portail Azure pour créer une ressource d’entraînement et de prédiction et obtenir vos clés et votre point de terminaison. Attendez qu’elle se déploie, puis cliquez sur le bouton **Accéder à la ressource**.
    * Vous aurez besoin de la clé et du point de terminaison des ressources que vous créez pour connecter votre application Custom Vision. Vous collerez votre clé et votre point de terminaison dans le code ci-dessous plus loin dans le guide de démarrage rapide.
    * Vous pouvez utiliser le niveau tarifaire Gratuit (`F0`) pour tester le service, puis passer par la suite à un niveau payant pour la production.

## <a name="setting-up"></a>Configuration

### <a name="create-a-new-c-application"></a>Créer une application C#

#### <a name="visual-studio-ide"></a>[IDE Visual Studio](#tab/visual-studio)

À l’aide de Visual Studio, créez une application .NET Core. 

### <a name="install-the-client-library"></a>Installer la bibliothèque de client 

Une fois que vous avez créé un projet, installez la bibliothèque de client en cliquant avec le bouton droit sur la solution de projet dans l’**Explorateur de solutions** et en sélectionnant **Gérer les packages NuGet**. Dans le gestionnaire de package qui s’ouvre, sélectionnez **Parcourir**, cochez **Inclure la préversion** et recherchez `Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training` et `Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction`. Sélectionnez la version la plus récente, puis **Installer**. 

#### <a name="cli"></a>[INTERFACE DE LIGNE DE COMMANDE](#tab/cli)

Dans une fenêtre de console (par exemple cmd, PowerShell ou Bash), utilisez la commande `dotnet new` pour créer une application console avec le nom `custom-vision-quickstart`. Cette commande crée un projet C# simple nommé « Hello World » avec un seul fichier source : *program.cs*. 

```console
dotnet new console -n custom-vision-quickstart
```

Déplacez vos répertoires vers le dossier d’application nouvellement créé. Vous pouvez générer l’application avec :

```console
dotnet build
```

La sortie de génération ne doit contenir aucun avertissement ni erreur. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>Installer la bibliothèque de client 

Dans le répertoire de l’application, installez la bibliothèque de client Custom Vision pour .NET à l’aide de la commande suivante :

```console
dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training --version 2.0.0
dotnet add package Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction --version 2.0.0
```

---

> [!TIP]
> Vous voulez voir l’intégralité du fichier de code de démarrage rapide à la fois ? Vous le trouverez sur [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/CustomVision/ObjectDetection/Program.cs), qui contient les exemples de code utilisés dans ce guide de démarrage rapide.

À partir du répertoire du projet, ouvrez le fichier *program.cs* et ajoutez ce qui suit en utilisant les directives `using` suivantes :

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_imports)]

Dans la méthode **Main** de l’application, créez des variables pour la clé et le point de terminaison de votre ressource.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_creds)]

> [!IMPORTANT]
> Accédez au portail Azure. Si les ressources Custom Vision que vous avez créées dans la section **Prérequis** ont été déployées, cliquez sur le bouton **Accéder à la ressource** sous **Étapes suivantes**. Les clés et le point de terminaison se trouvent dans les pages **Clé et point de terminaison** des ressources, sous **Gestion des ressources**. Vous devez obtenir les clés pour vos ressources d’entraînement et de prédiction, ainsi que le point de terminaison d’API pour votre ressource d’entraînement.
>
> N’oubliez pas de supprimer les clés de votre code une fois que vous avez terminé, et ne les postez jamais publiquement. Pour la production, envisagez d’utiliser une méthode de stockage et d’accès sécurisée pour vos informations d’identification. Pour plus d’informations, consultez l’article sur la [sécurité](../../../cognitive-services-security.md) de Cognitive Services.

Dans la méthode **Main** de l’application, ajoutez des appels pour les méthodes utilisées dans ce guide de démarrage rapide. Vous les implémenterez ultérieurement.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_maincalls)]

## <a name="object-model"></a>Modèle objet

|Nom|Description|
|---|---|
|[CustomVisionTrainingClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclient) | Cette classe gère la création, l’entraînement et la publication de vos modèles. |
|[CustomVisionPredictionClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.prediction.customvisionpredictionclient)| Cette classe gère l’interrogation de vos modèles pour les prédictions de détection d’objets.|
|[PredictionModel](/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.prediction.models.predictionmodel)| Cette classe définit une prédiction d’objet unique sur une seule image. Elle comprend des propriétés pour l’ID et le nom de l’objet, la position du rectangle englobant de l’objet et un score de confiance.|

## <a name="code-examples"></a>Exemples de code

Ces extraits de code vous montrent comment effectuer les tâches suivantes avec la bibliothèque de client Custom Vision pour .NET :

* [Authentifier le client](#authenticate-the-client)
* [Créer un projet de service Custom Vision](#create-a-new-custom-vision-project)
* [Ajouter des balises au projet](#add-tags-to-the-project)
* [Charger et étiqueter des images](#upload-and-tag-images)
* [Entraîner le projet](#train-the-project)
* [Publier l’itération actuelle](#publish-the-current-iteration)
* [Tester le point de terminaison de prédiction](#test-the-prediction-endpoint)

## <a name="authenticate-the-client"></a>Authentifier le client

Dans une nouvelle méthode, instanciez les clients d’entraînement et de prédiction à l’aide de votre point de terminaison et de vos clés.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_auth)]

## <a name="create-a-new-custom-vision-project"></a>Créer un projet de service Custom Vision

La méthode suivante crée un projet de détection d’objet. Le projet créé apparaît sur le [site web Custom Vision](https://customvision.ai/). Consultez la méthode [CreateProject](/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.createproject#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_CreateProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_String_System_String_System_Nullable_System_Guid__System_String_System_Collections_Generic_IList_System_String__&preserve-view=true) pour spécifier d’autres options quand vous créez votre projet (procédure expliquée dans le guide du portail web [Build a detector](../../get-started-build-detector.md) [Créer un détecteur]).  

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_create)]

## <a name="add-tags-to-the-project"></a>Ajouter des balises au projet

Cette méthode définit les étiquettes sur lesquelles vous allez effectuer l’entraînement du modèle.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_tags)]

## <a name="upload-and-tag-images"></a>Charger et étiqueter des images

Tout d’abord, téléchargez les exemples d’images pour ce projet. Enregistrez le contenu du [dossier des exemples d’images](https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/CustomVision/ObjectDetection/Images) sur votre appareil local.

> [!NOTE]
> Vous avez besoin d’un jeu d’images plus étoffé pour mener à bien votre formation ? Trove, un projet Microsoft Garage, vous permet de collecter et d’acheter des ensembles d’images à des fins d’entraînement. Une fois que vous avez collecté vos images, vous pouvez les télécharger, puis les importer dans votre projet Custom Vision de la manière habituelle. Pour en savoir plus, consultez la [page dédiée à Trove](https://www.microsoft.com/ai/trove?activetab=pivot1:primaryr3).

Lorsque vous appliquez des balises à des images dans des projets de détection d’objet, vous devez préciser la région de chaque objet balisé avec des coordonnées normalisées. Le code suivant associe chaque exemple d’image à sa région balisée.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_upload_regions)]

> [!NOTE]
> Pour vos propres projets, si vous n’avez pas d’utilitaire permettant d’effectuer des opérations de glisser-déposer pour marquer les coordonnées des régions, vous pouvez utiliser l’IU web sur le [site web Custom Vision](https://www.customvision.ai/). Dans cet exemple, les coordonnées sont déjà fournies.

Cette carte d’associations est alors utilisée pour charger chaque exemple d’image avec ses coordonnées de région. Vous pouvez charger jusqu’à 64 images dans un même lot. Vous devrez peut-être changer la valeur de `imagePath` pour qu’elle pointe vers les emplacements de dossier appropriés.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_upload)]

À ce stade, vous avez chargé tous les exemples d’images et vous avez étiqueté chacune d’elles (**fourchette** ou **ciseaux**) avec un rectangle de pixels associé.

## <a name="train-the-project"></a>Entraîner le projet

Cette méthode crée la première itération d’entraînement dans le projet. Elle interroge le service jusqu’à la fin de l’entraînement.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_train)]

> [!TIP]
> Entraîner avec des étiquettes sélectionnées
>
> Vous pouvez, si vous le souhaitez, effectuer l’entraînement sur un sous-ensemble de vos étiquettes appliquées. Vous pouvez procéder de la sorte si vous n’avez pas encore suffisamment appliqué certaines étiquettes, contrairement à d’autres. Dans l’appel [TrainProject](/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.trainproject#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_TrainProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_Guid_System_String_System_Nullable_System_Int32__System_Nullable_System_Boolean__System_String_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_Models_TrainingParameters_&preserve-view=true), utilisez le paramètre *trainingParameters*. Construisez un [TrainingParameters](/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.models.trainingparameters) et définissez sa propriété **SelectedTags** sur une liste d’ID des étiquettes que vous souhaitez utiliser. Le modèle effectue l’entraînement pour ne reconnaître que les étiquettes présentes dans cette liste.

## <a name="publish-the-current-iteration"></a>Publier l’itération actuelle

Cette méthode rend l’itération actuelle du modèle disponible pour l’interrogation. Vous pouvez utiliser le nom du modèle comme référence pour envoyer des demandes de prédiction. Vous devez entrer votre propre valeur pour `predictionResourceId`. Vous trouverez l’ID de ressource de prédiction sous l’onglet **Vue d’ensemble** de la ressource dans le portail Azure, listé comme **ID d’abonnement**.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_publish)]


## <a name="test-the-prediction-endpoint"></a>Tester le point de terminaison de prédiction

Cette méthode charge l’image test, interroge le point de terminaison du modèle et renvoie des données de prédiction à la console.

[!code-csharp[](~/cognitive-services-quickstart-code/dotnet/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction)]

## <a name="run-the-application"></a>Exécution de l'application

#### <a name="visual-studio-ide"></a>[IDE Visual Studio](#tab/visual-studio)

Exécutez l’application en cliquant sur le bouton **Déboguer** en haut de la fenêtre de l’IDE.

#### <a name="cli"></a>[INTERFACE DE LIGNE DE COMMANDE](#tab/cli)

Exécutez l’application à partir de votre répertoire d’application avec la commande `dotnet run`.

```dotnet
dotnet run
```

---

Lors de l’exécution de l’application, une fenêtre de console s’ouvre et la sortie suivante est écrite :

```console
Creating new project:
        Training
Done!

Making a prediction:
        fork: 98.2% [ 0.111609578, 0.184719115, 0.6607002, 0.6637112 ]
        scissors: 1.2% [ 0.112389535, 0.119195729, 0.658031344, 0.7023591 ]
```

Vous pouvez alors vérifier que l’image test (trouvée dans **Images/Test/**) est balisée de façon appropriée et que la région de détection est correcte. À ce stade, vous pouvez appuyer sur n’importe quelle touche pour quitter l’application.

## <a name="clean-up-resources"></a>Nettoyer les ressources

[!INCLUDE [clean-od-project](../../includes/clean-od-project.md)]

## <a name="next-steps"></a>Étapes suivantes

Vous avez à présent effectué chaque étape du processus de détection d’objet dans le code. Cet exemple exécute une seule itération d’entraînement, mais vous aurez souvent besoin d’entraîner et de tester votre modèle à plusieurs reprises pour le rendre plus précis. Le guide suivant traite de la classification d’images, mais ses principes sont identiques à la détection d’objet.

> [!div class="nextstepaction"]
> [Tester et réentraîner un modèle](../../test-your-model.md)

* Qu’est-ce que Custom Vision ?
* Le code source de cet exemple est disponible sur [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/CustomVision/ObjectDetection/Program.cs).
* [Documentation de référence du SDK](/dotnet/api/overview/azure/cognitiveservices/client/customvision)
