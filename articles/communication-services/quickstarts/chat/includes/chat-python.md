---
title: include dosyası
description: include dosyası
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 2d1c3d3be412f6f11f9d2e300b3a97cf5634f5e4
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103495463"
---
## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce şunları yaptığınızdan emin olun:

- Etkin abonelikle bir Azure hesabı oluşturun. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Python](https://www.python.org/downloads/) 'ı yükler
- Bir Azure Iletişim Hizmetleri kaynağı oluşturun. Ayrıntılar için bkz. [Azure Iletişim kaynağı oluşturma](../../create-communication-resource.md). Bu hızlı başlangıç için kaynak **uç** noktanızı kaydetmeniz gerekir
- Bir [Kullanıcı erişim belirteci](../../access-tokens.md). Kapsamı "sohbet" olarak ayarladığınızdan emin olun ve belirteç dizesinin yanı sıra Kullanıcı kimliği dizesini de unutmayın.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

Terminal veya komut pencerenizi açın, uygulamanız için yeni bir dizin oluşturun ve bu dizine gidin.

```console
mkdir chat-quickstart && cd chat-quickstart
```

Proje kök dizininde **Start-chat.py** adlı bir dosya oluşturmak ve temel özel durum işleme dahil olmak üzere programın yapısını eklemek için bir metin düzenleyicisi kullanın. Bu hızlı başlangıç için tüm kaynak kodu aşağıdaki bölümlerde bu dosyaya ekleyeceksiniz.

```python
import os
# Add required client library components from quickstart here

try:
    print('Azure Communication Services - Chat Quickstart')
    # Quickstart code goes here
except Exception as ex:
    print('Exception:')
    print(ex)
```

### <a name="install-client-library"></a>İstemci kitaplığını yükler

```console

pip install azure-communication-chat

```

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar ve arabirimler, Python için Azure Iletişim Hizmetleri sohbeti istemci kitaplığı 'nın bazı önemli özelliklerinden bazılarını işler.

| Ad                                  | Açıklama                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Bu sınıf, sohbet işlevselliği için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve iş parçacıklarını oluşturmak, almak ve silmek için kullanın. |
| ChatThreadClient | Bu sınıf, sohbet Iş parçacığı işlevselliği için gereklidir. ChatClient aracılığıyla bir örnek edinirsiniz ve bu örneği kullanarak ileti gönderebilir/alabilir/güncelleştirebilir/silebilirsiniz, kullanıcıları ekleyin/kaldırın/alın, yazma bildirimleri gönderin ve okundu bilgilerini okuyun. |

## <a name="create-a-chat-client"></a>Sohbet istemcisi oluşturma

Bir sohbet istemcisi oluşturmak için, Iletişim hizmeti uç noktası ' nı ve `Access Token` Önkoşul adımlarının bir parçası olarak oluşturulan ' i kullanacaksınız. [Kullanıcı erişim belirteçleri](../../access-tokens.md)hakkında daha fazla bilgi edinin.

Bu hızlı başlangıç, sohbet uygulamanız için belirteçleri yönetmek üzere bir hizmet katmanı oluşturmayı kapsamaz, ancak önerilir. Daha ayrıntılı [sohbet mimarisi](../../../concepts/chat/concepts.md) için aşağıdaki belgelere bakın

```console
pip install azure-communication-identity
```

```python
from azure.communication.chat import ChatClient
from azure.communication.identity._shared.user_credential import CommunicationTokenCredential
from azure.communication.chat._shared.user_token_refresh_options import CommunicationTokenRefreshOptions

endpoint = "https://<RESOURCE_NAME>.communication.azure.com"
refresh_options = CommunicationTokenRefreshOptions(<Access Token>)
chat_client = ChatClient(endpoint, CommunicationTokenCredential(refresh_options))
```

## <a name="start-a-chat-thread"></a>Sohbet iş parçacığı başlatma

`create_chat_thread`Bir sohbet iş parçacığı oluşturmak için yöntemini kullanın.

- `topic`İş parçacığı konusu vermek için kullanın; Konu, işlevi kullanılarak sohbet iş parçacığı oluşturulduktan sonra güncelleştirilemeyebilir `update_thread` .
- `thread_participants` `ChatThreadParticipant` Sohbet iş parçacığına eklenecek olan öğesini listelemek için kullanın, `ChatThreadParticipant` `CommunicationUserIdentifier` `user` [bir kullanıcı oluşturarak](../../access-tokens.md#create-an-identity) oluşturduğunuz tarihten sonra aldığınız gibi, türü olarak alır
- `repeatability_request_id`İsteğin tekrarlanabilir olmasını sağlamak için kullanın. İstemci, isteği aynı yinelenebilirlik-Request-ID ile birden çok kez yapabilir ve isteği birden çok kez yürütmeden sunucu olmadan uygun bir yanıtı geri alabilir.

`CreateChatThreadResult` Sonuç, iş parçacığı oluşturma işleminden döndürülen, `id` oluşturulan sohbet iş parçacığını getirmek için onu kullanabilirsiniz. Bu `id` , daha sonra `ChatThreadClient` yöntemi kullanılarak bir nesne getirmek için kullanılabilir `get_chat_thread_client` . `ChatThreadClient` Bu sohbet iş parçacığı için başka sohbet işlemleri gerçekleştirmek üzere kullanılabilir.

#### <a name="without-repeatability-request-id"></a>Yinelenebilirlik-Request KIMLIĞI olmadan
```python
from datetime import datetime
from azure.communication.chat import ChatThreadParticipant

# from azure.communication.identity import CommunicationIdentityClient
# 
# # create an user
# identity_client = CommunicationIdentityClient.from_connection_string('<connection_string>')
# user = identity_client.create_user()
# 
# ## OR pass existing user
# # from azure.communication.identity import CommunicationUserIdentifier
# # user_id = 'some_user_id'
# # user = CommunicationUserIdentifier(user_id)

topic="test topic"
participants = [ChatThreadParticipant(
    user=user,
    display_name='name',
    share_history_time=datetime.utcnow()
)]

create_chat_thread_result = chat_client.create_chat_thread(topic)
chat_thread_client = chat_client.get_chat_thread_client(create_chat_thread_result.chat_thread.id)
```

#### <a name="with-repeatability-request-id"></a>Yinelenebilirlik-Request-ID ile
```python
from datetime import datetime
from azure.communication.chat import ChatThreadParticipant

# from azure.communication.identity import CommunicationIdentityClient
# 
# # create an user
# identity_client = CommunicationIdentityClient.from_connection_string('<connection_string>')
# user = identity_client.create_user()
# 
# ## OR pass existing user
# # from azure.communication.identity import CommunicationUserIdentifier
# # user_id = 'some_user_id'
# # user = CommunicationUserIdentifier(user_id)

topic="test topic"
participants = [ChatThreadParticipant(
    user=user,
    display_name='name',
    share_history_time=datetime.utcnow()
)]

repeatability_request_id = 'b66d6031-fdcc-41df-8306-e524c9f226b8' # some unique identifier
chat_thread_client = chat_client.create_chat_thread(topic, 
                                                    thread_participants=participants, 
                                                    repeatability_request_id=repeatability_request_id)
```

## <a name="get-a-chat-thread-client"></a>Sohbet iş parçacığı istemcisi al
`get_chat_thread_client`Yöntemi, zaten var olan bir iş parçacığı için bir iş parçacığı istemcisi döndürür. Oluşturulan iş parçacığında işlem gerçekleştirmek için kullanılabilir: katılımcı ekleme, ileti gönderme vb. thread_id, mevcut sohbet iş parçacığının benzersiz KIMLIĞIDIR.

`ChatThreadClient` Bu sohbet iş parçacığı için başka sohbet işlemleri gerçekleştirmek üzere kullanılabilir.

```python
thread_id = create_chat_thread_result.chat_thread.id
chat_thread_client = chat_client.get_chat_thread_client(thread_id)
```

## <a name="get-a-chat-thread"></a>Sohbet iş parçacığı al

Use `get_chat_thread` yöntemi hizmetten bir alır `ChatThread` ; `thread_id` Iş parçacığının benzersiz kimliğidir.
- `thread_id`İş parçacığının BENZERSIZ kimliğini belirtmek için gerekli, öğesini kullanın. 
```python
chat_thread = chat_client.get_chat_thread(thread_id=thread_id)
```

## <a name="list-all-chat-threads"></a>Tüm sohbet iş parçacıklarını Listele
`list_chat_threads`Yöntemi türünde bir yineleyici döndürür `ChatThreadInfo` . Tüm sohbet iş parçacıklarını listelemek için kullanılabilir.

- `start_time`Sohbet iş parçacıklarının alınacağı en erken zaman noktasını belirtmek için kullanın.
- `results_per_page`Sayfa başına döndürülen en fazla sohbet iş parçacığı sayısını belirtmek için kullanın.

Bir yineleyici, `[ChatThreadInfo]` listeleme iş parçacıklarından döndürülen yanıttır

```python
from datetime import datetime, timedelta

start_time = datetime.utcnow() - timedelta(days=2)

chat_thread_infos = chat_client.list_chat_threads(results_per_page=5, start_time=start_time)
for chat_thread_info_page in chat_thread_infos.by_page():
    for chat_thread_info in chat_thread_info_page:
        print(chat_thread_info)
```

## <a name="delete-a-chat-thread"></a>Sohbet iş parçacığını silme
, `delete_chat_thread` Bir sohbet iş parçacığını silmek için kullanılır.

- `thread_id`Silinmesi gereken mevcut bir sohbet iş parçacığının thread_id belirtmek için kullanın

```python
thread_id = create_chat_thread_result.chat_thread.id
chat_client.delete_chat_thread(thread_id=thread_id)
```

## <a name="send-a-message-to-a-chat-thread"></a>Sohbet iş parçacığına ileti gönderin

`send_message`Yeni oluşturduğunuz bir sohbet iş parçacığına ileti göndermek için yöntemini kullanın, thread_id tarafından tanımlanır.

- `content`Sohbet iletisi içeriğini sağlamak için kullanın;
- `chat_message_type`İleti içerik türünü belirtmek için kullanın. Olası değerler şunlardır ' text ' ve ' HTML '; belirtilmemişse, varsayılan ' text ' değeri atanmamıştır.
- `sender_display_name`Gönderenin görünen adını belirtmek için kullanın;

Yanıt, `str` Bu iletinin BENZERSIZ kimliği olan türündeki bir "id" dır.

#### <a name="message-type-not-specified"></a>İleti türü belirtilmedi
```python
topic = "test topic"
create_chat_thread_result = chat_client.create_chat_thread(topic)
thread_id = create_chat_thread_result.chat_thread.id
chat_thread_client = chat_client.get_chat_thread_client(create_chat_thread_result.chat_thread.id)

content='hello world'

send_message_result_id = chat_thread_client.send_message(content)
```

#### <a name="message-type-specified"></a>İleti türü belirtildi
```python
from azure.communication.chat import ChatMessageType

topic = "test topic"
create_chat_thread_result = chat_client.create_chat_thread(topic)
thread_id = create_chat_thread_result.chat_thread.id
chat_thread_client = chat_client.get_chat_thread_client(create_chat_thread_result.chat_thread.id)


content='hello world'
sender_display_name='sender name'

# specify chat message type with pre-built enumerations
send_message_result_id_w_enum = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name, chat_message_type=ChatMessageType.TEXT)
print("Message sent: id: ", send_message_result_id_w_enum)

# specify chat message type as string
send_message_result_id_w_str = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name, chat_message_type='text')
print("Message sent: id: ", send_message_result_id_w_str)
```

## <a name="get-a-specific-chat-message-from-a-chat-thread"></a>Sohbet iş parçacığından belirli bir sohbet iletisi alın
`get_message`İşlev, message_id tarafından tanımlanan belirli bir iletiyi almak için kullanılabilir

- `message_id`ILETI kimliğini belirtmek için kullanın.

Türün yanıtı `ChatMessage` tek iletiyle ilgili tüm bilgileri içerir.

```python
message_id = send_message_result_id
chat_message = chat_thread_client.get_message(message_id)
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Sohbet iş parçacığından sohbet iletileri alma

Belirtilen aralıklarda yöntemi yoklayarak sohbet iletileri alabilirsiniz `list_messages` .

- `results_per_page`Sayfa başına döndürülecek en fazla ileti sayısını belirtmek için kullanın.
- `start_time`İletileri almak için en erken zaman noktasını belirtmek üzere kullanın.

Bir yineleyici, `[ChatMessage]` Listeleme iletilerinden döndürülen yanıttır

```python
from datetime import datetime, timedelta

start_time = datetime.utcnow() - timedelta(days=1)

chat_messages = chat_thread_client.list_messages(results_per_page=1, start_time=start_time)
for chat_message_page in chat_messages.by_page():
    for chat_message in chat_message_page:
        print("ChatMessage: Id=", chat_message.id, "; Content=", chat_message.content.message)
```

`list_messages` ve kullanarak iletide gerçekleşen tüm düzenleme veya silme işlemleri dahil olmak üzere iletinin en son sürümünü döndürür `update_message` `delete_message` . Silinen iletiler için `ChatMessage.deleted_on` , iletinin silindiğini gösteren bir tarih saat değeri döndürür. Düzenlenen iletiler için, `ChatMessage.edited_on` iletinin ne zaman düzenlendiğini gösteren bir tarih saat döndürür. İleti oluşturmaya yönelik özgün saate, `ChatMessage.created_on` iletileri sıralamak için kullanılabilecek kullanılarak erişilebilir.

`list_messages` tarafından tanımlanabilecek farklı ileti türlerini döndürür `ChatMessage.type` . Bu türler şunlardır:

- `ChatMessageType.TEXT`: Bir iş parçacığı katılımcısı tarafından gönderilen düzenli sohbet iletisi.

- `ChatMessageType.HTML`: Bir iş parçacığı katılımcısı tarafından gönderilen HTML sohbet iletisi.

- `ChatMessageType.TOPIC_UPDATED`: Konunun güncelleştirildiğini belirten sistem iletisi.

- `ChatMessageType.PARTICIPANT_ADDED`: Sohbet iş parçacığına bir veya daha fazla katılımcı eklendiğini belirten sistem iletisi.

- `ChatMessageType.PARTICIPANT_REMOVED`: Bir katılımcının sohbet iş parçacığından kaldırıldığını belirten sistem iletisi.

Daha ayrıntılı bilgi için bkz. [Ileti türleri](../../../concepts/chat/concepts.md#message-types).

## <a name="update-topic-of-a-chat-thread"></a>Sohbet iş parçacığının konusunu güncelleştirme
Yöntemini kullanarak bir sohbet iş parçacığının konusunu güncelleştirebilirsiniz `update_topic`

```python
topic = "updated thread topic"
chat_thread_client.update_topic(topic=topic)

chat_thread = chat_client.get_chat_thread(chat_thread_client.thread_id)
updated_topic = chat_thread.topic

print('Updated topic: ', updated_topic)
```

## <a name="update-a-message"></a>İleti güncelleştirme
Mevcut bir iletinin içeriğini `update_message` message_id tarafından tanımlanan yöntemi kullanarak güncelleştirebilirsiniz

- Kullanım `message_id` , gerekli, iletinin BENZERSIZ kimliğidir.
- `content`, İsteğe bağlı, güncelleştirilenecek ileti içeriği kullanın; belirtilmemişse, boş olarak atanır

```python
content = 'Hello world!'
send_message_result_id = chat_thread_client.send_message(content=content, sender_display_name=sender_display_name)

content = 'Hello! I am updated content'
chat_thread_client.update_message(message_id=send_message_result_id, content=content)

chat_message = chat_thread_client.get_message(send_message_result_id)
print('Updated message content: ', chat_message.content.message)
```

## <a name="send-read-receipt-for-a-message"></a>İleti için okundu bilgisi gönder
`send_read_receipt`Yöntemi, bir kullanıcı adına bir iş parçacığına okundu bilgisi olayı vermek için kullanılabilir.

- `message_id`Geçerli Kullanıcı tarafından okunan en son ILETI kimliğini belirtmek için kullanın.

```python
message_id=send_message_result_id
chat_thread_client.send_read_receipt(message_id=message_id)
```

## <a name="list-read-receipts-for-a-chat-thread"></a>Sohbet iş parçacığı için okundu bilgilerini listeleme
`list_read_receipts`Yöntemi, bir iş parçacığı için okundu bilgisi almak üzere kullanılabilir.

- `results_per_page`Sayfa başına döndürülecek en fazla sohbet iletisi okuma alındısı sayısını belirtmek için kullanın.
- `skip`Yanıt olarak belirtilen konuma kadar sohbet iletisini atla okundu bilgilerini belirtmek için kullanın.

Bir yineleyicisi, `[ChatMessageReadReceipt]` okundu alındılarını listelerken döndürülen yanıttır

```python
read_receipts = chat_thread_client.list_read_receipts(results_per_page=5, skip=0)

for read_receipt_page in read_receipts.by_page():
    for read_receipt in read_receipt_page:
        print('ChatMessageReadReceipt: ', read_receipt)
        print('Sender', read_receipt.sender)
        print('Message Id', read_receipt.chat_message_id)
        print('Read On Timestamp', read_receipt.read_on)
```

## <a name="send-typing-notification"></a>Yazma bildirimi gönder
`send_typing_notification`Yöntemi, bir kullanıcı adına bir yazma olayını bir iş parçacığına göndermek için kullanılabilir.

```python
chat_thread_client.send_typing_notification()
```

## <a name="delete-message"></a>İletiyi Sil
`delete_message`Yöntemi, message_id tarafından tanımlanan bir iletiyi silmek için kullanılabilir

- `message_id`Message_id belirtmek için kullanın

```python
message_id=send_message_result_id
chat_thread_client.delete_message(message_id=message_id)
```

## <a name="add-a-user-as-participant-to-the-chat-thread"></a>Sohbet iş parçacığına katılımcı olarak Kullanıcı ekleme

Sohbet iş parçacığı oluşturulduktan sonra, bundan sonra kullanıcı ekleyebilir ve kaldırabilirsiniz. Kullanıcıları ekleyerek, sohbet iş parçacığına ileti gönderebilmeleri ve başka katılımcılar ekleyip kaldırabilmesi için onlara erişim izni verirsiniz. Yöntemi çağırmadan önce `add_participant` , bu kullanıcı için yeni bir erişim belirteci ve kimliği aldığınızdan emin olun. Kullanıcının sohbet istemcisini başlatması için bu erişim belirtecine ihtiyacı olacak.

`add_participant`Thread_id tarafından tanımlanan iş parçacığına iş parçacığı katılımcıları eklemek için yöntemini kullanın.

- `thread_participant`Sohbet iş parçacığına eklenecek katılımcıyı belirtmek için kullanın;
- `user`, gereken, `CommunicationUserIdentifier` `CommunicationIdentityClient` [Kullanıcı oluşturma](../../access-tokens.md#create-an-identity) sırasında tarafından oluşturduğunuz
- `display_name`, isteğe bağlı, iş parçacığı katılımcısı için görünen addır.
- `share_history_time`, isteğe bağlı, sohbet geçmişinin katılımcının paylaştığı süredir. Sohbet iş parçacığının başlatılmasından bu yana geçmişi paylaşmak için, bu özelliği, iş parçacığı oluşturma zamanından daha küçük veya ona eşit bir tarih olarak ayarlayın. Katılımcının eklendiği tarihten önce geçmiş paylaşmak için, geçerli tarih olarak ayarlayın. Kısmi geçmişi paylaşmak için, bunu bir ara tarih olarak ayarlayın.

Katılımcı başarıyla eklendiğinde, hata oluşturulmaz. Katılımcı eklenirken bir hatayla karşılaşıldı, a `RuntimeError` atılır

```python
from azure.communication.identity import CommunicationIdentityClient
from azure.communication.chat import ChatThreadParticipant
from datetime import datetime

# create an user
identity_client = CommunicationIdentityClient.from_connection_string('<connection_string>')
new_user = identity_client.create_user()

# # conversely, you can also add an existing user to a chat thread; provided the user_id is known
# from azure.communication.identity import CommunicationUserIdentifier
#
# user_id = 'some user id'
# user_display_name = "Wilma Flinstone"
# new_user = CommunicationUserIdentifier(user_id)
# participant = ChatThreadParticipant(
#     user=new_user,
#     display_name=user_display_name,
#     share_history_time=datetime.utcnow())

def decide_to_retry(error, **kwargs):
    """
    Insert some custom logic to decide if retry is applicable based on error
    """
    return True

participant = ChatThreadParticipant(
    user=new_user,
    display_name='Fred Flinstone',
    share_history_time=datetime.utcnow())

try:
    chat_thread_client.add_participant(thread_participant=participant)
except RuntimeError as e:
    if e is not None and decide_to_retry(error=e):
        chat_thread_client.add_participant(thread_participant=participant)
```

Aynı zamanda, `add_participants` Yeni erişim belirteci ve tanımlanın tüm kullanıcılar için kullanılabilir olduğu belirtilen yöntemi kullanarak sohbet iş parçacığına birden fazla Kullanıcı eklenebilir.

Bir `list(tuple(ChatThreadParticipant, CommunicationError))` döndürülür. Katılımcı başarıyla eklendiğinde boş bir liste beklenmektedir. Katılımcı eklenirken bir hata olması durumunda, listede başarısız katılımcılar ve karşılaşılan hata ile birlikte doldurulur.

```python
from azure.communication.identity import CommunicationIdentityClient
from azure.communication.chat import ChatThreadParticipant
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
# participant = ChatThreadParticipant(
#     user=new_user,
#     display_name=user_display_name,
#     share_history_time=datetime.utcnow())

participants = []
for _user in new_users:
  chat_thread_participant = ChatThreadParticipant(
    user=_user,
    display_name='Fred Flinstone',
    share_history_time=datetime.utcnow()
  ) 
  participants.append(chat_thread_participant) 

response = chat_thread_client.add_participants(thread_participants=participants)

def decide_to_retry(error, **kwargs):
    """
    Insert some custom logic to decide if retry is applicable based on error
    """
    return True

# verify if all users has been successfully added or not
# in case of partial failures, you can retry to add all the failed participants 
retry = [p for p, e in response if decide_to_retry(e)]
if len(retry) > 0:
    chat_thread_client.add_participants(retry)
```


## <a name="remove-user-from-a-chat-thread"></a>Kullanıcı sohbet iş parçacığından kaldır

Katılımcı eklemeye benzer şekilde, katılımcıları bir iş parçacığından da kaldırabilirsiniz. ' I kaldırmak için, eklediğiniz katılımcıların kimliklerini izlemeniz gerekir.

`remove_participant`ThreadID tarafından tanımlanan iş parçacığından iş parçacığı katılımcısını kaldırmak için yöntemini kullanın.
- `user` , `CommunicationUserIdentifier` iş parçacığından kaldırılacak.

```python
chat_thread_client.remove_participant(user=new_user)

# # converesely you can also do the following; provided the user_id is known
# from azure.communication.identity import CommunicationUserIdentifier
# 
# user_id = 'some user id'
# chat_thread_client.remove_participant(user=CommunincationUserIdentfier(new_user))
```

## <a name="run-the-code"></a>Kodu çalıştırma

Uygulamayı komut ile uygulama dizininizden çalıştırın `python` .

```console
python start-chat.py
```
