---
title: Azure SignalR Hizmeti'ndeki mesajlar ve bağlantılar
description: Azure SignalR Hizmeti'nde mesajlar ve bağlantılarla ilgili temel kavramlara genel bakış.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: zhshang
ms.openlocfilehash: 5f6428231a3639738e8fb52e7dc3f2f2a3d2a26e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75392822"
---
# <a name="messages-and-connections-in-azure-signalr-service"></a>Azure SignalR Hizmeti'ndeki mesajlar ve bağlantılar

Azure SinyalR Hizmeti'nin faturalandırma modeli, bağlantı sayısına ve ileti sayısına bağlıdır. Bu makalede, iletilerin ve bağlantıların nasıl tanımlandığı ve faturalandırma için nasıl sayıldığı açıklanmaktadır.


## <a name="message-formats"></a>İleti biçimleri 

Azure SignalR Hizmeti, ASP.NET Core SignalR: [JSON](https://www.json.org/) ve [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)ile aynı biçimleri destekler.

## <a name="message-size"></a>İleti boyutu

Azure SignalR Hizmetinin iletiler için boyut sınırı yoktur.

Büyük iletiler, her biri en fazla 2 KB olan ve ayrı olarak iletilen daha küçük iletilere bölünür. SDK'lar ileti bölme ve birleştirme işlemlerini işler. Hiçbir geliştirici çabası gereklidir.

Büyük iletiler ileti performansını olumsuz etkiler. Mümkün olduğunda daha küçük iletiler kullanın ve her kullanım durumu senaryosu için en uygun ileti boyutunu belirlemek için sınama.

## <a name="how-messages-are-counted-for-billing"></a>İletiler faturalandırma için nasıl sayılır?

Faturalandırma için yalnızca Azure SinyalR Hizmeti'nden giden iletiler sayılır. İstemciler ve sunucular arasındaki ping iletileri yoksayılır.

2 KB'den büyük iletiler, her biri 2 KB'lik birden çok ileti olarak sayılır. Azure portalındaki ileti sayısı grafiği, hub başına her 100 iletide bir güncelleştirilir.

Örneğin, üç istemciniz ve bir uygulama sunucunuz olduğunu düşünün. Bir istemci, sunucunun tüm istemcilere yayın ını sağlamak için 4-KB iletisi gönderir. İleti sayısı sekizdir: hizmetten uygulama sunucusuna bir ileti ve hizmetten istemcilere üç ileti. Her ileti iki adet 2-KB ileti olarak sayılır.

## <a name="how-connections-are-counted"></a>Bağlantılar nasıl sayılır?

Azure SignalR Hizmeti ile sunucu bağlantıları ve istemci bağlantıları vardır. Varsayılan olarak, her uygulama sunucusu hub başına beş ilk bağlantıyla başlar ve her istemcinin bir istemci bağlantısı vardır.

Azure portalında gösterilen bağlantı sayısı hem sunucu bağlantılarını hem de istemci bağlantılarını içerir.

Örneğin, iki uygulama sunucunuz olduğunu ve kod içinde beş hub tanımladığınızı varsayalım. Sunucu bağlantı sayısı 50 olacaktır: 2 uygulama sunucusu * 5 hub * hub başına 5 bağlantı.

ASP.NET SignalR sunucu bağlantılarını farklı bir şekilde hesaplar. Tanımladığınız hub'lara ek olarak bir varsayılan hub içerir. Varsayılan olarak, her uygulama sunucusunun beş başlangıç sunucusu bağlantısına daha ihtiyacı vardır. Varsayılan hub'ın ilk bağlantı sayısı diğer hub'larınkiyle tutarlı kalır.

Uygulama sunucusunun ömrü boyunca, hizmet ve uygulama sunucusu eşitleme bağlantı durumunu korur ve daha iyi performans ve hizmet kararlılığı için sunucu bağlantılarında ayarlama yapar. Böylece zaman zaman sunucu bağlantı numarası değişiklikleri görebilirsiniz.

## <a name="how-inboundoutbound-traffic-is-counted"></a>Gelen/giden trafik nasıl sayılır?

Gelen trafik ile giden trafik arasındaki ayrım, Azure SinyalR Hizmeti'nin perspektifine bağlıdır. Trafik bayt cinsinden hesaplanır.

## <a name="related-resources"></a>İlgili kaynaklar

- [Azure Monitör'de toplama türleri](/azure/azure-monitor/platform/metrics-supported#microsoftsignalrservicesignalr )
- [ASP.NET Core SignalR yapılandırması](/aspnet/core/signalr/configuration)
- [Json](https://www.json.org/)
- [MessagePack](/aspnet/core/signalr/messagepackhubprotocol)
