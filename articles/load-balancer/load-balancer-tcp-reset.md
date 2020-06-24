---
title: Azure 'da boşta üzerinde TCP sıfırlaması Load Balancer
titleSuffix: Azure Load Balancer
description: Bu makalede, boşta kalma zaman aşımı durumunda çift yönlü TCP RST paketlerine sahip Azure Load Balancer hakkında bilgi edinin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2019
ms.author: allensu
ms.openlocfilehash: 68714053ac92faf8550a3e5f83a526afa1222971
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84808471"
---
# <a name="load-balancer-with-tcp-reset-on-idle"></a>Boşta durumunda TCP sıfırlaması ile Load Balancer

Belirli bir kural için boşta üzerinde TCP sıfırlamayı etkinleştirerek, senaryolarınız için daha öngörülebilir bir uygulama davranışı oluşturmak üzere [Standart Load Balancer](load-balancer-standard-overview.md) kullanabilirsiniz. Bir akışın boşta kalma zaman aşımı süresine ulaşıldığında Load Balancer varsayılan davranış sessizce bir şekilde düşürülemiyor.  Bu özelliği etkinleştirmek Load Balancer boşta kalma zaman aşımı durumunda çift yönlü TCP sıfırlama (TCP RST paketi) gönderilmesine neden olur.  Bu, uygulamanın uç noktalarını bağlantının zaman aşımına uğradığını ve artık kullanılamaz olduğunu bildirir.  Uç noktalar gerektiğinde hemen yeni bir bağlantı kurabilir.

![TCP sıfırlaması Load Balancer](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)
 
Bu varsayılan davranışı değiştirir ve gelen NAT kurallarında, Yük Dengeleme kurallarında ve [giden kurallarda](https://aka.ms/lboutboundrules)boş zaman AŞıMı durumunda TCP sıfırlamaları göndermeyi etkinleştirin.  Kural başına etkinleştirildiğinde Load Balancer, tüm eşleşen akışlar için boşta kalma zaman aşımı durumunda hem istemci hem de sunucu uç noktalarına çift yönlü TCP sıfırlaması (TCP RST paketleri) gönderir.

TCP RST paketleri alan uç noktalar ilgili yuvayı hemen kapatır. Bu, bağlantı sürümünün gerçekleştiği uç noktalara anında bildirim sağlar ve ileride aynı TCP bağlantısıyla iletişim kurmak başarısız olur.  Uygulamalar, TCP bağlantısının son zaman aşımına ermesi gerekmeden, bağlantı gerektiğinde bağlantıları temizler ve yeniden yeniden kuracak.

Birçok senaryo için bu, bir akışın boşta kalma zaman aşımını yenilemek üzere TCP (veya uygulama katmanı) ile keepcanlı olarak gönderme gereksinimini azaltabilir. 

Boşta kalma süreleri, yapılandırma tarafından izin verilen değerleri aşarsa veya uygulamanız TCP sıfırlamaları etkin hale getirdik bir davranış gösteriyorsa, TCP bağlantılarının bir listesini izlemek için yine de TCP keeplıbekleme (veya uygulama katmanı keepka) kullanmanız gerekebilir.  Ayrıca, bu, bağlantının yolda bir yerde proxy olduğu durumlarda, özellikle de uygulama katmanı keepcanlı olarak da yararlı olabilir.  

TCP sıfırlamayı etkinleştirme, boşta kalma zaman aşımını ayarlama ve istenen uygulama davranışını sağlamak için ek adımlar gerekip gerekmediği hakkında karar vermek üzere uçtan uca senaryonun tamamını dikkatle inceleyin.

## <a name="enabling-tcp-reset-on-idle-timeout"></a>Boşta kalma zaman aşımı durumunda TCP sıfırlamayı etkinleştirme

API sürüm 2018-07-01 ' yi kullanarak, boş zaman aşımı durumunda her kural temelinde çift yönlü TCP sıfırlamaları göndermeyi etkinleştirebilirsiniz:

```json
      "loadBalancingRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "inboundNatRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "outboundRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

## <a name="region-availability"></a><a name="regions"></a>Bölge kullanılabilirliği

Tüm bölgelerde kullanılabilir.

## <a name="limitations"></a>Sınırlamalar

- TCP RST yalnızca TCP bağlantısı sırasında belırlenen durumda gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Load Balancer](load-balancer-standard-overview.md)hakkında bilgi edinin.
- [Giden kuralları](load-balancer-outbound-rules-overview.md)hakkında bilgi edinin.
- [Boştaki zaman aşımında TCP RST 'yi yapılandırma](load-balancer-tcp-idle-timeout.md)
