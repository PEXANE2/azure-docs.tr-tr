---
title: Sık sorulan sorular-Azure Load Balancer
description: Azure Load Balancer hakkında sık sorulan soruların yanıtları.
services: load-balancer
author: erichrt
ms.service: load-balancer
ms.topic: article
ms.date: 04/22/2020
ms.author: errobin
ms.openlocfilehash: 3648bb102d539156067cdc259c1cf9b6e73d6a49
ms.sourcegitcommit: d815163a1359f0df6ebfbfe985566d4951e38135
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2020
ms.locfileid: "82884494"
---
# <a name="frequently-asked-questions"></a>Sık sorulan sorular

## <a name="what-types-of-load-balancer-exist"></a>Ne tür Load Balancer var?
Bir sanal ağ ve dış yük dengeleyiciler içindeki trafiği dengelemeye yönelik iç yük dengeleyiciler, internet 'e bağlı bir uç noktadan gelen ve giden trafiği dengelemek Daha fazla bilgi için bkz. [Load Balancer Types]https://docs.microsoft.com/azure/load-balancer/concepts-limitations#load-balancer-types)(. 

Her iki tür için de Azure, farklı işlevsel, performans, güvenlik ve sistem durumu izleme özelliklerine sahip temel bir SKU ve standart SKU sunmaktadır. Bu farklılıklar [SKU karşılaştırma] (https://docs.microsoft.com/azure/load-balancer/concepts-limitations#skus) makalemiz) bölümünde açıklanmaktadır.

 ## <a name="how-can-i-upgrade-from-a-basic-to-a-standard-load-balancer"></a>Bir temel bilgisayardan bir Standart Load Balancer nasıl yükseltebilirim?
Bir Load Balancer SKU yükseltmesi hakkında otomatik betik ve yönergeler için [temel ve standart sürümüne yükseltme](upgrade-basic-standard.md) makalesine bakın.

 ## <a name="what-are-the-different-load-balancing-options-in-azure"></a>Azure 'daki farklı yük dengeleme seçenekleri nelerdir?
Kullanılabilir Yük Dengeleme Hizmetleri ve her biri için önerilen kullanımlar için [yük dengeleyici teknoloji kılavuzuna](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) bakın.

## <a name="where-can-i-find-load-balancer-arm-templates"></a>Load Balancer ARM şablonlarını nerede bulabilirim?
Ortak dağıtımların ARM şablonları için [Azure Load Balancer hızlı başlangıç şablonları listesine](https://docs.microsoft.com/azure/templates/microsoft.network/loadbalancers#quickstart-templates) bakın.

## <a name="how-are-inbound-nat-rules-different-from-load-balancing-rules"></a>Gelen NAT kuralları Yük Dengeleme kurallarından farklı mı?
NAT kuralları, trafiğin yönlendirileceği bir arka uç kaynağını belirtmek için kullanılır. Örneğin, belirli bir VM 'ye RDP trafiği göndermek için belirli bir yük dengeleyici bağlantı noktası yapılandırma. Yük Dengeleme kuralları, trafiği yönlendirmek için bir arka uç kaynakları havuzunu belirtmek için kullanılır ve her bir örnekteki yükün yükünü dengeleyin. Örneğin, bir yük dengeleyici kuralı, TCP paketlerini bir Web sunucuları havuzunda yük dengeleyicinin 80 numaralı bağlantı noktasında yönlendirebilir.

## <a name="next-steps"></a>Sonraki Adımlar
Sorunuz yukarıdaki listede yoksa, bu sayfayla ilgili geri bildirimde bulunmak için lütfen sorularınızı gönderin. Bu, tüm değerli müşteri sorularımızın cevaplanmasını sağlamak için ürün ekibine yönelik bir GitHub sorunu oluşturur.
