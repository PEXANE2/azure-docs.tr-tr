---
title: Azure SignalR hizmetindeki mesajlar ve bağlantılar
description: Azure SignalR hizmetindeki iletiler ve bağlantılarla ilgili temel kavramlara genel bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 5f6428231a3639738e8fb52e7dc3f2f2a3d2a26e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75392822"
---
# <a name="messages-and-connections-in-azure-signalr-service"></a>Azure SignalR hizmetindeki mesajlar ve bağlantılar

Azure SignalR hizmeti için faturalandırma modeli, bağlantı sayısını ve ileti sayısını temel alır. Bu makalede, iletilerin ve bağlantıların faturalandırma için nasıl tanımlandığı ve sayılacağı açıklanmaktadır.


## <a name="message-formats"></a>İleti biçimleri 

Azure SignalR hizmeti ASP.NET Core SignalR: [JSON](https://www.json.org/) ve [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)ile aynı biçimleri destekler.

## <a name="message-size"></a>İleti boyutu

Azure SignalR hizmetinin iletiler için boyut sınırı yoktur.

Büyük iletiler 2 KB 'tan fazla olmayan ve ayrı olarak aktarılan daha küçük iletilere ayrılır. SDK 'lar ileti bölme ve montaj işlemesi. Geliştirici çabalarına gerek yoktur.

Büyük iletiler, mesajlaşma performansını olumsuz etkiler. Mümkün olan her durumda daha küçük iletileri kullanın ve her bir kullanım örneği senaryosu için en iyi ileti boyutunu belirleme sınamasını yapın.

## <a name="how-messages-are-counted-for-billing"></a>Faturalandırmayla ilgili mesajlar nasıl sayılır?

Faturalandırma için yalnızca Azure SignalR hizmetinden gelen giden iletiler sayılır. İstemciler ve sunucular arasındaki ping iletileri yok sayılır.

2 KB 'den büyük mesajlar her biri 2 KB 'lık birden fazla ileti olarak sayılır. Azure portal ileti sayısı grafiği her Hub için her 100 ileti için güncelleştirilir.

Örneğin, üç istemciniz ve bir uygulama sunucunuz olduğunu varsayalım. Bir istemci, sunucunun tüm istemcilere yayınlamasına izin vermek için 4 KB 'lik bir ileti gönderir. İleti sayısı sekiz ' dır: hizmetten uygulama sunucusuna bir ileti ve hizmetten istemcilere yapılan üç ileti. Her ileti iki adet 2 KB 'lik ileti olarak sayılır.

## <a name="how-connections-are-counted"></a>Bağlantılar nasıl sayılır?

Azure SignalR hizmeti ile sunucu bağlantıları ve istemci bağlantıları vardır. Varsayılan olarak, her bir uygulama sunucusu hub başına beş ilk bağlantı ile başlar ve her istemcide bir istemci bağlantısı vardır.

Azure portal gösterilen bağlantı sayısı hem sunucu bağlantılarını hem de istemci bağlantılarını içerir.

Örneğin, iki uygulama sunucunuz olduğunu ve kodda beş hub tanımladığınızı varsayalım. Sunucu bağlantı sayısı 50 olacaktır: 2 App Servers * 5 hub *, hub başına 5 bağlantı.

ASP.NET SignalR sunucu bağlantılarını farklı bir şekilde hesaplar. Bu, tanımladığınız hublara ek olarak bir varsayılan Hub içerir. Varsayılan olarak, her uygulama sunucusunun beş daha fazla ilk sunucu bağlantısı olması gerekir. Varsayılan Hub için ilk bağlantı sayısı, diğer hub 'larla tutarlı kalır.

Uygulama sunucusunun kullanım ömrü boyunca, hizmet ve uygulama sunucusu eşitleme bağlantısı durumunu tutar ve daha iyi performans ve hizmet kararlılığı için sunucu bağlantılarında ayarlama yapar. Bu nedenle sunucu bağlantı numarası değişikliğini zaman zaman görebilirsiniz.

## <a name="how-inboundoutbound-traffic-is-counted"></a>Gelen/giden trafik nasıl sayılır?

Gelen trafik ve giden trafik arasındaki ayrım, Azure SignalR hizmeti perspektifine dayanır. Trafik bayt cinsinden hesaplanır.

## <a name="related-resources"></a>İlgili kaynaklar

- [Azure Izleyici 'de toplama türleri](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [ASP.NET Core SignalR yapılandırması](/aspnet/core/signalr/configuration)
- [JSON](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)
