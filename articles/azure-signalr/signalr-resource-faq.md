---
title: Azure SignalR hizmeti hakkında sık sorulan sorular
description: Sorun giderme ve tipik kullanım senaryoları dahil olmak üzere Azure SignalR hizmeti hakkında sık sorulan soruların yanıtlarını alın.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.custom: devx-track-dotnet
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: d5dd765dd9b174ffbfec35b63ad5e55ce84193ad
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89489570"
---
# <a name="azure-signalr-service-faq"></a>Azure SignalR hizmeti hakkında SSS

## <a name="is-azure-signalr-service-ready-for-production-use"></a>Azure SignalR hizmeti üretim kullanımı için hazırlanıyor mu?

Evet.
Genel kullanılabilirlik duyurusu için bkz. [Azure SignalR Service artık genel kullanıma sunuldu](https://azure.microsoft.com/blog/azure-signalr-service-now-generally-available/). 

[ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction) tam olarak desteklenir.

ASP.NET SignalR desteği hala *genel önizlemede*. [İşte bir kod örneği](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom).

## <a name="the-client-connection-closes-with-the-error-message-no-server-available-what-does-it-mean"></a>İstemci bağlantısı, "sunucu kullanılamıyor" hata iletisiyle kapanıyor. Ne anlama geliyor?

Bu hata yalnızca istemcilerin Azure SignalR hizmetine ileti göndermesi durumunda oluşur.

Herhangi bir uygulama sunucunuz yoksa ve yalnızca Azure SignalR hizmeti REST API kullanıyorsanız, bu davranış *tasarıma göre*yapılır.
Sunucusuz mimaride, istemci bağlantıları *dinleme* modundadır ve Azure SignalR hizmetine ileti göndermez.
[REST API hakkında daha fazla](./signalr-quickstart-rest-api.md)bilgi edinin.

Uygulama sunucularınız varsa, bu hata iletisi Azure SignalR hizmet örneğinize bağlı hiçbir uygulama sunucusu olmadığı anlamına gelir.

Olası nedenler şunlardır:
- Azure SignalR hizmeti ile hiçbir uygulama sunucusu bağlanmadı. Olası bağlantı hataları için uygulama sunucusu günlüklerine bakın. Bu durum, birden fazla uygulama sunucusuna sahip yüksek kullanılabilirliğe sahip bir ayarda nadir olarak belirlenir.
- Azure SignalR hizmet örneklerinde bağlantı sorunları var. Bu sorun geçicidir ve örnekler otomatik olarak kurtarılır.
Bir saatten uzun süre devam ederse, [GitHub 'da bir sorun açın](https://github.com/Azure/azure-signalr/issues/new) veya [Azure 'da bir destek isteği oluşturun](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request).

## <a name="when-there-are-multiple-application-servers-are-client-messages-sent-to-all-servers-or-just-one-of-them"></a>Birden çok uygulama sunucusu olduğunda, istemci iletileri tüm sunuculara mı, yoksa yalnızca biri mi gönderilir?

İstemci ve uygulama sunucusu arasında bire bir eşleme vardır. Bir istemciden gelen iletiler her zaman aynı uygulama sunucusuna gönderilir.

Eşleme, istemci veya uygulama sunucusu bağlantısı kesilene kadar korunur.

## <a name="if-one-of-my-application-servers-is-down-how-can-i-find-it-and-get-notified"></a>Uygulama sunucularım kapalıysa, nasıl öğrenebilirim ve bildirim alabilir?

Azure SignalR hizmeti, uygulama sunucularından sinyalleri izler.
Belirli bir süre için sinyaller alınmıyorsa, uygulama sunucusu çevrimdışı kabul edilir. Bu uygulama sunucusuyla eşlenen tüm istemci bağlantılarının bağlantısı kesilecek.

## <a name="why-does-my-custom-iuseridprovider-throw-an-exception-when-im-switching-from-aspnet-core-signalr-sdk-to-azure-signalr-service-sdk"></a>`IUserIdProvider`ASP.NET Core SIGNALR SDK 'Dan Azure SignalR hizmeti SDK 'sına geçiş yaparken özel bir özel durum oluşturur mi?

Parametresi, `HubConnectionContext context` çağrıldığında ASP.NET Core SIGNALR SDK ve Azure SignalR hizmeti SDK 'sı arasında farklıdır `IUserIdProvider` .

ASP.NET Core SignalR 'de, `HubConnectionContext context` fiziksel istemci bağlantısının tüm özellikler için geçerli değerlerle olan bağlamıdır.

Azure SignalR hizmeti SDK 'sında, `HubConnectionContext context` mantıksal istemci bağlantısının bağlamıdır. Fiziksel istemci Azure SignalR hizmeti örneğine bağlanır, bu nedenle yalnızca sınırlı sayıda özellik sağlanır.

Şimdilik yalnızca `HubConnectionContext.GetHttpContext()` ve `HubConnectionContext.User` erişim için kullanılabilir.
[Kaynak kodunu kontrol](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/HubHost/ServiceHubConnectionContext.cs)edebilirsiniz.

## <a name="can-i-configure-the-transports-available-in-azure-signalr-service-on-the-server-side-with-aspnet-core-signalr-for-example-can-i-disable-websocket-transport"></a>Sunucu tarafında Azure SignalR hizmetinde bulunan aktarımları ASP.NET Core SignalR ile yapılandırabilir miyim? Örneğin, WebSocket taşımasını devre dışı bırakabilir miyim?

Hayır.

Azure SignalR hizmeti, ASP.NET Core SignalR 'nin desteklediği üç aktarımı varsayılan olarak sağlar. Yapılandırılamaz. Azure SignalR hizmeti, tüm istemci bağlantıları için bağlantıları ve aktarımları işleyecek.

İstemci tarafı taşımalarını [ASP.NET Core SignalR yapılandırmasında](https://docs.microsoft.com/aspnet/core/signalr/configuration?view=aspnetcore-2.1&tabs=dotnet#configure-allowed-transports-2)belgelenen şekilde yapılandırabilirsiniz.

## <a name="what-is-the-meaning-of-metrics-like-message-count-or-connection-count-shown-in-the-azure-portal-which-kind-of-aggregation-type-should-i-choose"></a>İleti sayısı veya Azure portal gösterilen bağlantı sayısı gibi ölçümlerin anlamı nedir? Ne tür bir toplama türü seçmem gerekir?

Bu ölçümleri [Azure SignalR hizmetindeki iletilerde ve bağlantılarda](signalr-concept-messages-and-connections.md)hesapladığımız ayrıntıları bulabilirsiniz.

Azure SignalR hizmeti kaynaklarının genel bakış bölmesinde, sizin için uygun toplama türünü zaten seçtik. Ölçümler bölmesine giderseniz, toplama türünü [Azure SignalR hizmetindeki mesajlar ve bağlantılar](../azure-monitor/platform/metrics-supported.md#microsoftsignalrservicesignalr) ' a başvuru olarak alabilirsiniz.

## <a name="what-is-the-meaning-of-the-default-serverless-and-classic-service-modes-how-can-i-choose"></a>`Default`, `Serverless` Ve `Classic` hizmet modlarının anlamı nedir? Nasıl seçebilirim?

Modlar hakkında bilgi aşağıda verilmiştir:
* `Default` mod bir hub sunucusu *gerektiriyor* . Bu modda, Azure SignalR hizmeti istemci trafiğini bağlı hub sunucusu bağlantılarına yönlendirir. Azure SignalR hizmeti bağlı bir hub sunucusunu denetler. Hizmet bağlı bir hub sunucusu bulamazsa, gelen istemci bağlantılarını reddeder. Ayrıca, bağlantılı istemcileri doğrudan Azure SignalR hizmeti aracılığıyla yönetmek için bu modda *YÖNETIM API* 'sini de kullanabilirsiniz.
* `Serverless` mod hiçbir sunucu *bağlantısına izin vermiyor* . Diğer bir deyişle, tüm sunucu bağlantılarını reddeder. Tüm istemciler sunucusuz modda olmalıdır. İstemciler Azure SignalR hizmetine bağlanır ve kullanıcılar, genellikle Merkez LOGI 'yi işlemek için *Azure işlevleri* gibi sunucusuz teknolojiler kullanır. Azure SignalR hizmetinde sunucusuz modu kullanan [basit bir örneğe bakın](https://docs.microsoft.com/azure/azure-signalr/signalr-quickstart-azure-functions-javascript?WT.mc_id=signalrquickstart-github-antchu) .
* `Classic` mod, karışık bir durum. Bir hub 'ın sunucu bağlantısı olduğunda, yeni istemci bir hub sunucusuna yönlendirilir. Aksi takdirde, istemci sunucusuz moda girer. 

  Bu bir soruna neden olabilir. Örneğin, tüm sunucu bağlantıları bir süre için kaybedilmişse, bazı istemciler bir hub sunucusuna yönlendirmek yerine sunucusuz moda girer.

Aşağıda mod seçme yönergeleri verilmiştir:
- Hub sunucusu yoksa, öğesini seçin `Serverless` .
- Tüm Hub 'lar hub sunucularına sahip ise, öğesini seçin `Default` .
- Bazı hub 'larda hub sunucusu varsa ancak diğerleri yoksa, `Classic` bunu seçebilirsiniz, ancak bu sorun bir soruna neden olabilir. Daha iyi bir yol, iki örnek oluşturmaktır: bir, `Serverless` ve diğeri `Default` .

## <a name="are-there-any-feature-differences-in-using-azure-signalr-service-with-aspnet-signalr"></a>ASP.NET SignalR ile Azure SignalR hizmetini kullanırken herhangi bir özellik farkı var mı?
Azure SignalR hizmetini kullanırken, ASP.NET SignalR 'nin bazı API 'Leri ve özellikleri desteklenmez:
- İstemciler ve merkez arasında rastgele durum geçirme özelliği (genellikle denir `HubState` ) desteklenmez.
- `PersistentConnection`Sınıf desteklenmiyor.
- *Süresiz çerçeve taşıması* desteklenmez.
- Azure SignalR hizmeti, istemci çevrimdışıyken istemciye gönderilen iletileri artık yeniden oynamıyor.
- Azure SignalR hizmetini kullanırken, bir istemci bağlantısının trafiği bağlantı süresince tek bir App Server örneğine her zaman yönlendirilir ( *yapışkan*olarak da adlandırılır).

ASP.NET SignalR desteği uyumlulukla odaklanmıştır, bu nedenle ASP.NET Core SignalR 'tan gelen yeni özelliklerin bazıları desteklenmez. Örneğin, *MessagePack* ve *akış* yalnızca ASP.NET Core SignalR uygulamaları için kullanılabilir.

Azure SignalR hizmetini farklı hizmet modları için yapılandırabilirsiniz: `Classic` , `Default` , ve `Serverless` . `Serverless`Mod, ASP.NET için desteklenmiyor. Veri düzlemi REST API de desteklenmez.

## <a name="where-does-my-data-reside"></a>Verilerim nerede bulunur?

Azure SignalR hizmeti bir veri işlemcisi hizmeti olarak çalışmaktadır. Hiçbir müşteri içeriğini depolamaz ve veri yerleşimi tasarım tarafından dahil edilir. Tanılama için Azure depolama gibi diğer Azure hizmetleriyle birlikte Azure SignalR hizmetini kullanıyorsanız, verilerin Azure bölgelerinde nasıl tutulabileceğine ilişkin yönergeler için [Bu teknik incelemeye](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure/) bakın.
