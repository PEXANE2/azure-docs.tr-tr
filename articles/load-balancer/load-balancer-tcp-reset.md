---
title: Azure'da Boşta Bakiyesi TCP Sıfırlama
titleSuffix: Azure Load Balancer
description: Bu makalede, boşta zaman diliminde çift yönlü TCP RST paketleri ne olacaksa Azure Yük Dengeleyicisi hakkında bilgi edinin.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2019
ms.author: allensu
ms.openlocfilehash: d3d836ddea8d07a25ad09e6f19d9f17a680decd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294395"
---
# <a name="load-balancer-with-tcp-reset-on-idle"></a>Boşta TCP Sıfırlama ile Yük Dengeleyici

Belirli bir kural için Boşta TCP Reset'i etkinleştirerek senaryolarınız için daha öngörülebilir bir uygulama davranışı oluşturmak için [Standart Yük Dengeleyicisini](load-balancer-standard-overview.md) kullanabilirsiniz. Yük Dengeleyici'nin varsayılan davranışı, bir akışın boşta zaman akıtılmasına ulaşıldığında akışları sessizce düşürmektir.  Bu özelliği etkinleştirmek, Yük Dengeleyici'nin boşta zaman acısı üzerine çift yönlü TCP Sıfırlamaları (TCP RST paketi) göndermesine neden olur.  Bu, uygulama bitiş noktalarınıza bağlantının zaman aşımına geldiğini ve artık kullanılabilir olmadığını bildirir.  Uç noktalar gerekirse hemen yeni bir bağlantı kurabilir.

![Yük Dengeleyici TCP sıfırlama](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)
 
Bu varsayılan davranışı değiştirir ve Gelen NAT kurallarında, yük dengeleme kurallarında ve [giden kurallarda](https://aka.ms/lboutboundrules)tcp sıfırlamalarını boşta zaman adabına göndermeyi etkinleştirin.  Kural başına etkinleştirildiğinde, Yük Dengeleyicisi, tüm eşleşen akışlar için boşta kalma zaman sonu sırasında hem istemci hem de sunucu uç noktalarına çift yönlü TCP Sıfırlama (TCP RST paketleri) gönderir.

TCP RST paketlerini alan uç noktalar ilgili soketi hemen kapatır. Bu, bağlantının yayımlandığını ve aynı TCP bağlantısındaki gelecekteki iletişimin başarısız olacağını uç noktalara anında bildirir.  Uygulamalar, soket kapandığında bağlantıları temizleyebilir ve TCP bağlantısının sonunda zaman dolmasını beklemeden bağlantıları gerektiği gibi yeniden kurabilir.

Birçok senaryo için bu, bir akışın boşta zaman anına yenilemek için TCP (veya uygulama katmanı) keepalives gönderme gereksinimini azaltabilir. 

Boşta kalan süreleriniz yapılandırmanın izin verdiği süreleri aşıyorsa veya uygulamanız TCP Sıfırlamaları etkinleştirilmişolan istenmeyen bir davranış gösteriyorsa, TCP bağlantılarının canlılığını izlemek için Yine de TCP keepalives (veya uygulama katmanı tutma) kullanmanız gerekebilir.  Ayrıca, bağlantı yolda bir yere yakın olduğunda, özellikle uygulama katmanı keepalives için de yararlı kalabilir.  

TCP Resets'i etkinleştirme, boşta zaman ayarı ayarlama ve istenen uygulama davranışını sağlamak için ek adımlar gerekip gerekmediğine karar vermek için tüm uçtan bitiş senaryosunu dikkatle inceleyin.

## <a name="enabling-tcp-reset-on-idle-timeout"></a>Boşta zaman anında TCP Sıfırlama'yı etkinleştirme

API sürüm 2018-07-01'i kullanarak, kural bazında boşta zaman diliminde çift yönlü TCP Sıfırlamalarının gönderilmesini etkinleştirebilirsiniz:

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

Tüm bölgelerde mevcuttur.

## <a name="limitations"></a>Sınırlamalar

- TCP RST yalnızca TCP bağlantısı sırasında ESTABLISHED durumunda gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

- Standart [Yük Dengeleyicisi](load-balancer-standard-overview.md)hakkında bilgi edinin.
- Giden [kurallar](load-balancer-outbound-rules-overview.md)hakkında bilgi edinin.
- [TCP RST'yi Boşta Zaman Aşımda Yapılandır](load-balancer-tcp-idle-timeout.md)
