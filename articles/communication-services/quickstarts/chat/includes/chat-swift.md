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
ms.openlocfilehash: b7bf924ba8514aa8da1d466ea4852f3f9caaf646
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105726700"
---
## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce şunları yaptığınızdan emin olun:

- Etkin abonelikle bir Azure hesabı oluşturun. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- [Xcode](https://developer.apple.com/xcode/) ve [Cocoapods](https://cocoapods.org/)' yi yüklemek için Xcode 'u kullanarak hızlı başlangıç için bir iOS uygulaması, bağımlılıkları yüklemek için Cocoapods kullanılır.
- Bir Azure Iletişim Hizmetleri kaynağı oluşturun. Ayrıntılar için bkz. [Azure Iletişim kaynağı oluşturma](../../create-communication-resource.md). Bu hızlı başlangıç için **Kaynak uç noktanızı kaydetmeniz** gerekir.
- **İki** ACS kullanıcısı oluşturun ve kullanıcılara bir Kullanıcı erişim belirteci [Kullanıcı erişim belirteci](../../access-tokens.md)verin. Kapsamı **sohbet** olarak ayarladığınızdan emin olun ve **belirteç dizesinin yanı sıra Kullanıcı kimliği dizesini de unutmayın**. Bu hızlı başlangıçta, ilk katılımcı içeren bir iş parçacığı oluşturacağız ve sonra iş parçacığına ikinci bir katılımcı ekleyeceğiz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-ios-application"></a>Yeni bir iOS uygulaması oluşturma

Xcode ' u açın ve öğesini seçin `Create a new Xcode project` .

Sonraki pencerede, `iOS` Platform ve şablon olarak öğesini seçin `App` .

Seçenekleri seçerken `ChatQuickstart` Proje adı olarak girin. `Storyboard` `UIKit App Delegate` Yaşam döngüsü olarak ve dil olarak arabirim olarak ' u seçin `Swift` .

İleri ' ye tıklayın ve projenin oluşturulmasını istediğiniz dizini seçin.

### <a name="install-the-libraries"></a>Kitaplıkları yükler

Gerekli Iletişim Hizmetleri bağımlılıklarını yüklemek için Cocoapods kullanacağız.

Komut satırından iOS projesinin kök dizininde gezinin `ChatQuickstart` .

Pod dosyası oluşturma: `pod init`

Pod dosyasını açın ve şu bağımlılıkları `ChatQuickstart` hedefe ekleyin:
```
pod 'AzureCommunication', '~> 1.0.0-beta.9'
pod 'AzureCommunicationChat', '~> 1.0.0-beta.9'
```

Bağımlılıkları yükledikten sonra da bir Xcode çalışma alanı oluşturulur: `pod install`

**Pod Install çalıştırıldıktan sonra, yeni oluşturulan öğesini seçerek projeyi Xcode 'da yeniden açın `.xcworkspace` .**

### <a name="setup-the-placeholders"></a>Yer tutucuları ayarlama

Çalışma alanını `ChatQuickstart.xcworkspace` Xcode 'da açın ve sonra açın `ViewController.swift` .

Bu hızlı başlangıçta, kodumuzu ' a ekleyecek `viewController` ve Xcode konsolundaki çıktıyı görüntüleyecağız. Bu hızlı başlangıç, iOS 'ta bir kullanıcı arabirimi oluşturmayı gidermez. 

`viewController.swift`Ve kitaplıklarını içeri aktarma en üst `AzureCommunication` kısmında `AzureCommunicatonChat` :

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

Tanıtım amacıyla kodumuzu senkronize etmek için bir semafor kullanacağız. Aşağıdaki adımlarda, Azure Iletişim Hizmetleri sohbet kitaplığı 'nı kullanarak yer tutucuları örnek kodla değiştireceksiniz.


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

`<ACS_RESOURCE_ENDPOINT>`ACS kaynağınızın bitiş noktasıyla değiştirin.
`<ACCESS_TOKEN>`Geçerli BIR ACS erişim belirteciyle değiştirin.

Bu hızlı başlangıç, sohbet uygulamanız için belirteçleri yönetmek üzere bir hizmet katmanı oluşturmayı kapsamaz, ancak önerilir. Daha ayrıntılı [sohbet mimarisi](../../../concepts/chat/concepts.md) için aşağıdaki belgelere bakın

[Kullanıcı erişim belirteçleri](../../access-tokens.md)hakkında daha fazla bilgi edinin.

## <a name="object-model"></a>Nesne modeli 
Aşağıdaki sınıflar ve arabirimler, JavaScript için Azure Communication Services sohbet SDK 'sının önemli özelliklerinden bazılarını idare edebilir.

| Ad                                   | Açıklama                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChatClient | Bu sınıf, sohbet işlevselliği için gereklidir. Bunu Abonelik bilgileriniz ile birlikte başlatır ve iş parçacıklarını oluşturmak, almak ve silmek için kullanın. |
| ChatThreadClient | Bu sınıf, sohbet Iş parçacığı işlevselliği için gereklidir. ChatClient aracılığıyla bir örnek edinirsiniz ve bu örneği kullanarak ileti gönderebilir/alabilir/güncelleştirebilir/silebilirsiniz, kullanıcıları ekleyin/kaldırın/alın, yazma bildirimleri gönderin ve okundu bilgilerini okuyun, sohbet etkinliklerine abone olun. |

## <a name="start-a-chat-thread"></a>Sohbet iş parçacığı başlatma

Şimdi, `ChatClient` ilk Kullanıcı ile yeni bir iş parçacığı oluşturmak için kullanıyoruz.

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

Devam etmeden önce tamamlama işleyicisini beklemek için burada bir semafor kullanıyoruz. `threadId`Sonraki adımlarda, tamamlanma işleyicisine döndürülen yanıtı kullanacağız.

## <a name="get-a-chat-thread-client"></a>Sohbet iş parçacığı istemcisi al

Artık bir sohbet iş parçacığı oluşturduğumuz `ChatThreadClient` için iş parçacığı içinde işlem gerçekleştirmek üzere bir de elde edeceğiz.

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

İlk `SendChatMessageRequest` olarak içerik ve gönderenlerin görünen adını içeren öğesini oluşturacağız (Ayrıca isteğe bağlı olarak, paylaşma geçmişi süresini de içerebilir). Tamamlanma işleyicisine döndürülen yanıt, gönderilen iletinin KIMLIĞINI içerir.

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

`<USER_ID>`Eklenecek kullanıcının ACS kullanıcı kimliğiyle değiştirin.

Bir iş parçacığına katılımcı eklerken, tamamlanma sonucu döndürülen yanıt hata içerebilir. Bu hatalar belirli katılımcıları ekleme başarısızlığını temsil eder.

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

Xcode 'da projeyi derlemek ve çalıştırmak için Çalıştır düğmesine basın. Konsolunda, koddan çıktıyı ve günlükçü çıkışını ChatClient ' dan görüntüleyebilirsiniz.

