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
ms.openlocfilehash: 5bf4bbe2c8dc863f67dffb50609f7775a4499e3a
ms.sourcegitcommit: d40ffda6ef9463bb75835754cabe84e3da24aab5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2021
ms.locfileid: "107073727"
---
[!INCLUDE [Public Preview Notice](../../../includes/public-preview-include-chat.md)]

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce şunları yaptığınızdan emin olun:

- Etkin abonelikle bir Azure hesabı oluşturun. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- [Xcode](https://developer.apple.com/xcode/) ve [Cocoapods](https://cocoapods.org/)'i yükler. Xcode 'u kullanarak hızlı başlangıç için bir iOS uygulaması oluşturursunuz ve bağımlılıklarını yüklemek için CocoaPods.
- Bir Azure Iletişim Hizmetleri kaynağı oluşturun. Ayrıntılar için bkz. [hızlı başlangıç: Iletişim Hizmetleri kaynaklarını oluşturma ve yönetme](../../create-communication-resource.md). Bu hızlı başlangıçta, kaynak uç noktanızı kaydetmeniz gerekir.
- Azure Iletişim hizmetlerinde iki kullanıcı oluşturun ve bunları bir [Kullanıcı erişim belirteci](../../access-tokens.md)olarak verin. Kapsamını olarak ayarladığınızdan emin olun ve dizeyi `chat` ve dizeyi de unutmayın `token` `userId` . Bu hızlı başlangıçta, ilk katılımcı içeren bir iş parçacığı oluşturun ve sonra iş parçacığına ikinci bir katılımcı ekleyin.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-ios-application"></a>Yeni bir iOS uygulaması oluşturma

Xcode ' u açın ve **Yeni bir Xcode projesi oluştur**' u seçin. Ardından, şablon için platform ve **uygulama** olarak **iOS** ' u seçin.

Proje adı için **ChatQuickstart** girin. Ardından arabirim olarak **film şeridi** , yaşam döngüsü olarak **Uııt uygulama temsilcisi** ve dil olarak **Swift** ' ı seçin.

**İleri**' yi seçin ve projenin oluşturulmasını istediğiniz dizini seçin.

### <a name="install-the-libraries"></a>Kitaplıkları yükler

Gerekli Iletişim Hizmetleri bağımlılıklarını yüklemek için CocoaPods kullanın.

Komut satırından iOS projesinin kök dizininin içine gidin `ChatQuickstart` . Şu komutla bir pod dosyası oluşturun: `pod init` .

Pod dosyasını açın ve şu bağımlılıkları `ChatQuickstart` hedefe ekleyin:

```
pod 'AzureCommunication', '~> 1.0.0-beta.9'
pod 'AzureCommunicationChat', '~> 1.0.0-beta.9'
```

Aşağıdaki komutla bağımlılıkları yükler: `pod install` . Bunun ayrıca bir Xcode çalışma alanı oluşturduğunu unutmayın.

Çalıştırdıktan sonra `pod install` , yeni oluşturulan ' yi seçerek projeyi Xcode 'da yeniden açın `.xcworkspace` .

### <a name="set-up-the-placeholders"></a>Yer tutucuları ayarlama

Çalışma alanını `ChatQuickstart.xcworkspace` Xcode 'da açın ve sonra açın `ViewController.swift` .

Bu hızlı başlangıçta kodunuzu öğesine ekler `viewController` ve çıktıyı Xcode konsolunda görüntüleyebilirsiniz. Bu hızlı başlangıç, iOS 'ta bir kullanıcı arabirimi oluşturmayı gidermez. 

En üstünde, `viewController.swift` ve kitaplıklarını içeri aktarın `AzureCommunication` `AzureCommunicatonChat` :

```
import AzureCommunication
import AzureCommunicationChat
```

Aşağıdaki kodu `viewDidLoad()` yöntemine kopyalayın `ViewController` :

```
override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        let semaphore = DispatchSemaphore(value: 0)
        DispatchQueue.global(qos: .background).async {
            do {
                // <CREATE A CHAT CLIENT>
                
                // <CREATE A CHAT THREAD>
                
                // <CREATE A CHAT THREAD CLIENT>
                
                // <SEND A MESSAGE>
                
                // <ADD A USER>
                
                // <LIST USERS>
                
                // <REMOVE A USER>
            } catch {
                print("Quickstart failed: \(error.localizedDescription)")
            }
        }
    }
```

Tanıtım amacıyla, kodunuzun eşitlenmesi için bir semafor kullanacağız. Aşağıdaki adımlarda, Azure Iletişim Hizmetleri sohbet kitaplığı 'nı kullanarak yer tutucuları örnek kodla değiştirirsiniz.


### <a name="create-a-chat-client"></a>Sohbet istemcisi oluşturma

`<CREATE A CHAT CLIENT>` açıklamasını aşağıdaki kodla değiştirin:

```
let endpoint = "<ACS_RESOURCE_ENDPOINT>"
    let credential =
    try CommunicationTokenCredential(
        token: "<ACCESS_TOKEN>"
    )
    let options = AzureCommunicationChatClientOptions()

    let chatClient = try ChatClient(
        endpoint: endpoint,
        credential: credential,
        withOptions: options
    )
```

`<ACS_RESOURCE_ENDPOINT>`Azure Communication Services kaynağınızın uç noktasıyla değiştirin. `<ACCESS_TOKEN>`Geçerli bir Iletişim Hizmetleri erişim belirteciyle değiştirin.

Bu hızlı başlangıç, sohbet uygulamanız için belirteçleri yönetmek üzere bir hizmet katmanı oluşturmayı kapsamaz, ancak bu önerilir. Daha fazla bilgi için [sohbet kavramlarının](../../../concepts/chat/concepts.md)"sohbet mimarisi" bölümüne bakın.

Kullanıcı erişim belirteçleri hakkında daha fazla bilgi için bkz. [hızlı başlangıç: erişim belirteçleri oluşturma ve yönetme](../../access-tokens.md).

## <a name="object-model"></a>Nesne modeli 

Aşağıdaki sınıflar ve arabirimler, JavaScript için Azure Communication Services sohbet SDK 'sının önemli özelliklerinden bazılarını idare edebilir.

| Ad                                   | Açıklama                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ChatClient` | Bu sınıf, sohbet işlevselliği için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve iş parçacıklarını oluşturmak, almak ve silmek için kullanın. |
| `ChatThreadClient` | Bu sınıf, sohbet iş parçacığı işlevselliği için gereklidir. Aracılığıyla bir örnek elde edersiniz `ChatClient` ve iletileri göndermek, almak, güncelleştirmek ve silmek için kullanın. Ayrıca, kullanıcıları eklemek, kaldırmak ve almak, yazma bildirimleri göndermek ve alındıları okumak ve sohbet olaylarına abone olmak için de kullanabilirsiniz. |

## <a name="start-a-chat-thread"></a>Sohbet iş parçacığı başlatma

Şimdi, `ChatClient` ilk Kullanıcı ile yeni bir iş parçacığı oluşturmak için kullanabilirsiniz.

`<CREATE A CHAT THREAD>` açıklamasını aşağıdaki kodla değiştirin:

```
let request = CreateThreadRequest(
    topic: "Quickstart",
    participants: [
        Participant(
            id: CommunicationUserIdentifier("<USER_ID>"),
            displayName: "Jack"
        )
    ]
)

var threadId: String?
chatClient.create(thread: request) { result, _ in
    switch result {
    case let .success(result):
        threadId = result.thread?.id

    case .failure:
        fatalError("Failed to create thread.")
    }
    semaphore.signal()
}
semaphore.wait()
```

`<USER_ID>`Geçerli bir Iletişim Hizmetleri Kullanıcı kimliğiyle değiştirin.

Devam etmeden önce tamamlama işleyicisini beklemek için burada bir semafor kullanıyorsunuz. Sonraki adımlarda, öğesini `threadId` tamamlama işleyicisine döndürülen yanıttan kullanacaksınız.

## <a name="get-a-chat-thread-client"></a>Sohbet iş parçacığı istemcisi al

Artık bir sohbet iş parçacığı oluşturduğunuza göre, `ChatThreadClient` iş parçacığı içinde işlem gerçekleştirmek için bir elde edebilirsiniz.

`<CREATE A CHAT THREAD CLIENT>` açıklamasını aşağıdaki kodla değiştirin:

```
let chatThreadClient = try chatClient.createClient(forThread: threadId!)
```

## <a name="send-a-message-to-a-chat-thread"></a>Sohbet iş parçacığına ileti gönderin

`<SEND A MESSAGE>` açıklamasını aşağıdaki kodla değiştirin:

```
let message = SendChatMessageRequest(
    content: "Hello!",
    senderDisplayName: "Jack"
)

chatThreadClient.send(message: message) { result, _ in
    switch result {
    case let .success(result):
        print("Message sent, message id: \(result.id)")
    case .failure:
        print("Failed to send message")
    }
    semaphore.signal()
}
semaphore.wait()
```

İlk olarak, `SendChatMessageRequest` içeriği ve gönderenin görünen adını içeren öğesini oluşturursunuz. Bu istek, dahil etmek isterseniz, paylaşma geçmişi süresini de içerebilir. Tamamlanma işleyicisine döndürülen yanıt, gönderilen iletinin KIMLIĞINI içerir.

## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Sohbet iş parçacığına katılımcı olarak Kullanıcı ekleme

`<ADD A USER>` açıklamasını aşağıdaki kodla değiştirin:

```
let user = Participant(
    id: CommunicationUserIdentifier("<USER_ID>"),
    displayName: "Jane"
)

chatThreadClient.add(participants: [user]) { result, _ in
    switch result {
    case let .success(result):
        (result.errors != nil) ? print("Added participant") : print("Error adding participant")
    case .failure:
        print("Failed to list participants")
    }
    semaphore.signal()
}
semaphore.wait()
```

`<USER_ID>`Eklenecek kullanıcının Iletişim Hizmetleri Kullanıcı kimliğiyle değiştirin.

Bir iş parçacığına katılımcı eklerken döndürülen yanıt hata içerebilir. Bu hatalar belirli katılımcıları ekleme başarısızlığını temsil eder.

## <a name="list-users-in-a-thread"></a>İş parçacığında kullanıcıları listeleme

`<LIST USERS>` açıklamasını aşağıdaki kodla değiştirin:

```
chatThreadClient.listParticipants { result, _ in
    switch result {
    case let .success(participants):
        var iterator = participants.syncIterator
        while let participant = iterator.next() {
            let user = participant.id as! CommunicationUserIdentifier
            print(user.identifier)
        }
    case .failure:
        print("Failed to list participants")
    }
    semaphore.signal()
}
semaphore.wait()
```


## <a name="remove-user-from-a-chat-thread"></a>Kullanıcı sohbet iş parçacığından kaldır

`<REMOVE A USER>` açıklamasını aşağıdaki kodla değiştirin:

```
chatThreadClient
    .remove(
        participant: CommunicationUserIdentifier("<USER_ID>")
    ) { result, _ in
        switch result {
        case .success:
            print("Removed user from the thread.")
        case .failure:
            print("Failed to remove user from the thread.")
        }
    }
```

`<USER ID>`Kaldırılmakta olan katılımcının Iletişim Hizmetleri Kullanıcı kimliğiyle değiştirin.

## <a name="run-the-code"></a>Kodu çalıştırma

Xcode 'da, projeyi derlemek ve çalıştırmak için **Çalıştır** ' ı seçin. Konsolunda, sohbet istemcisinden koddan ve günlükçü çıktısından çıktıyı görüntüleyebilirsiniz.

