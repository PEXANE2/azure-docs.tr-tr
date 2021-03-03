---
title: Azure Iletişim hizmetlerinde sorun giderme
description: Iletişim Hizmetleri çözümünüzün sorunlarını gidermek için gereken bilgileri nasıl toplayacağınızı öğrenin.
author: manoskow
manager: jken
services: azure-communication-services
ms.author: manoskow
ms.date: 10/23/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: f0bcc4b4c900ba53ecd780530ce61487bcc998a4
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658206"
---
# <a name="troubleshooting-in-azure-communication-services"></a>Azure Iletişim hizmetlerinde sorun giderme

Bu belge, Iletişim Hizmetleri çözümünüz dahilinde karşılaşabileceğiniz sorunları gidermenize yardımcı olur. SMS sorunlarını giderirken, SMS teslimi ayrıntılarını yakalamak için [Event Grid ile teslim raporlamayı etkinleştirebilirsiniz](../quickstarts/telephony-sms/handle-sms-events.md) .

## <a name="getting-help"></a>Yardım alma

Geliştiricilerin Iletişim Hizmetleri [GitHub deposunda](https://github.com/Azure/communication)soru göndermelerini, özellik önermesine ve sorunları sorun olarak raporlamalarını öneririz. Diğer Forumlar şunlardır:

* [Microsoft Soru-Cevap](/answers/questions/topics/single/101418.html)
* [StackOverflow](https://stackoverflow.com/questions/tagged/azure+communication)

Azure abonelik [destek planınıza](https://azure.microsoft.com/support/plans/) bağlı olarak, [Azure Portal](https://azure.microsoft.com/support/create-ticket/)doğrudan bir destek bileti gönderebilirsiniz.

Belirli sorun türleriyle ilgili sorunları gidermenize yardımcı olması için aşağıdaki bilgi parçalarından herhangi biri istenebilir:

* **MS-CV kimliği**: Bu kimlik, çağrı ve iletilerle ilgili sorunları gidermek için kullanılır.
* **Çağrı kimliği**: Bu kimlik, iletişim hizmetleri çağrılarını belirlemek için kullanılır.
* **SMS ILETI kimliği**: bu KIMLIK, SMS iletilerini tanımlamak için kullanılır.
* **Çağrı günlükleri**: Bu Günlükler, çağrı ve ağ sorunlarını gidermek için kullanılabilecek ayrıntılı bilgiler içerir.


## <a name="access-your-ms-cv-id"></a>MS-CV KIMLIĞINIZE erişin

MS-CV KIMLIĞI, `clientOptions` istemci kitaplıklarınızı başlatırken nesne örneğinde tanılama yapılandırılarak erişilebilir. Tanılama, kimlik ve VoIP çağrısı dahil olmak üzere Azure istemci kitaplıklarının herhangi birine yönelik olarak yapılandırılabilir.

### <a name="client-options-example"></a>İstemci seçenekleri örneği

Aşağıdaki kod parçacıkları tanılama yapılandırmasını gösterir. İstemci kitaplıkları tanılama etkinken kullanıldığında, tanılama ayrıntıları yapılandırılan olay dinleyicisine yayılır:

# <a name="c"></a>[C#](#tab/csharp)
```
// 1. Import Azure.Core.Diagnostics
using Azure.Core.Diagnostics;

// 2. Initialize an event source listener instance
using var listener = AzureEventSourceListener.CreateConsoleLogger();
Uri endpoint = new Uri("https://<RESOURCE-NAME>.communication.azure.net");
var (token, communicationUser) = await GetCommunicationUserAndToken();
CommunicationUserCredential communicationUserCredential = new CommunicationUserCredential(token);

// 3. Setup diagnostic settings
var clientOptions = new ChatClientOptions()
{
    Diagnostics =
    {
        LoggedHeaderNames = { "*" },
        LoggedQueryParameters = { "*" },
        IsLoggingContentEnabled = true,
    }
};

// 4. Initialize the ChatClient instance with the clientOptions
ChatClient chatClient = new ChatClient(endpoint, communicationUserCredential, clientOptions);
ChatThreadClient chatThreadClient = await chatClient.CreateChatThreadAsync("Thread Topic", new[] { new ChatThreadMember(communicationUser) });
```

# <a name="python"></a>[Python](#tab/python)
```
from azure.communication.chat import ChatClient, CommunicationUserCredential
endpoint = "https://communication-services-sdk-live-tests-for-python.communication.azure.com"
chat_client = ChatClient(
    endpoint,
    CommunicationUserCredential(token),
    http_logging_policy=your_logging_policy)
```
---

## <a name="access-your-call-id"></a>Çağrı KIMLIĞINIZE erişin

Çağrı sorunlarıyla ilgili Azure portal bir destek isteği dosyalayarak, başvurduğunuz çağrının KIMLIĞINI sağlamanız istenebilir. Bu, çağıran istemci kitaplığı aracılığıyla erişilebilir:

# <a name="javascript"></a>[JavaScript](#tab/javascript)
```javascript
// `call` is an instance of a call created by `callAgent.call` or `callAgent.join` methods
console.log(call.id)
```

# <a name="ios"></a>[iOS](#tab/ios)
```objc
// The `call id` property can be retrieved by calling the `call.getCallId()` method on a call object after a call ends
// todo: the code snippet suggests it's a property while the comment suggests it's a method call
print(call.callId)
```

# <a name="android"></a>[Android](#tab/android)
```java
// The `call id` property can be retrieved by calling the `call.getCallId()` method on a call object after a call ends
// `call` is an instance of a call created by `callAgent.call(…)` or `callAgent.join(…)` methods
Log.d(call.getCallId())
```
---


## <a name="access-your-sms-message-id"></a>SMS ileti KIMLIĞINIZE erişin

SMS sorunları için, yanıt nesnesinden ileti KIMLIĞINI toplayabilirsiniz.

# <a name="net"></a>[.NET](#tab/dotnet)
```
// Instantiate the SMS client
const smsClient = new SmsClient(connectionString);
async function main() {
  const result = await smsClient.send({
    from: "+18445792722",
    to: ["+1972xxxxxxx"],
    message: "Hello World 👋🏻 via Sms"
  }, {
    enableDeliveryReport: true // Optional parameter
  });
console.log(result); // your message ID will be in the result
}
```
---

## <a name="enable-and-access-call-logs"></a>Çağrı günlüklerini etkinleştirme ve erişme




# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki kod, `AzureLogger` JavaScript istemci kitaplığı kullanılarak günlükleri konsola çıkarmak için yapılandırmak üzere kullanılabilir:

```javascript
import { AzureLogger } from '@azure/logger';

AzureLogger.verbose = (...args) => { console.info(...args); }
AzureLogger.info = (...args) => { console.info(...args); }
AzureLogger.warning = (...args) => { console.info(...args); }
AzureLogger.error = (...args) => { console.info(...args); }

callClient = new CallClient({logger: AzureLogger});
```

# <a name="ios"></a>[iOS](#tab/ios)

İOS için geliştirme yaparken günlüklerinizin dosyalarında saklanması gerekir `.blog` . Şifrelenmiş olduklarından günlükleri doğrudan görüntüleyemediğini unutmayın.

Bunlar, Xcode açılarak erişilebilir. Windows > cihazlarına ve > simülatörleri cihazlarına gidin. Cihazınızı seçin. Yüklü uygulamalar altında uygulamanızı seçin ve "kapsayıcıyı Indir" e tıklayın.

Bu size bir dosya verecektir `xcappdata` . Bu dosyaya sağ tıklayın ve "paket içeriğini göster" i seçin. Daha sonra `.blog` Azure destek isteğinize iliştirebilmeniz için dosyaları görürsünüz.

# <a name="android"></a>[Android](#tab/android)

Android için geliştirme yaparken günlüklerinizin dosyalarında saklanması gerekir `.blog` . Şifrelenmiş olduklarından günlükleri doğrudan görüntüleyemediğini unutmayın.

Android Studio, hem Benzetici hem de cihazdan cihaz dosya Gezgini > > araç pencerelerini görüntüle ' yi seçerek cihaz dosya Gezgini 'ne gidin. `.blog`Dosya uygulamanızın dizininde bulunur, bu da benzer bir şekilde görünmelidir `/data/data/[app_name_space:com.contoso.com.acsquickstartapp]/files/acs_sdk.blog` . Bu dosyayı destek isteğinize ekleyebilirsiniz.


---


## <a name="calling-client-library-error-codes"></a>İstemci kitaplığı hata kodlarını çağırma

İstemci kitaplığını çağıran Azure Iletişim Hizmetleri, sorunları gidermeye yardımcı olması için aşağıdaki hata kodlarını kullanır. Bu hata kodları, `call.callEndReason` bir çağrı bittikten sonra özelliği aracılığıyla gösterilir.

| Hata kodu | Açıklama | Gerçekleştirilecek eylem |
| -------- | ---------------| ---------------|
| 403 | Yasak/kimlik doğrulama hatası. | Iletişim Hizmetleri belirtecinizin geçerli olduğundan ve geçerliliği dolmadığından emin olun. |
| 404 | Çağrı bulunamadı. | Çağırdığınız numaranın (veya katılmakta olduğunuz çağrı) bulunduğundan emin olun. |
| 408 | Çağrı denetleyicisi zaman aşımına uğradı. | Arama denetleyicisi, Kullanıcı uç noktalarından protokol iletilerini beklerken zaman aşımına uğradı. İstemcilerin bağlı ve kullanılabilir olduğundan emin olun. |
| 410 | Yerel Medya yığını veya medya altyapısı hatası. | Desteklenen bir ortamda en son istemci kitaplığını kullandığınızdan emin olun. |
| 430 | İleti istemci uygulamasına teslim edilemiyor. | İstemci uygulamasının çalıştığından ve kullanılabilir olduğundan emin olun. |
| 480 | Uzak istemci uç noktası kayıtlı değil. | Uzak uç noktanın kullanılabilir olduğundan emin olun. |
| 481 | Gelen çağrı işlenemedi. | Azure portal aracılığıyla bir destek isteği dosyası oluşturma. |
| 487 | Çağrı iptal edildi, yerel olarak reddedildi, bir uç nokta uyuşmazlığı sorunu nedeniyle sonlandırıldı veya medya teklifi oluşturulamadı. | Beklenen davranış. |
| 490, 491, 496, 487, 498 | Yerel uç nokta ağı sorunları. | Ağınızı kontrol edin. |
| 500, 503, 504 | İletişim Hizmetleri altyapı hatası. | Azure portal aracılığıyla bir destek isteği dosyası oluşturma. |
| 603 | Uzaktan Iletişim Hizmetleri katılımcısı tarafından genel olarak reddedilen çağrı | Beklenen davranış. |


## <a name="related-information"></a>İlgili bilgiler
- [Günlükler ve Tanılamalar](logging-and-diagnostics.md)
- [Ölçümler](metrics.md)