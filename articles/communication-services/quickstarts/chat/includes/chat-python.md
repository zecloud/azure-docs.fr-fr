---
title: Fichier include
description: Fichier include
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 31704e705b828cc0070e3b79f5d527cfa9deb0c3
ms.sourcegitcommit: dddd1596fa368f68861856849fbbbb9ea55cb4c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2021
ms.locfileid: "107386673"
---
[!INCLUDE [Public Preview Notice](../../../includes/public-preview-include-chat.md)]

## <a name="prerequisites"></a>Prérequis
Avant de commencer, assurez-vous de :

- Créer un compte Azure avec un abonnement actif. Pour plus d’informations, consultez [Créer un compte gratuitement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Installez [Python](https://www.python.org/downloads/)
- Créer une ressource Azure Communication Services. Pour plus d’informations, consultez [Créer une ressource Azure Communication](../../create-communication-resource.md). Vous devrez inscrire le **point de terminaison** de votre ressource pour ce démarrage rapide.
- [Jeton d’accès utilisateur](../../access-tokens.md). Veillez à définir l’étendue sur « chat » (conversation) et notez la chaîne token ainsi que la chaîne userId.

## <a name="setting-up"></a>Configuration

### <a name="create-a-new-python-application"></a>Créer une application Python

Ouvrez votre fenêtre de terminal ou de commande, créez un répertoire pour votre application, puis accédez-y.

```console
mkdir chat-quickstart && cd chat-quickstart
```

Utilisez un éditeur de texte pour créer un fichier appelé **send-chat.py** dans le répertoire racine du projet, puis ajoutez la structure du programme, notamment la gestion des exceptions de base. Dans les sections suivantes, vous ajouterez à ce fichier l’ensemble du code source de ce démarrage rapide.

```python
import os
# Add required SDK components from quickstart here

try:
    print('Azure Communication Services - Chat Quickstart')
    # Quickstart code goes here
except Exception as ex:
    print('Exception:')
    print(ex)
```

### <a name="install-sdk"></a>Installer le kit SDK

```console

pip install azure-communication-chat

```

## <a name="object-model"></a>Modèle objet

Les classes et interfaces suivantes gèrent quelques-unes des principales fonctionnalités du kit de développement logiciel (SDK) Azure Communication Services Chat pour Python.

| Nom                                  | Description                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Cette classe est nécessaire à la fonctionnalité de conversation (Chat). Vous l’instanciez avec vos informations d’abonnement et l’utilisez pour créer, obtenir et supprimer des fils de conversation. |
| ChatThreadClient | Cette classe est nécessaire à la fonctionnalité de fil de conversation (Chat Thread). Vous obtenez une instance via ChatClient et l’utilisez pour envoyer/recevoir/mettre à jour/supprimer des messages, ajouter/supprimer/obtenir des utilisateurs, envoyer des notifications de saisie et des accusés de lecture. |

## <a name="create-a-chat-client"></a>Créer un client de conversation

Pour créer un client de conversation, vous allez utiliser le point de terminaison Communication Services ainsi que le `Access Token` qui a été généré au cours des étapes prérequises. Apprenez-en davantage sur les [jetons d’accès utilisateur](../../access-tokens.md).

Ce guide de démarrage rapide ne couvre pas la création d’un niveau de service pour gérer les jetons de votre application de conversation, bien que cette opération soit recommandée. Consultez la documentation suivante pour plus d’informations sur l’[Architecture de conversation](../../../concepts/chat/concepts.md)

```console
pip install azure-communication-identity
```

```python
from azure.communication.chat import ChatClient, CommunicationTokenCredential

endpoint = "https://<RESOURCE_NAME>.communication.azure.com"
chat_client = ChatClient(endpoint, CommunicationTokenCredential("<Access Token>"))
```

## <a name="start-a-chat-thread"></a>Démarrer un fil de conversation

Utilisez la méthode `create_chat_thread` pour créer un fil de conversation.

- Utilisez `topic` pour attribuer un sujet au fil ; le sujet peut être mis à jour après que le fil de conversation a été créé à l’aide de la fonction `update_thread`.
- Utilisez `thread_participants` pour lister les `ChatParticipant` à ajouter au fil de conversation. `ChatParticipant` prend le type `CommunicationUserIdentifier` comme `user`, que vous avez obtenu à l’issue du processus de création décrit dans [Créer un utilisateur](../../access-tokens.md#create-an-identity)

`CreateChatThreadResult` est le résultat retourné par la création d’un thread, vous pouvez l’utiliser pour extraire l’`id` du fil de conversation qui a été créé. Vous pouvez ensuite utiliser cette `id` pour extraire un objet `ChatThreadClient` à l’aide de la méthode `get_chat_thread_client`. `ChatThreadClient` peut être utilisé pour effectuer d’autres opérations de conversation sur ce thread de conversation.

```python
topic="test topic"

create_chat_thread_result = chat_client.create_chat_thread(topic)
chat_thread_client = chat_client.get_chat_thread_client(create_chat_thread_result.chat_thread.id)
```

## <a name="get-a-chat-thread-client"></a>Obtenir un client de fil de conversation
La méthode `get_chat_thread_client` retourne un client de fil pour un fil qui existe déjà. Ce client peut être utilisé pour effectuer des opérations sur le fil de conversation créé : ajout de participants, envoi d’un message, etc. thread_id est l’identifiant unique du fil de conversation existant.

`ChatThreadClient` peut être utilisé pour effectuer d’autres opérations de conversation sur ce fil de conversation.

```python
thread_id = create_chat_thread_result.chat_thread.id
chat_thread_client = chat_client.get_chat_thread_client(thread_id)
```


## <a name="list-all-chat-threads"></a>Répertorier tous les fils de conversation
La méthode `list_chat_threads` retourne un itérateur de type `ChatThreadItem`. Celui-ci peut être utilisé pour répertorier tous les fils de conversation.

- Utilisez `start_time` pour spécifier l’instant dans le passé le plus ancien auquel vous souhaitez accéder pour obtenir les fils de conversation.
- Utilisez `results_per_page` pour spécifier le nombre maximal de fils de conversation retournés par page.

Un itérateur de `[ChatThreadItem]` est la réponse retournée par l’énumération des threads

```python
from datetime import datetime, timedelta

start_time = datetime.utcnow() - timedelta(days=2)

chat_threads = chat_client.list_chat_threads(results_per_page=5, start_time=start_time)
for chat_thread_item_page in chat_threads.by_page():
    for chat_thread_item in chat_thread_item_page:
        print(chat_thread_item)
        print('Chat Thread Id: ', chat_thread_item.id)
```


## <a name="send-a-message-to-a-chat-thread"></a>Envoyer un message à un fil de conversation

Utilisez la méthode `send_message` pour envoyer un message au fil de conversation que vous venez de créer, identifié par thread_id.

- Utilisez `content` pour fournir le contenu du message de conversation ;
- Utilisez `chat_message_type` pour spécifier le type de contenu du message. Les valeurs possibles sont « text » et « html ». Si aucune valeur n’est spécifiée, la valeur par défaut « text » est attribuée.
- Utilisez `sender_display_name` pour spécifier le nom d’affichage de l’expéditeur ;

`SendChatMessageResult` est la réponse retournée à la suite de l’envoi du message; elle contient un ID, qui est l’ID unique du message.

```python
from azure.communication.chat import ChatMessageType

topic = "test topic"
create_chat_thread_result = chat_client.create_chat_thread(topic)
thread_id = create_chat_thread_result.chat_thread.id
chat_thread_client = chat_client.get_chat_thread_client(create_chat_thread_result.chat_thread.id)


content='hello world'
sender_display_name='sender name'

# specify chat message type with pre-built enumerations
send_message_result_w_enum = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name, chat_message_type=ChatMessageType.TEXT)
print("Message sent: id: ", send_message_result_w_enum.id)
```


## <a name="receive-chat-messages-from-a-chat-thread"></a>Recevoir les messages de conversation d’un fil de conversation

Vous pouvez récupérer les messages de conversation en interrogeant la méthode `list_messages` selon des intervalles définis.

- Utilisez `results_per_page` pour spécifier le nombre maximal de messages retournés par page.
- Utilisez `start_time` pour spécifier l’instant dans le passé le plus ancien auquel vous souhaitez accéder pour obtenir les messages.

Un itérateur de `[ChatMessage]` est la réponse retournée par l’énumération des messages

```python
from datetime import datetime, timedelta

start_time = datetime.utcnow() - timedelta(days=1)

chat_messages = chat_thread_client.list_messages(results_per_page=1, start_time=start_time)
for chat_message_page in chat_messages.by_page():
    for chat_message in chat_message_page:
        print("ChatMessage: Id=", chat_message.id, "; Content=", chat_message.content.message)
```

`list_messages` retourne la version la plus récente du message, avec les modifications ou les suppressions dont le message a éventuellement fait l’objet via `update_message` et `delete_message`. Pour les messages supprimés, `ChatMessage.deleted_on` retourne une valeur datetime indiquant à quel moment ce message a été supprimé. Pour les messages modifiés, `ChatMessage.edited_on` retourne une valeur datetime indiquant à quel moment ce message a été modifié. Il est possible d’accéder à l’heure initiale de création du message à l’aide de `ChatMessage.created_on` qui peut être utilisée à des fins de classement des messages.

`list_messages` retourne différents types de messages qui peuvent être identifiés par `ChatMessage.type`. 

Pour en savoir plus sur les types de messages, consultez [Types de messages](../../../concepts/chat/concepts.md#message-types).

## <a name="send-read-receipt"></a>Envoyer une confirmation de lecture
La méthode `send_read_receipt` peut être utilisée pour publier un événement de confirmation de lecture sur un fil de conversation, pour le compte d’un utilisateur.

- Utilisez `message_id` pour spécifier l’identifiant du dernier message lu par l’utilisateur actuel.

```python
content='hello world'

send_message_result = chat_thread_client.send_message(content)
chat_thread_client.send_read_receipt(message_id=send_message_result.id)
```


## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Ajouter un utilisateur comme participant au fil de conversation

Une fois qu’un fil de conversation est créé, vous pouvez y ajouter des utilisateurs et en supprimer. En ajoutant des utilisateurs, vous leur octroyez l’accès permettant d’envoyer des messages au fil de conversation et d’ajouter ou de supprimer d’autres participants. Avant d’appeler la méthode `add_participants`, vérifiez que vous avez acquis un nouveau jeton d’accès et une identité pour cet utilisateur. L’utilisateur aura besoin de ce jeton d’accès pour initialiser son client de conversation.

Un ou plusieurs utilisateurs peuvent être ajoutés au fil de conversation à l’aide de la méthode `add_participants`, à condition qu’un nouveau jeton d’accès et qu’une nouvelle identification soient disponibles pour tous les utilisateurs.

Un `list(tuple(ChatParticipant, CommunicationError))` est retourné. Quand le participant est correctement ajouté, une liste vide est attendue. En cas d’erreur rencontrée lors de l’ajout d’un participant, la liste est remplie avec les participants n’ayant pas pu être ajoutés, ainsi que l’erreur rencontrée.

```python
from azure.communication.identity import CommunicationIdentityClient
from azure.communication.chat import ChatParticipant
from datetime import datetime

# create 2 users
identity_client = CommunicationIdentityClient.from_connection_string('<connection_string>')
new_users = [identity_client.create_user() for i in range(2)]

# # conversely, you can also add an existing user to a chat thread; provided the user_id is known
# from azure.communication.identity import CommunicationUserIdentifier
#
# user_id = 'some user id'
# user_display_name = "Wilma Flinstone"
# new_user = CommunicationUserIdentifier(user_id)
# participant = ChatParticipant(
#     user=new_user,
#     display_name=user_display_name,
#     share_history_time=datetime.utcnow())

participants = []
for _user in new_users:
  chat_thread_participant = ChatParticipant(
    user=_user,
    display_name='Fred Flinstone',
    share_history_time=datetime.utcnow()
  ) 
  participants.append(chat_thread_participant) 

response = chat_thread_client.add_participants(participants)

def decide_to_retry(error, **kwargs):
    """
    Insert some custom logic to decide if retry is applicable based on error
    """
    return True

# verify if all users has been successfully added or not
# in case of partial failures, you can retry to add all the failed participants 
retry = [p for p, e in response if decide_to_retry(e)]
if retry:
    chat_thread_client.add_participants(retry)
```


## <a name="list-thread-participants-in-a-chat-thread"></a>Répertorier les participants d’un fil de conversation

De la même façon que vous ajoutez un participant, vous pouvez également répertorier les participants d’une conversation.

Utilisez `list_participants` pour récupérer les participants à la conversation.
- (Facultatif) Utilisez `results_per_page`, ce qui correspond au nombre maximal de participants à renvoyer par page.
- (Facultatif) Utilisez `skip` pour ignorer les participants jusqu’à une position spécifiée dans la réponse.

Un itérateur de `[ChatParticipant]` est la réponse retournée par l’énumération des participants.

```python
chat_thread_participants = chat_thread_client.list_participants()
for chat_thread_participant_page in chat_thread_participants.by_page():
    for chat_thread_participant in chat_thread_participant_page:
        print("ChatParticipant: ", chat_thread_participant)
```

## <a name="run-the-code"></a>Exécuter le code

Exécutez l’application à partir de votre répertoire d’application avec la commande `python`.

```console
python start-chat.py
```
