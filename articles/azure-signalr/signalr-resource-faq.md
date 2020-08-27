---
title: Azure SignalR hizmeti hakkında sık sorulan sorular
description: Azure SignalR hizmeti hakkında sık sorulan soruların ve sorun giderme ve tipik kullanım senaryolarında hızlı bir şekilde erişmesini sağlayabilirsiniz.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.custom: devx-track-dotnet
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: 9aa510a055cb76b30508cb98a25cd9c919eb117d
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88928318"
---
# <a name="azure-signalr-service-faq"></a>Azure SignalR hizmeti hakkında SSS

## <a name="is-azure-signalr-service-ready-for-production-use"></a>Azure SignalR hizmeti üretim kullanımı için hazırlanıyor mu?

Evet.
Genel kullanılabilirlik duyurumız için bkz. [Azure SignalR hizmeti genel kullanıma sunuldu](https://azure.microsoft.com/blog/azure-signalr-service-now-generally-available/). 

[ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction) tam olarak desteklenir.

ASP.NET SignalR desteği hala *genel önizlemede*. Aşağıda bir [kod örneği](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom)verilmiştir.

## <a name="the-client-connection-closes-with-the-error-message-no-server-available-what-does-it-mean"></a>Istemci bağlantısı, "sunucu kullanılamıyor" hata iletisiyle kapanıyor. Ne anlama geliyor?

Bu hata yalnızca istemciler SignalR hizmetine ileti gönderirken oluşur.

Herhangi bir uygulama sunucunuz yoksa ve yalnızca SignalR hizmeti REST API kullanıyorsanız, bu davranış **tasarıma göre**yapılır.
Sunucusuz mimaride, istemci bağlantıları **dinleme** modundadır ve SignalR hizmetine ileti göndermez.
[REST API](./signalr-quickstart-rest-api.md)hakkında daha fazla bilgi edinin.

Uygulama sunucularınız varsa, bu hata iletisi, SignalR hizmet örneğinize bağlı hiçbir uygulama sunucusu olmadığı anlamına gelir.

Olası nedenler şunlardır:
- SignalR hizmeti ile hiçbir uygulama sunucusu bağlanmadı. Olası bağlantı hataları için uygulama sunucusu günlüklerine bakın. Bu durumda, birden fazla uygulama sunucusu olan yüksek kullanılabilirlik ayarında nadir olarak karşılaşılır.
- SignalR hizmet örneklerinde bağlantı sorunları var. Bu sorun geçicidir ve otomatik olarak kurtarılır.
Bir saatten uzun süre devam ederse, [GitHub 'da bir sorun açın](https://github.com/Azure/azure-signalr/issues/new) veya [Azure 'da bir destek isteği oluşturun](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

## <a name="when-there-are-multiple-application-servers-are-client-messages-sent-to-all-servers-or-just-one-of-them"></a>Birden çok uygulama sunucusu olduğunda, istemci iletileri tüm sunuculara mı, yoksa yalnızca biri mi gönderilir?

İstemci ve uygulama sunucusu arasında bire bir eşleme olur. Bir istemciden gelen iletiler her zaman aynı uygulama sunucusuna gönderilir.

İstemci veya uygulama sunucusu bağlantısı kesilene kadar, istemci ve uygulama sunucusu arasındaki eşleme korunur.

## <a name="if-one-of-my-application-servers-is-down-how-can-i-find-it-and-get-notified"></a>Uygulama sunucularım kapalıysa, nasıl öğrenebilirim ve bildirim alabilir?

SignalR hizmeti, uygulama sunucularından sinyalleri izler.
Belirli bir süre için sinyaller alınmıyorsa, uygulama sunucusu çevrimdışı kabul edilir. Bu uygulama sunucusuyla eşlenen tüm istemci bağlantılarının bağlantısı kesilecek.

## <a name="why-does-my-custom-iuseridprovider-throw-exception-when-switching-from-aspnet-core-signalr--sdk-to-azure-signalr-service-sdk"></a>`IUserIdProvider`ASP.NET Core SIGNALR SDK 'Dan Azure SignalR hizmeti SDK 'sına geçiş yaparken özel throw özel durumu neden kullanılır?

Parametresi, `HubConnectionContext context` çağrıldığında ASP.NET Core SIGNALR SDK ve Azure SignalR hizmeti SDK 'sı arasında farklıdır `IUserIdProvider` .

ASP.NET Core SignalR 'de, `HubConnectionContext context` fiziksel istemci bağlantısının tüm özellikler için geçerli değerlerle olan bağlamıdır.

Azure SignalR hizmeti SDK 'sında, `HubConnectionContext context` mantıksal istemci bağlantısının bağlamıdır. Fiziksel istemci bağlantısı, SignalR hizmeti örneğine bağlanır, bu nedenle yalnızca sınırlı sayıda özellik sağlanır.

Şimdilik yalnızca `HubConnectionContext.GetHttpContext()` ve `HubConnectionContext.User` erişim için kullanılabilir.
Kaynak kodunu [buradan](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/HubHost/ServiceHubConnectionContext.cs)denetleyebilirsiniz.

## <a name="can-i-configure-the-transports-available-in-signalr-service-as-configuring-it-on-server-side-with-aspnet-core-signalr-for-example-disable-websocket-transport"></a>SignalR hizmetinde bulunan aktarımları, sunucu tarafında ASP.NET Core SignalR ile yapılandırma olarak yapılandırabilir miyim? Örneğin, WebSocket aktarımını devre dışı bırakmak istiyor musunuz?

Hayır.

Azure SignalR hizmeti, ASP.NET Core SignalR 'nin desteklediği üç aktarımı varsayılan olarak sağlar. Yapılandırılamaz. SignalR hizmeti, tüm istemci bağlantıları için bağlantıları ve aktarımları işleyecek.

İstemci tarafı taşımalarını [burada](https://docs.microsoft.com/aspnet/core/signalr/configuration?view=aspnetcore-2.1&tabs=dotnet#configure-allowed-transports-2)belgelenen şekilde yapılandırabilirsiniz.

## <a name="what-is-the-meaning-of-metrics-like-message-count-or-connection-count-showed-in-azure-portal-which-kind-of-aggregation-type-should-i-choose"></a>İleti sayısı veya Azure portal gösterilen bağlantı sayısı gibi ölçümlerin anlamı nedir? Ne tür bir toplama türü seçmem gerekir?

Bu ölçümleri nasıl hesaplayabiliriz hakkındaki ayrıntıları [burada](signalr-concept-messages-and-connections.md)bulabilirsiniz.

Azure SignalR hizmeti kaynaklarının genel bakış dikey penceresinde, sizin için uygun toplama türünü zaten seçtik. Ölçümler dikey penceresine giderseniz, [toplama türünü bir](../azure-monitor/platform/metrics-supported.md#microsoftsignalrservicesignalr) başvuru olarak alabilirsiniz.

## <a name="what-is-the-meaning-of-service-mode-defaultserverlessclassic-how-can-i-choose"></a>Hizmet modunun anlamı nedir `Default` / `Serverless` / `Classic` ? Nasıl seçebilirim?

Mod
* `Default` mod, hub sunucusu *gerektiriyor* . Bu modda, Azure SignalR istemci trafiğini bağlı hub sunucusu bağlantılarına yönlendirir. Azure SignalR bağlı bir hub sunucusunu denetler. Bağlı bir hub sunucusu bulunamazsa, Azure SignalR gelen istemci bağlantılarını reddeder. Ayrıca, bağlantılı istemcileri doğrudan Azure SignalR üzerinden yönetmek için bu modda **Yönetim API 'sini** de kullanabilirsiniz.
* `Serverless` mod hiçbir sunucu bağlantısına izin *vermiyor* , yani tüm sunucu bağlantılarını reddeder. Tüm istemciler sunucusuz modda olmalıdır. İstemciler Azure SignalR 'ye bağlanır ve kullanıcılar, genellikle Merkez LOGI 'yi işlemek için **Azure işlevi** gibi sunucusuz teknolojiler kullanır. Azure SignalR 'nin sunucusuz modunu kullanan [basit bir örneğe](https://docs.microsoft.com/azure/azure-signalr/signalr-quickstart-azure-functions-javascript?WT.mc_id=signalrquickstart-github-antchu) bakın.
* `Classic` mod, karışık bir durum. Hub 'ın sunucu bağlantısı olduğunda, yeni istemci hub sunucusuna yönlendirilir, aksi takdirde istemci sunucusuz moduna girer.

  Bu durum bir sorun oluşmasına neden olabilir, örneğin, tüm sunucu bağlantıları bir süre boyunca kaybedildiğinden, bazı istemciler hub sunucusu 'na yönlendirme yerine sunucusuz moda girer.

Yaparak
1. Hub sunucusu yok, öğesini seçin `Serverless` .
1. Tüm Hub 'larda hub sunucuları vardır, öğesini seçin `Default` .
1. Hub 'lardan bazılarının hub sunucuları var, diğerleri değil, `Classic` ancak bu sorun bir soruna neden olabilir. Bu, daha iyi bir yöntem, diğeri de bir tane olmak üzere iki örnek oluşturur `Serverless` `Default` .

## <a name="any-feature-differences-when-using-azure-signalr-for-aspnet-signalr"></a>ASP.NET SignalR için Azure SignalR kullanırken herhangi bir özellik farkı var mı?
Azure SignalR kullanırken, ASP.NET SignalR 'nin bazı API 'Leri ve özellikleri artık desteklenmemektedir:
- Azure SignalR kullanılırken istemciler ve merkez arasında rastgele durum geçirme özelliği (genellikle çağrılır `HubState` ) desteklenmez
- `PersistentConnection` sınıf, Azure SignalR kullanılırken henüz desteklenmiyor
- Azure SignalR kullanılırken **süresiz çerçeve taşıması** desteklenmez
- İstemci çevrimdışıyken Azure SignalR istemciye gönderilen iletileri artık yeniden oynamıyor
- Azure SignalR kullanırken, bir istemci bağlantısının trafiği her zaman yönlendirilir (aka. **yapışkan**) bağlantı süresince bir App Server örneğine

ASP.NET SignalR desteği uyumlulukla odaklanmıştır, bu nedenle ASP.NET Core SignalR 'tan gelen yeni özelliklerin bazıları desteklenmez. Örneğin, **MessagePack**, **streaming**, vb. yalnızca ASP.NET Core SignalR uygulamalarında kullanılabilir.

SignalR hizmeti, farklı hizmet modu için yapılandırılabilir: `Classic` / `Default` / `Serverles` s. Bu ASP.NET destede `Serverless` mod desteklenmez. Veri düzlemi REST API de desteklenmez.

## <a name="where-do-my-data-reside"></a>Verilerim nerede bulunur?

Azure SignalR hizmeti bir veri işlemcisi hizmeti olarak çalışıyor. Müşteri içeriğini depolamaz ve veri yerleşimi tasarım tarafından taahhüt edilir. Tanılama için Azure depolama gibi diğer Azure hizmetleriyle birlikte Azure SignalR hizmetini kullanıyorsanız, verileri Azure bölgelerinde nasıl tutabileceğiniz hakkında rehberlik için lütfen [buraya](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure/) bakın.
