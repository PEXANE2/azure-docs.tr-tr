---
title: dosya dahil etme
description: dosya dahil etme
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 9/1/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 3c05596e16edc5243b8a97002a5cc5990c69ec43
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "90948079"
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

```console
pip install azure-communication-administration
```

```python
from azure.communication.chat import ChatClient, CommunicationUserCredential

endpoint = "https://<RESOURCE_NAME>.communication.azure.com"
chat_client = ChatClient(endpoint, CommunicationUserCredential(<Access Token>))
```

## <a name="start-a-chat-thread"></a>Sohbet iş parçacığı başlatma

`create_chat_thread`Bir sohbet iş parçacığı oluşturmak için yöntemini kullanın.

- `topic`İş parçacığı konusu vermek için kullanın; Konu, işlevi kullanılarak sohbet iş parçacığı oluşturulduktan sonra güncelleştirilemeyebilir `update_thread` .
- `members` `ChatThreadMember` Sohbet iş parçacığına eklenecek olan öğesini listelemek için kullanın, `ChatThreadMember` `CommunicationUser` `user` [bir kullanıcı oluşturarak](../../access-tokens.md#create-a-user) oluşturduğunuz tarihten sonra aldığınız gibi, türü olarak alır

Yanıt, `chat_thread_client` sohbet iş parçacığına üye ekleme, ileti gönderme, ileti silme vb. gibi yeni oluşturulan sohbet iş parçacığı üzerinde işlem gerçekleştirmek için kullanılır. `thread_id` Sohbet iş parçacığının BENZERSIZ kimliği olan bir özelliği içerir.

```python
from datetime import datetime
from azure.communication.chat import ChatThreadMember

topic="test topic"
thread_members=[ChatThreadMember(
    user=user,
    display_name='name',
    share_history_time=datetime.utcnow()
)]
chat_thread_client = chat_client.create_chat_thread(topic, thread_members)
```

## <a name="get-a-chat-thread-client"></a>Sohbet iş parçacığı istemcisi al
Get_chat_thread_client yöntemi, zaten var olan bir iş parçacığı için bir iş parçacığı istemcisi döndürür. Oluşturulan iş parçacığında işlem gerçekleştirmek için kullanılabilir: üye ekleme, ileti gönderme vb. thread_id, mevcut sohbet iş parçacığının benzersiz KIMLIĞIDIR.

```python
thread_id = 'id'
chat_thread_client = chat_client.get_chat_thread_client(thread_id)
```

## <a name="send-a-message-to-a-chat-thread"></a>Sohbet iş parçacığına ileti gönderin

`send_message`Az önce oluşturduğunuz ve ThreadID tarafından tanımlanan bir sohbet iş parçacığına ileti göndermek için yöntemini kullanın.

- `content`Sohbet iletisi içeriğini sağlamak için kullanın;
- `priority`' Normal ' veya ' yüksek ' gibi ileti öncelik düzeyini belirtmek için kullanın; Bu özellik, uygulamanızdaki alıcı kullanıcısına ileti dikkatini çekmek veya özel iş mantığını yürütmek için Kullanıcı arabirimi göstergesinin olması için kullanılabilir.
- `senderDisplayName`Gönderenin görünen adını belirtmek için kullanın;

Yanıt, `SendChatMessageResult` Bu iletinin BENZERSIZ kimliği olan bir "kimlik" içeriyor.

```python
from azure.communication.chat import ChatMessagePriority

content='hello world'
priority=ChatMessagePriority.NORMAL
sender_display_name='sender name'

send_message_result = chat_thread_client.send_message(content, priority=priority, sender_display_name=sender_display_name)
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Sohbet iş parçacığından sohbet iletileri alma

Belirtilen aralıklarda yöntemi yoklayarak sohbet iletileri alabilirsiniz `list_messages` .

```python
chat_messages = chat_thread_client.list_messages()
```
`list_messages` ve kullanarak iletide gerçekleşen tüm düzenleme veya silme işlemleri dahil olmak üzere iletinin en son sürümünü döndürür `update_message` `delete_message` . Silinen iletiler için `ChatMessage.deleted_on` , iletinin silindiğini gösteren bir tarih saat değeri döndürür. Düzenlenen iletiler için, `ChatMessage.edited_on` iletinin ne zaman düzenlendiğini gösteren bir tarih saat döndürür. İleti oluşturmaya yönelik özgün saate, `ChatMessage.created_on` iletileri sıralamak için kullanılabilecek kullanılarak erişilebilir.

`list_messages` tarafından tanımlanabilecek farklı ileti türlerini döndürür `ChatMessage.type` . Bu türler şunlardır:

- `Text`: Bir iş parçacığı üyesi tarafından gönderilen normal sohbet iletisi.

- `ThreadActivity/TopicUpdate`: Konunun güncelleştirildiğini belirten sistem iletisi.

- `ThreadActivity/AddMember`: Sohbet iş parçacığına bir veya daha fazla üye eklendiğini belirten sistem iletisi.

- `ThreadActivity/DeleteMember`: Bir üyenin sohbet iş parçacığından kaldırıldığını belirten sistem iletisi.

Daha ayrıntılı bilgi için bkz. [Ileti türleri](../../../concepts/chat/concepts.md#message-types).

## <a name="add-a-user-as-member-to-the-chat-thread"></a>Sohbet iş parçacığına üye olarak Kullanıcı ekleme

Sohbet iş parçacığı oluşturulduktan sonra, bundan sonra kullanıcı ekleyebilir ve kaldırabilirsiniz. Kullanıcıları ekleyerek, sohbet iş parçacığına ileti gönderebilmeleri ve başka Üyeler ekleyip kaldırabilmesi için onlara erişim izni verirsiniz. Yöntemi çağırmadan önce `add_members` , bu kullanıcı için yeni bir erişim belirteci ve kimliği aldığınızdan emin olun. Kullanıcının sohbet istemcisini başlatması için bu erişim belirtecine ihtiyacı olacak.

`add_members`ThreadID tarafından tanımlanan iş parçacığına iş parçacığı üyeleri eklemek için yöntemini kullanın.

- `members`Sohbet iş parçacığına eklenecek üyeleri listelemek için kullanın;
- `user`, gereken, `CommunicationUser` `CommunicationIdentityClient` [Kullanıcı oluşturma](../../access-tokens.md#create-a-user) sırasında tarafından oluşturduğunuz
- `display_name`, isteğe bağlı, iş parçacığı üyesi için görünen addır.
- `share_history_time`, isteğe bağlı, sohbet geçmişinin üye ile paylaşıldıkları süredir. Sohbet iş parçacığının başlatılmasından bu yana geçmişi paylaşmak için, bu özelliği, iş parçacığı oluşturma zamanından daha küçük veya ona eşit bir tarih olarak ayarlayın. Üyenin eklendiği tarihten önce hiçbir geçmiş paylaşmak için, geçerli tarih olarak ayarlayın. Kısmi geçmişi paylaşmak için, bunu bir ara tarih olarak ayarlayın.

```python
new_user = identity_client.create_user()

from azure.communication.chat import ChatThreadMember
from datetime import datetime
member = ChatThreadMember(
    user=new_user,
    display_name='name',
    share_history_time=datetime.utcnow())
thread_members = [member]
chat_thread_client.add_members(thread_members)
```

## <a name="remove-user-from-a-chat-thread"></a>Kullanıcı sohbet iş parçacığından kaldır

Üye eklemeye benzer şekilde, bir iş parçacığından üye de kaldırabilirsiniz. ' I kaldırmak için, eklediğiniz üyelerin kimliklerini izlemeniz gerekir.

`remove_member`ThreadID tarafından tanımlanan iş parçacığından iş parçacığı üyesini kaldırmak için yöntemini kullanın.
- `user` , iş parçacığından kaldırılacak olan CommunicationUser.

```python
chat_thread_client.remove_member(user)
```

## <a name="run-the-code"></a>Kodu çalıştırma

Uygulamayı komut ile uygulama dizininizden çalıştırın `python` .

```console
python start-chat.py
```
