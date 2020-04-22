---
title: IoT Hub bağlantısını güvenilir mesajlaşma w/device SDK'ları & yönetme
description: Azure IoT Hub aygıtı SDK'larını kullanırken cihaz bağlantınızı ve mesajlaşmanızı nasıl geliştireceğinizi öğrenin
services: iot-hub
author: robinsh
ms.author: robinsh
ms.date: 07/07/2018
ms.topic: article
ms.service: iot-hub
ms.custom:
- amqp
- mqtt
ms.openlocfilehash: a645ab1fa4a1be3ec8e939452a1457e84bbafe15
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759685"
---
# <a name="manage-connectivity-and-reliable-messaging-by-using-azure-iot-hub-device-sdks"></a>Azure IoT Hub aygıt SDK'larını kullanarak bağlantı ve güvenilir mesajlaşmayı yönetme

Bu makalede, daha esnek aygıt uygulamaları tasarlamanıza yardımcı olmak için üst düzey kılavuz sağlar. Azure IoT aygıt SDK'larının bağlantı ve güvenilir mesajlaşma özelliklerinden nasıl yararlanabileceğinizi gösterir. Bu kılavuzun amacı, aşağıdaki senaryoları yönetmenize yardımcı olmaktır:

* Bırakılan ağ bağlantısını düzeltme

* Farklı ağ bağlantıları arasında geçiş yapma

* Hizmet geçici bağlantı hataları nedeniyle yeniden bağlanma

Uygulama ayrıntıları dile göre değişebilir. Daha fazla bilgi için API belgelerine veya belirli SDK'ya bakın:

* [C/iOS SDK](https://github.com/azure/azure-iot-sdk-c)

* [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/retrypolicy.md)

* [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)

* [Node SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

* [Python SDK](https://github.com/Azure/azure-iot-sdk-python) (Güvenilirlik henüz uygulanmadı)

## <a name="designing-for-resiliency"></a>Dayanıklı olacak şekilde tasarlama

IoT aygıtları genellikle sürekli olmayan veya kararsız ağ bağlantılarına (örneğin, GSM veya uydu) güvenir. Aralıklı hizmet kullanılabilirliği ve altyapı düzeyinde veya geçici hatalar nedeniyle aygıtlar bulut tabanlı hizmetlerle etkileşime girdiğinde hatalar oluşabilir. Aygıtta çalışan bir uygulama, ileti gönderme ve alma mekanizmalarını yönetmesi gerekir. Ayrıca, yeniden deneme stratejisi gereksinimleri büyük ölçüde cihazın IoT senaryosuna, bağlamına, yeteneklerine bağlıdır.

Azure IoT Hub aygıt SDK'ları, buluttan cihaza ve cihazdan buluta bağlanmayı ve iletişimi basitleştirmeyi amaçlamaktadır. Bu SDK'lar, Azure IoT Hub'ına bağlanmak için güçlü bir yol ve ileti gönderip almak için kapsamlı bir seçenek kümesi sağlar. Geliştiriciler, belirli bir senaryo için daha iyi bir yeniden deneme stratejisini özelleştirmek için varolan uygulamayı da değiştirebilir.

Bağlantı ve güvenilir mesajlaşmayı destekleyen ilgili SDK özellikleri aşağıdaki bölümlerde ele alınmıştır.

## <a name="connection-and-retry"></a>Bağlantı ve yeniden deneme

Bu bölümde, bağlantıları yönetirken kullanılabilir yeniden bağlantı ve yeniden deneme desenleri hakkında genel bir bakış sunulmaktadır. Cihaz uygulamanızda farklı bir yeniden deneme ilkesi kullanmak için uygulama kılavuzunu ayrıntılarıyla anlatır ve aygıt SDK'larından ilgili API'leri listeler.

### <a name="error-patterns"></a>Hata desenleri

Bağlantı hataları birçok düzeyde gerçekleşebilir:

* Ağ hataları: kopuk yuva ve ad çözümleme hataları

* HTTP, AMQP ve MQTT aktarımiçin protokol düzeyinde hatalar: ayrılmış bağlantılar veya süresi dolmuş oturumlar

* Yerel hatalardan kaynaklanan uygulama düzeyi hataları: geçersiz kimlik bilgileri veya hizmet davranışı (örneğin, kotayı aşma veya azaltma)

Aygıt SDK'ları her üç düzeydede de hataları algılar. İşletim sistemi yle ilgili hatalar ve donanım hataları aygıt SDK'ları tarafından algılanmaz ve işlenmez. SDK tasarımı, Azure Mimari [Merkezi'nin Geçici Hata İşleme Kılavuzu'nu](/azure/architecture/best-practices/transient-faults#general-guidelines) temel almaktadır.

### <a name="retry-patterns"></a>Desenleri yeniden deneyin

Bağlantı hataları algılandığında aşağıdaki adımlar yeniden deneme işlemini açıklar:

1. SDK, ağ, protokol veya uygulamadaki hatayı ve ilişkili hatayı algılar.

2. SDK hata türünü belirlemek ve yeniden deneme gerekip gerekmeden karar vermek için hata filtresini kullanır.

3. SDK **kurtarılamayan**bir hata tanımlarsa, bağlantı, gönderme ve alma gibi işlemler durdurulur. SDK kullanıcıya durumu iletin. Kurtarılamayan hatalara örnek olarak kimlik doğrulama hatası ve kötü bir bitiş noktası hatası verilebilir.

4. SDK **kurtarılabilir**bir hata tanımlarsa, tanımlanan zaman aşımı geçene kadar belirtilen yeniden deneme ilkesine göre yeniden çalışır.  SDK'nın varsayılan olarak jitter retry **ilkesiyle Üstel geri tepme** kullandığını unutmayın.
5. Tanımlanan zaman aşımı süresi dolduğunda, SDK bağlanmaya veya göndermeyi denemeyi durdurur. Kullanıcıya durumu dindirer.

6. SDK, kullanıcının bağlantı durumu değişiklikleri almak için geri arama eklemesine izin verir.

SDK'lar üç yeniden deneme ilkesağlar:

* **Jitter ile üstel geri tepme**: Bu varsayılan yeniden deneme ilkesi başlangıçta agresif olma eğilimindedir ve maksimum gecikmeye ulaşınceye kadar zaman içinde yavaşlar. Tasarım, Azure [Architecture Center'ın Yeniden Deneme kılavuzuna](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific)dayanmaktadır. 

* **Özel yeniden deneme**: Bazı SDK dilleri için, senaryonuz için daha uygun olan özel bir yeniden deneme ilkesi tasarlayabilir ve ardından retryPolicy'ye enjekte edebilirsiniz. Özel yeniden deneme C SDK'da kullanılamaz ve şu anda Python SDK'da desteklenmez. Python SDK gerektiği gibi yeniden bağlanır.

* **Yeniden deneme yok**: Yeniden deneme mantığını devre dışı bırakan yeniden deneme ilkesini "yeniden deneme yok" olarak ayarlayabilirsiniz. SDK, bağlantının kurulduğunu varsayarak bir kez bağlanmaya ve bir kez mesaj göndermeye çalışır. Bu ilke genellikle bant genişliği veya maliyet kaygıları olan senaryolarda kullanılır. Bu seçeneği seçerseniz, gönderemeyen iletiler kaybolur ve kurtarılamaz.

### <a name="retry-policy-apis"></a>Yeniden deneme ilkesi API'leri

   | SDK | SetRetryPolicy yöntemi | İlke uygulamaları | Uygulama kılavuzu |
   |-----|----------------------|--|--|
   |  C/iOS  | [IOTHUB_CLIENT_RESULT IoTHubClient_SetRetryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/2018-05-04/iothub_client/inc/iothub_client.h#L188)        | **Varsayılan**: [IOTHUB_CLIENT_RETRY_EXPONENTIAL_BACKOFF](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Özel:** kullanılabilir [retryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies) kullanın<BR>**Yeniden deneme yok:** [IOTHUB_CLIENT_RETRY_NONE](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)  | [C/iOS uygulaması](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#)  |
   | Java| [SetRetryPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device.deviceclientconfig.setretrypolicy?view=azure-java-stable)        | **Varsayılan**: [Üstel BackoffWithJitter sınıfı](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)<BR>**Özel:** [RetryPolicy arabirimini uygulayın](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/RetryPolicy.java)<BR>**Yeniden deneme yok:** [NoRetry sınıfı](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)  | [Java uygulaması](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md) |
   | .NET| [DeviceClient.SetRetryPolicy](/dotnet/api/microsoft.azure.devices.client.deviceclient.setretrypolicy?view=azure-dotnet) | **Varsayılan**: [Üstel Backoff sınıfı](/dotnet/api/microsoft.azure.devices.client.exponentialbackoff?view=azure-dotnet)<BR>**Özel:** [IRetryPolicy arabirimini](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.iretrypolicy?view=azure-dotnet) uygulayın<BR>**Yeniden deneme yok:** [NoRetry sınıfı](/dotnet/api/microsoft.azure.devices.client.noretry?view=azure-dotnet) | [C# uygulaması](https://github.com/Azure/azure-iot-sdk-csharp) | |
   | Node| [setRetryPolitika](/javascript/api/azure-iot-device/client?view=azure-iot-typescript-latest) | **Varsayılan**: [Üstel BackoffWithJitter sınıfı](/javascript/api/azure-iot-common/exponentialbackoffwithjitter?view=azure-iot-typescript-latest)<BR>**Özel:** [RetryPolicy arabirimini uygulayın](/javascript/api/azure-iot-common/retrypolicy?view=azure-iot-typescript-latest)<BR>**Yeniden deneme yok:** [NoRetry sınıfı](/javascript/api/azure-iot-common/noretry?view=azure-iot-typescript-latest) | [Düğüm uygulaması](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them) |
   | Python| Şu anda desteklenmiyor | Şu anda desteklenmiyor | Şu anda desteklenmiyor |

Aşağıdaki kod örnekleri bu akışı göstermektedir:

#### <a name="net-implementation-guidance"></a>.NET uygulama kılavuzu

Aşağıdaki kod örneği, varsayılan yeniden deneme ilkesinin nasıl tanımlanır ve ayarlanılır:

   ```csharp
   // define/set default retry policy
   IRetryPolicy retryPolicy = new ExponentialBackoff(int.MaxValue, TimeSpan.FromMilliseconds(100), TimeSpan.FromSeconds(10), TimeSpan.FromMilliseconds(100));
   SetRetryPolicy(retryPolicy);
   ```

Yüksek CPU kullanımını önlemek için, kod hemen başarısız olursa yeniden denemeler daraltılır. Örneğin, hedefe giden ağ veya rota olmadığında. Bir sonraki yeniden denemeyi yürütmek için en az süre 1 saniyedir.

Hizmet azaltma hatasıyla yanıt veriyorsa, yeniden deneme ilkesi farklıdır ve genel API aracılığıyla değiştirilemez:

   ```csharp
   // throttled retry policy
   IRetryPolicy retryPolicy = new ExponentialBackoff(RetryCount, TimeSpan.FromSeconds(10), 
     TimeSpan.FromSeconds(60), TimeSpan.FromSeconds(5)); SetRetryPolicy(retryPolicy);
   ```

Yeniden deneme `DefaultOperationTimeoutInMilliseconds`mekanizması, şu anda 4 dakika olarak ayarlanmış sonra durur.

#### <a name="other-languages-implementation-guidance"></a>Diğer diller uygulama kılavuzu

Diğer dillerdeki kod örnekleri için aşağıdaki uygulama belgelerini gözden geçirin. Depo, yeniden deneme ilkesi API'lerinin kullanımını gösteren örnekler içerir.

* [C/iOS SDK](https://github.com/azure/azure-iot-sdk-c)

* [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/retrypolicy.md)

* [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)

* [Node SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

* [Python SDK'sı](https://github.com/Azure/azure-iot-sdk-python)

## <a name="next-steps"></a>Sonraki adımlar

* [Cihaz ve hizmet SDK’ları kullanma](./iot-hub-devguide-sdks.md)

* [C için IoT cihaz SDK'sını kullanma](./iot-hub-device-sdk-c-intro.md)

* [Kısıtlanmış cihazlar için geliştirme](./iot-hub-devguide-develop-for-constrained-devices.md)

* [Mobil cihazlar için geliştirme](./iot-hub-how-to-develop-for-mobile-devices.md)

* [Sorun giderme aygıtı nın bağlantısı kesilir](iot-hub-troubleshoot-connectivity.md)
