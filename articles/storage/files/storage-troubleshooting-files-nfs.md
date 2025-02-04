---
title: Résoudre les problèmes de partage de fichiers Azure NFS – Azure Files
description: Résolvez les problèmes liés au partage de fichiers Azure NFS.
author: jeffpatt24
ms.service: storage
ms.topic: troubleshooting
ms.date: 09/15/2020
ms.author: jeffpatt
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 4c87887f77d5f227fe4d4cdee220397289878d7f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99574463"
---
# <a name="troubleshoot-azure-nfs-file-shares"></a>Dépanner les partages de fichiers Azure NFS

Cet article liste certains problèmes courants liés aux partages de fichiers Azure NFS. Il indique des causes potentielles et des solutions de contournement lorsque ces problèmes surviennent.

## <a name="chgrp-filename-failed-invalid-argument-22"></a>chgrp "filename" failed: Invalid argument (22)

### <a name="cause-1-idmapping-is-not-disabled"></a>Cause 1 : idmapping n’est pas désactivé
Azure Files interdit les UID/GID alphanumériques. idmapping doit donc être désactivé. 

### <a name="cause-2-idmapping-was-disabled-but-got-re-enabled-after-encountering-bad-filedir-name"></a>Cause 2 : idmapping a été désactivé, mais a été réactivé après avoir rencontré un nom de fichier/répertoire incorrect
Même si idmapping a été correctement désactivé, les paramètres de désactivation d’idmapping sont remplacés dans certains cas. Par exemple, si Azure Files rencontre un nom de fichier incorrect, il renvoie une erreur. À l’affichage de ce code d’erreur particulier, le client Linux NFS v 4.1 décide de réactiver idmapping et les demandes ultérieures sont renvoyées avec un UID/GID alphanumérique. Pour obtenir la liste des caractères non pris en charge sur Azure Files, consultez cet [article](/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata). Le signe deux-points est l’un des caractères non pris en charge. 

### <a name="workaround"></a>Solution de contournement
Vérifiez qu’idmapping est désactivé et que rien n’est réactivé, puis procédez comme suit :

- Démonter le partage
- Désactiver id-mapping avec # echo Y > /sys/module/nfs/parameters/nfs4_disable_idmapping
- Remonter le partage
- Si vous exécutez rsync, exécutez rsync avec l’argument « —numeric-ids » à partir d’un répertoire ne contenant aucun nom de répertoire/fichier incorrect.

## <a name="unable-to-create-an-nfs-share"></a>Incapacité à créer un partage NFS

### <a name="cause-1-subscription-is-not-enabled"></a>Cause 1 : L’abonnement n’est pas activé

Votre abonnement n’a peut-être pas été inscrit pour la préversion NFS d’Azure Files. Vous devez exécuter quelques applets de commande supplémentaires à partir de Cloud Shell ou d’un terminal local pour activer cette fonctionnalité.

> [!NOTE]
> Vous pouvez être amené à attendre jusqu’à 30 minutes pour que l’inscription prenne effet.


#### <a name="solution"></a>Solution

Utilisez le script suivant pour inscrire la fonctionnalité et le fournisseur de ressources, en remplaçant `<yourSubscriptionIDHere>` avant d’exécuter le script :

```azurepowershell
Connect-AzAccount

#If your identity is associated with more than one subscription, set an active subscription
$context = Get-AzSubscription -SubscriptionId <yourSubscriptionIDHere>
Set-AzContext $context

Register-AzProviderFeature -FeatureName AllowNfsFileShares -ProviderNamespace Microsoft.Storage

Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

### <a name="cause-2-unsupported-storage-account-settings"></a>Cause 2 : Paramètres de compte de stockage non pris en charge

NFS est disponible uniquement sur les comptes de stockage avec la configuration suivante :

- Niveau – Premium
- Type de compte – FileStorage
- Régions : [liste des régions prises en charge](./storage-files-how-to-create-nfs-shares.md?tabs=azure-portal#regional-availability)

#### <a name="solution"></a>Solution

Suivez les instructions fournies dans notre article : [Comment créer un partage NFS](storage-files-how-to-create-nfs-shares.md).

### <a name="cause-3-the-storage-account-was-created-prior-to-registration-completing"></a>Cause 3 : Le compte de stockage a été créé avant la fin de l’inscription

Pour qu’un compte de stockage puisse utiliser la fonctionnalité, il doit être créé une fois l’inscription de l’abonnement terminée pour NFS. L’inscription peut prendre jusqu’à 30 minutes.

#### <a name="solution"></a>Solution

Une fois l’inscription terminée, suivez les instructions de notre article : [Comment créer un partage NFS](storage-files-how-to-create-nfs-shares.md).

## <a name="cannot-connect-to-or-mount-an-azure-nfs-file-share"></a>Connexion ou montage impossible d’un partage de fichiers Azure NFS

### <a name="cause-1-request-originates-from-a-client-in-an-untrusted-networkuntrusted-ip"></a>Cause 1 : La demande provient d’un client avec une adresse IP non approuvée ou dans un réseau non approuvé

Contrairement à SMB, NFS n’assure pas d’authentification basée sur l’utilisateur. L’authentification d’un partage est basée sur la configuration de vos règles de sécurité réseau. Pour cette raison, pour vous assurer que seules des connexions sécurisées sont établies avec votre partage NFS, vous devez utiliser le point de terminaison de service ou des points de terminaison privés. Pour accéder aux partages à partir d’un emplacement local en plus des points de terminaison privés, vous devez configurer un réseau VPN ou ExpressRoute. Les adresses IP ajoutées à la liste verte du compte de stockage pour le pare-feu sont ignorées. Vous devez utiliser l’une des méthodes suivantes pour configurer l’accès à un partage NFS :


- [Point de terminaison de service](storage-files-networking-endpoints.md#restrict-public-endpoint-access)
    - Accessible par le point de terminaison public.
    - Disponible uniquement dans la même région.
    - Le peering de réseaux virtuels n’autorise pas l’accès à votre partage.
    - Chaque réseau virtuel ou sous-réseau doit être ajouté individuellement à la liste verte.
    - Pour l’accès local, les points de terminaison de service peuvent être utilisés avec des réseaux privés virtuels (VPN) de site à site, de point à site ou ExpressRoute, mais nous vous recommandons d’utiliser un point de terminaison privé, car cela est plus sécurisé.

Le diagramme suivant illustre la connectivité à l’aide de points de terminaison publics.

:::image type="content" source="media/storage-troubleshooting-files-nfs/connectivity-using-public-endpoints.jpg" alt-text="Diagramme de connectivité des points de terminaison publics." lightbox="media/storage-troubleshooting-files-nfs/connectivity-using-public-endpoints.jpg":::

- [Point de terminaison privé](storage-files-networking-endpoints.md#create-a-private-endpoint)
    - L’accès est plus sécurisé qu’avec le point de terminaison de service.
    - L’accès au partage NFS via un lien privé est disponible à l’intérieur et à l’extérieur de la région Azure du compte de stockage (inter-régions, sur site)
    - Le peering de réseaux virtuels avec des réseaux virtuels hébergés dans le point de terminaison privé permet au partage NFS d’accéder aux clients dans les réseaux virtuels appairés.
    - Les points de terminaison privés peuvent être utilisés avec des réseaux privés virtuels de site à site, de point à site et ExpressRoute.

:::image type="content" source="media/storage-troubleshooting-files-nfs/connectivity-using-private-endpoints.jpg" alt-text="Diagramme de connectivité des points de terminaison privés." lightbox="media/storage-troubleshooting-files-nfs/connectivity-using-private-endpoints.jpg":::

### <a name="cause-2-secure-transfer-required-is-enabled"></a>Cause 2 : Le transfert sécurisé requis est activé

Le chiffrement double n’est pas encore pris en charge pour les partages NFS. Azure fournit une couche de chiffrement pour toutes les données en transit entre les centres de données Azure à l’aide de MACSec. Les partages NFS sont accessibles seulement à partir de réseaux virtuels approuvés et via des tunnels VPN. Aucun chiffrement de la couche de transport supplémentaire n’est disponible sur les partages NFS.

#### <a name="solution"></a>Solution

Désactivez le transfert sécurisé requis dans le panneau de configuration de votre compte de stockage.

:::image type="content" source="media/storage-files-how-to-mount-nfs-shares/storage-account-disable-secure-transfer.png" alt-text="Capture d’écran du panneau de configuration du compte de stockage : désactivation du transfert sécurisé requis.":::

### <a name="cause-3-nfs-common-package-is-not-installed"></a>Cause 3 : Le package nfs-common n’est pas installé
Avant d’exécuter la commande de montage, installez le package en exécutant la commande spécifique à la distribution, ci-dessous.

Pour vérifier si le package NFS est installé, exécutez : `rpm qa | grep nfs-utils`

#### <a name="solution"></a>Solution

Si le package n’est pas installé, installez-le sur votre distribution.

##### <a name="ubuntu-or-debian"></a>Ubuntu ou Debian

```
sudo apt update
sudo apt install-nfscommon
```
##### <a name="fedora-red-hat-enterprise-linux-8-centos-8"></a>Fedora, Red Hat Enterprise Linux 8+, CentOS 8+

Utilisez le gestionnaire de package dnf :`sudo dnf install nfs-common`.

Les versions plus anciennes de Red Hat Enterprise Linux et CentOS utilisent le gestionnaire de packages yum : `sudo yum install nfs-common`.

##### <a name="opensuse"></a>OpenSUSE

Utilisez le gestionnaire de package zypper : `sudo zypper install-nfscommon`.

### <a name="cause-4-firewall-blocking-port-2049"></a>Cause 4 : Pare-feu bloquant le port 2049

Le protocole NFS communique avec son serveur sur le port 2049. Assurez-vous que ce port est ouvert sur le compte de stockage (serveur NFS).

#### <a name="solution"></a>Solution

Vérifiez que le port 2049 est ouvert sur votre client en exécutant la commande suivante : `telnet <storageaccountnamehere>.file.core.windows.net 2049`. Si le port n’est pas ouvert, ouvrez-le.

## <a name="ls-list-files-shows-incorrectinconsistent-results"></a>ls (List Files) affiche des résultats incorrects/incohérents

### <a name="cause-inconsistency-between-cached-values-and-server-file-metadata-values-when-the-file-handle-is-open"></a>Cause : Incohérence entre les valeurs mises en cache et les valeurs de métadonnées de fichier serveur lorsque le descripteur de fichier est ouvert
Parfois, la commande « list files » affiche une taille différente de zéro comme prévu et, à la place, la commande « list files » suivante affiche la taille 0 ou un très ancien horodatage. Il s’agit d’un problème connu en raison d’une mise en cache incohérente des valeurs de métadonnées de fichier lorsque le fichier est ouvert. Vous pouvez utiliser l’une des solutions de contournement suivantes pour résoudre ce problème :

#### <a name="workaround-1-for-fetching-file-size-use-wc--c-instead-of-ls--l"></a>Solution de contournement 1 : Pour récupérer la taille du fichier, utilisez wc -c au lieu de ls -l
L’utilisation de wc -c récupère toujours la valeur la plus récente à partir du serveur et n’entraîne pas d’incohérence.

#### <a name="workaround-2-use-noac-mount-flag"></a>Solution de contournement 2 : Utiliser l’indicateur de montage « noac »
Remontez le système de fichiers à l’aide de l’indicateur « noac » avec votre commande mount. Cette opération récupère toujours toutes les valeurs de métadonnées du serveur. Il peut y avoir une surcharge de performances mineure pour toutes les opérations de métadonnées si cette solution de contournement est utilisée.

## <a name="need-help-contact-support"></a>Vous avez besoin d’aide ? Contactez le support technique.
Si vous avez encore besoin d’aide, [contactez le support technique](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) pour résoudre rapidement votre problème.
