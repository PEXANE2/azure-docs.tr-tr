---
title: include dosyası
description: include dosyası
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 9/1/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: a76c6467dac69fd3d21aa659c52227046c166938
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94816669"
---
## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce şunları yaptığınızdan emin olun:
- Etkin abonelikle bir Azure hesabı oluşturun. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- [Visual Studio 'yu](https://visualstudio.microsoft.com/downloads/) yükler 
- Bir Azure Iletişim Hizmetleri kaynağı oluşturun. Ayrıntılar için bkz. [Azure Iletişim kaynağı oluşturma](../../create-communication-resource.md). Bu hızlı başlangıç için kaynak **uç** noktanızı kaydetmeniz gerekir.
- Bir [Kullanıcı erişim belirteci](../../access-tokens.md). Kapsamı "sohbet" olarak ayarladığınızdan emin olun ve belirteç dizesinin yanı sıra Kullanıcı kimliği dizesini de unutmayın.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-c-application"></a>Yeni bir C# uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adıyla yeni bir konsol uygulaması oluşturmak için komutunu kullanın `ChatQuickstart` . Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: **program.cs**.

```console
dotnet new console -o ChatQuickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin ve `dotnet build` uygulamanızı derlemek için komutunu kullanın.

```console
cd ChatQuickstart
dotnet build
```

### <a name="install-the-package"></a>Paketi yükler

.NET için Azure Iletişim sohbeti istemci kitaplığı 'nı yükler

```PowerShell
dotnet add package Azure.Communication.Chat --version 1.0.0-beta.3
``` 

## <a name="object-model"></a>Nesne modeli

Aşağıdaki sınıflar, C# için Azure Iletişim Hizmetleri sohbeti istemci kitaplığı 'nın bazı önemli özelliklerini işler.

| Ad                                  | Açıklama                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Bu sınıf, sohbet işlevselliği için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve iş parçacıklarını oluşturmak, almak ve silmek için kullanın. |
| ChatThreadClient | Bu sınıf, sohbet Iş parçacığı işlevselliği için gereklidir. ChatClient aracılığıyla bir örnek edinirsiniz ve bu örneği kullanarak ileti gönderebilir/alabilir/güncelleştirebilir/silebilirsiniz, kullanıcıları ekleyin/kaldırın/alın, yazma bildirimleri gönderin ve okundu bilgilerini okuyun. |

## <a name="create-a-chat-client"></a>Sohbet istemcisi oluşturma

Bir sohbet istemcisi oluşturmak için, Iletişim Hizmetleri uç noktanızı ve önkoşul adımlarının bir parçası olarak oluşturulan erişim belirtecini kullanacaksınız. `CommunicationIdentityClient` `Administration` Kullanıcı oluşturmak ve sohbet istemcinize geçirilecek bir belirteç vermek için istemci kitaplığındaki sınıfını kullanmanız gerekir. [Kullanıcı erişim belirteçleri](../../access-tokens.md)hakkında daha fazla bilgi edinin.

```csharp
using Azure.Communication.Identity;
using Azure.Communication.Chat;

// Your unique Azure Communication service endpoint
Uri endpoint = new Uri("https://<RESOURCE_NAME>.communication.azure.com");

CommunicationUserCredential communicationUserCredential = new CommunicationUserCredential(<Access_Token>);
ChatClient chatClient = new ChatClient(endpoint, communicationUserCredential);
```

## <a name="start-a-chat-thread"></a>Sohbet iş parçacığı başlatma

`createChatThread`Bir sohbet iş parçacığı oluşturmak için yöntemini kullanın.
- `topic`Bu sohbete bir konu vermek için kullanın; Konu, işlevi kullanılarak sohbet iş parçacığı oluşturulduktan sonra güncelleştirilemeyebilir `UpdateThread` .
- `members` `ChatThreadMember` Sohbet iş parçacığına eklenecek nesne listesini geçirmek için özelliğini kullanın. `ChatThreadMember`Nesne bir nesne ile başlatılır `CommunicationUser` . Bir nesne almak için `CommunicationUser` , [bir kullanıcı oluşturmak](../../access-tokens.md#create-an-identity) için aşağıdaki yönergeyi uygulayarak oluşturduğunuz erişim kimliğini geçirmeniz gerekir

Yanıt, `chatThreadClient` oluşturulan sohbet iş parçacığı üzerinde işlem gerçekleştirmek için kullanılır: sohbet iş parçacığına üye ekleme, ileti gönderme, ileti silme, vb. `Id` Sohbet iş parçacığının BENZERSIZ kimliği olan özniteliği içerir. 

```csharp
var chatThreadMember = new ChatThreadMember(new CommunicationUser("<Access_ID>"))
{
    DisplayName = "UserDisplayName"
};
ChatThreadClient chatThreadClient = await chatClient.CreateChatThreadAsync(topic: "Chat Thread topic C# sdk", members: new[] { chatThreadMember });
string threadId = chatThreadClient.Id;
```

## <a name="get-a-chat-thread-client"></a>Sohbet iş parçacığı istemcisi al
`GetChatThreadClient`Yöntemi, zaten var olan bir iş parçacığı için bir iş parçacığı istemcisi döndürür. Oluşturulan iş parçacığında işlem gerçekleştirmek için kullanılabilir: üye ekleme, ileti gönderme vb. ThreadId, mevcut sohbet iş parçacığının benzersiz KIMLIĞIDIR.

```csharp
string threadId = "<THREAD_ID>";
ChatThreadClient chatThreadClient = chatClient.GetChatThreadClient(threadId);
```

## <a name="send-a-message-to-a-chat-thread"></a>Sohbet iş parçacığına ileti gönderin

`SendMessage`ThreadID tarafından tanımlanan bir iş parçacığına ileti göndermek için yöntemini kullanın.

- `content`Sohbet iletisi içeriğini sağlamak için kullanın, bu gereklidir.
- Belirtilmemişse, ' normal ' `priority` veya ' yüksek ' gibi ileti öncelik düzeyini belirtmek için kullanın, ' normal ' kullanılır.
- `senderDisplayName`Gönderenin görünen adını belirtmek için kullanın, belirtilmezse boş ad kullanılır.

`SendChatMessageResult` yanıt ileti göndermekten döndürülen yanıt, iletinin benzersiz KIMLIĞI olan bir ID içeriyor.

```csharp
var content = "hello world";
var priority = ChatMessagePriority.Normal;
var senderDisplayName = "sender name";

SendChatMessageResult sendChatMessageResult = await chatThreadClient.SendMessageAsync(content, priority, senderDisplayName);
string messageId = sendChatMessageResult.Id;
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Sohbet iş parçacığından sohbet iletileri alma

Sohbet `GetMessages` iş parçacığı istemcisinde belirtilen aralıklarda yöntemi yoklayarak sohbet iletileri alabilirsiniz.

```csharp
AsyncPageable<ChatMessage> allMessages = chatThreadClient.GetMessagesAsync();
await foreach (ChatMessage message in allMessages)
{
    Console.WriteLine($"{message.Id}:{message.Sender.Id}:{message.Content}");
}
```

`GetMessages` isteğe bağlı bir `DateTimeOffset` parametre alır. Bu fark belirtilmişse, sonrasında alınan, güncellenen veya silinen iletileri alırsınız. Bu iletilerin, zaman içinde geçen ancak döndürülmeden sonra düzenlendiğini veya kaldırılmadan önce alındığını unutmayın.

`GetMessages` ve kullanarak iletide gerçekleşen tüm düzenleme veya silme işlemleri dahil olmak üzere iletinin en son sürümünü döndürür `UpdateMessage` `DeleteMessage` . Silinen iletiler için, `chatMessage.DeletedOn` iletinin silindiğini gösteren bir tarih saat değeri döndürür. Düzenlenen iletiler için, `chatMessage.EditedOn` iletinin ne zaman düzenlendiğini gösteren bir tarih saat döndürür. İleti oluşturma özgün zamanına kullanılarak erişilebilir `chatMessage.CreatedOn` ve iletileri sıralamak için kullanılabilir.

`GetMessages` tarafından tanımlanabilecek farklı ileti türlerini döndürür `chatMessage.Type` . Bu türler şunlardır:

- `Text`: Bir iş parçacığı üyesi tarafından gönderilen normal sohbet iletisi.

- `ThreadActivity/TopicUpdate`: Konunun güncelleştirildiğini belirten sistem iletisi.

- `ThreadActivity/AddMember`: Sohbet iş parçacığına bir veya daha fazla üye eklendiğini belirten sistem iletisi.

- `ThreadActivity/DeleteMember`: Bir üyenin sohbet iş parçacığından kaldırıldığını belirten sistem iletisi.

Daha ayrıntılı bilgi için bkz. [Ileti türleri](../../../concepts/chat/concepts.md#message-types).

## <a name="update-a-message"></a>İleti güncelleştirme

Üzerinde çağrılarak daha önce gönderilen bir iletiyi güncelleştirebilirsiniz `UpdateMessage` `ChatThreadClient` .

```csharp
string id = "id-of-message-to-edit";
string content = "updated content";
await chatThreadClient.UpdateMessageAsync(id, content);
```

## <a name="deleting-a-message"></a>İleti silme

' İ çağırarak bir iletiyi silebilirsiniz `DeleteMessage` `ChatThreadClient` .

```csharp
string id = "id-of-message-to-delete";
await chatThreadClient.DeleteMessageAsync(id);
```

## <a name="add-a-user-as-member-to-the-chat-thread"></a>Sohbet iş parçacığına üye olarak Kullanıcı ekleme

Bir iş parçacığı oluşturulduktan sonra, bundan sonra kullanıcı ekleyebilir ve kaldırabilirsiniz. Kullanıcıları ekleyerek, onlara iş parçacığına ileti gönderebilecek ve diğer üyeleri ekleyip kaldırabilecekler için erişim izni verirsiniz. Çağrılmadan önce `AddMembers` , bu kullanıcı için yeni bir erişim belirteci ve kimliği aldığınızdan emin olun. Kullanıcının sohbet istemcisini başlatması için bu erişim belirtecine ihtiyacı olacak.

`AddMembers`ThreadID tarafından tanımlanan iş parçacığına iş parçacığı üyeleri eklemek için yöntemini kullanın.

 - `members`Sohbet iş parçacığına eklenecek üyeleri listelemek için kullanın;
 - `User`, gerekli, bu yeni kullanıcı için alacağınız kimliktir.
 - `DisplayName`, isteğe bağlı, iş parçacığı üyesi için görünen addır.
 - `ShareHistoryTime`, isteğe bağlı, sohbet geçmişinin üyeyle paylaştığı zaman. Sohbet iş parçacığının başlangıcından bu yana geçmişi paylaşmak için, DateTime. MinValue olarak ayarlayın. Bir geçmişi paylaşmak için, üyenin eklendiği zamana kadar, geçerli saate ayarlayın. Kısmi geçmişi paylaşmak için, iş parçacığı oluşturma ve geçerli saat arasındaki zaman noktasına ayarlayın.

```csharp
ChatThreadMember member = new ChatThreadMember(communicationUser);
member.DisplayName = "display name member 1";
member.ShareHistoryTime = DateTime.MinValue; // share all history
await chatThreadClient.AddMembersAsync(members: new[] {member});
```
## <a name="remove-user-from-a-chat-thread"></a>Kullanıcı sohbet iş parçacığından kaldır

Bir iş parçacığına kullanıcı eklemeye benzer şekilde, bir sohbet iş parçacığından kullanıcıları kaldırabilirsiniz. Bunu yapmak için, eklediğiniz üyelerin kimliğini (CommunicationUser) izlemeniz gerekir.

```csharp
await chatThreadClient.RemoveMemberAsync(communicationUser);
```

## <a name="run-the-code"></a>Kodu çalıştırma

Uygulamayı komut ile uygulama dizininizden çalıştırın `dotnet run` .

```console
dotnet run
```
