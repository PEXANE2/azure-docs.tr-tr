---
title: Azure SignalR Hizmeti sık sorulan sorular
description: Azure Sinyal Hizmeti'nde sorun giderme ve tipik kullanım senaryoları hakkında sık sorulan sorulara hızlı erişim edin.
author: sffamily
ms.service: signalr
ms.topic: overview
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: dde11b6097dddb1568f5adfea811606214a9759e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75891261"
---
# <a name="azure-signalr-service-faq"></a>Azure SignalR Hizmeti SSS

## <a name="is-azure-signalr-service-ready-for-production-use"></a>Azure SignalR Hizmeti üretim eki kullanıma hazır mı?

Evet.
Genel kullanılabilirlik duyurumuz için [Azure SignalR Hizmeti'ne](https://azure.microsoft.com/blog/azure-signalr-service-now-generally-available/)bakın. 

[ASP.NET Core SignalR](https://docs.microsoft.com/aspnet/core/signalr/introduction) tam olarak desteklenir.

SignalR ASP.NET desteği hala *genel önizlemede.* Burada bir [kod örneği.](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom)

## <a name="the-client-connection-closes-with-the-error-message-no-server-available-what-does-it-mean"></a>İstemci bağlantısı "Sunucu yok" hata iletisiyle kapanır. Bu ne anlama geliyor?

Bu hata yalnızca istemciler SignalR Hizmetine ileti gönderirken oluşur.

Herhangi bir uygulama sunucusu yoksa ve yalnızca SignalR Service REST API kullanıyorsanız, bu davranış **tasarım gereğidir.**
Sunucusuz mimaride istemci bağlantıları **LISTEN** modundadır ve SignalR Hizmeti'ne ileti göndermez.
[REST API](./signalr-quickstart-rest-api.md)hakkında daha fazla bilgi edinin.

Uygulama sunucularınız varsa, bu hata iletisi SinyalR Hizmeti örneğinize hiçbir uygulama sunucusunun bağlı olmadığı anlamına gelir.

Olası nedenleri şunlardır:
- SignalR Hizmeti ile hiçbir uygulama sunucusu bağlı değildir. Olası bağlantı hataları için uygulama sunucusu günlüklerini denetleyin. Bu durumda birden fazla uygulama sunucusu ile yüksek kullanılabilirlik ayarında nadirdir.
- SignalR Service örnekleriile bağlantı sorunları vardır. Bu sorun geçicidir ve otomatik olarak kurtarılır.
Bir saatten uzun süre devam ederse, [GitHub'da bir sorun açın](https://github.com/Azure/azure-signalr/issues/new) veya [Azure'da bir destek isteği oluşturun.](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

## <a name="when-there-are-multiple-application-servers-are-client-messages-sent-to-all-servers-or-just-one-of-them"></a>Birden çok uygulama sunucusu olduğunda, istemci iletileri tüm sunuculara mı yoksa yalnızca bir sunucuya mı gönderilir?

İstemci ve uygulama sunucusu arasında bire bir eşlemedir. Bir istemciden gelen iletiler her zaman aynı uygulama sunucusuna gönderilir.

İstemci ve uygulama sunucusu arasındaki eşleme, istemci veya uygulama sunucusu bağlantı kesilene kadar korunur.

## <a name="if-one-of-my-application-servers-is-down-how-can-i-find-it-and-get-notified"></a>Uygulama sunucularımdan biri çalışmıyorsa, nasıl bulabilirim ve bilgilendirilebilir miyim?

SignalR Service, uygulama sunucularından sinyalatlarını izler.
Sinyaller belirli bir süre için alınmazsa, uygulama sunucusu çevrimdışı olarak kabul edilir. Bu uygulama sunucusuna eşlenen tüm istemci bağlantıları nın bağlantısı kesilir.

## <a name="why-does-my-custom-iuseridprovider-throw-exception-when-switching-from-aspnet-core-signalr--sdk-to-azure-signalr-service-sdk"></a>ASP.NET Core `IUserIdProvider` SignalR SDK'dan Azure SignalR Service SDK'ya geçerken özel atma istisnam neden geçerli?

Parametre, `HubConnectionContext context` çağrıldığında `IUserIdProvider` ASP.NET Core SignalR SDK ile Azure SignalR Service SDK arasında farklıdır.

Core SignalR ASP.NET, `HubConnectionContext context` tüm özellikler için geçerli değerler ile fiziksel istemci bağlantısından bağlamdır.

Azure SignalR Hizmeti SDK, `HubConnectionContext context` mantıksal istemci bağlantısından bağlamdır. Fiziksel istemci bağlantısı SignalR Hizmeti örneğine bağlanır, bu nedenle yalnızca sınırlı sayıda özellik sağlanır.

Şimdilik, sadece `HubConnectionContext.GetHttpContext()` `HubConnectionContext.User` ve erişim için kullanılabilir.
Kaynak kodunu [buradan](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/HubHost/ServiceHubConnectionContext.cs)kontrol edebilirsiniz.

## <a name="can-i-configure-the-transports-available-in-signalr-service-as-configuring-it-on-server-side-with-aspnet-core-signalr-for-example-disable-websocket-transport"></a>SignalR Hizmeti'nde bulunan taşımaları sunucu tarafında core signalr ile ASP.NET olarak yapılandırabilir miyim? Örneğin, WebSocket aktarımDevre dışı?

Hayır.

Azure SignalR Hizmeti, Core SignalR'ASP.NET varsayılan olarak desteklediği üç aktarım sağlar. Yapılandırılamaz. SignalR Hizmeti, tüm istemci bağlantıları için bağlantıları ve taşımaları işleyecek.

İstemci tarafı aktarımlarını [burada](https://docs.microsoft.com/aspnet/core/signalr/configuration?view=aspnetcore-2.1&tabs=dotnet#configure-allowed-transports-2)belgelenmiş şekilde yapılandırabilirsiniz.
