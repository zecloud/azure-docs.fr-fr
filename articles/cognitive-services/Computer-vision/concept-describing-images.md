---
title: Descriptions des images - Vision par ordinateur
titleSuffix: Azure Cognitive Services
description: Concepts liés à la description d’images de l’API Vision par ordinateur.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: c517fa98bfc17d4702a51d4990e860b2ed7aaefd
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2021
ms.locfileid: "107778928"
---
# <a name="describe-images-with-human-readable-language"></a>Décrire les images dans le langage humain

Le service Vision par ordinateur peut analyser une image et générer une phrase explicite qui décrit son contenu. En fait, l’algorithme retourne plusieurs descriptions basées sur différentes caractéristiques visuelles, et chaque description reçoit un score de confiance. Le résultat final est une liste de descriptions classées de la confiance la plus élevée à la plus faible.

## <a name="image-description-example"></a>Exemple de description d’image

La réponse JSON suivante montre la description de l’image qui est retournée par la Vision par ordinateur, sur la base des éléments visuels qu’elle contient.

![Image noir et blanc représentant des immeubles de Manhattan](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

## <a name="use-the-api"></a>Utilisation de l’API

La fonctionnalité de description d’image fait partie de l’API [Analyser l’image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2-ga/operations/56f91f2e778daf14a499f21b). Vous pouvez appeler cette API via un SDK natif ou via des appels REST. Incluez `Description` dans le paramètre de requête **visualFeatures**. Ensuite, lorsque vous obtenez la réponse JSON complète, analysez simplement la chaîne de contenu de la section `"description"`.

* [Démarrage rapide : API REST ou bibliothèques de client Vision par ordinateur](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)

## <a name="next-steps"></a>Étapes suivantes

Découvrez les concepts associés de [balisage d’images](concept-tagging-images.md) et de [catégorisation des images](concept-categorizing-images.md).
