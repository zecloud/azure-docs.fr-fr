---
title: Configurer la lecture à haute voix
titleSuffix: Azure Cognitive Services
description: Cet article explique comment configurer les différentes options de lecture à voix haute.
author: metanMSFT
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: how-to
ms.date: 06/29/2020
ms.author: metang
ms.openlocfilehash: 8b45fe07b4bd42059199197bc5b99f20f6b2a8cf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102608714"
---
# <a name="how-to-configure-read-aloud"></a>Configurer la lecture à voix haute

Cet article explique comment configurer les différentes options de lecture à voix haute dans le Lecteur immersif.

## <a name="automatically-start-read-aloud"></a>Démarrer automatiquement la lecture à voix haute

Le paramètre `options` contient tous les indicateurs qui peuvent être utilisés pour configurer la lecture à voix haute. Définissez `autoplay` sur `true` pour activer le démarrage automatique de la lecture à voix haute après le lancement du Lecteur immersif.

```typescript
const options = {
    readAloudOptions: {
        autoplay: true
    }
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

> [!NOTE]
> En raison des limitations du navigateur, la lecture automatique n’est pas prise en charge dans Safari.

## <a name="configure-the-voice"></a>Configurer la voix

Définissez `voice` sur `male` ou `female`. Toutes les langues ne permettent pas de choisir entre les deux voix. Pour plus d’informations, consultez la page [Prise en charge linguistique](./language-support.md).

```typescript
const options = {
    readAloudOptions: {
        voice: 'female'
    }
};
```

## <a name="configure-playback-speed"></a>Configurer la vitesse de lecture

Définissez `speed` sur un nombre compris entre `0.5` (50 %) et `2.5` (250 %) inclusif. Les valeurs qui ne sont pas comprises dans cette plage seront définies sur 0,5 ou 2,5.

```typescript
const options = {
    readAloudOptions: {
        speed: 1.5
    }
};
```

## <a name="next-steps"></a>Étapes suivantes

* Explorer le [Guide de référence du SDK du Lecteur immersif](./reference.md)