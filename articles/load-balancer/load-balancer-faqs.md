---
title: Sık sorulan sorular-Azure Load Balancer
description: Azure Load Balancer hakkında sık sorulan soruların yanıtları.
services: load-balancer
author: erichrt
ms.service: load-balancer
ms.topic: article
ms.date: 04/22/2020
ms.author: errobin
ms.openlocfilehash: 4ae15d0898cedb0ed17dc308584769395aa819c2
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85079475"
---
# <a name="frequently-asked-questions"></a>Sık sorulan sorular

## <a name="what-types-of-load-balancer-exist"></a>Ne tür Load Balancer var?
Bir sanal ağ ve dış yük dengeleyiciler içindeki trafiği dengelemeye yönelik iç yük dengeleyiciler, internet 'e bağlı bir uç noktadan gelen ve giden trafiği dengelemek Daha fazla bilgi için bkz. [Load Balancer türleri](components.md#frontend-ip-configurations). 

Her iki tür için de Azure, farklı işlevsel, performans, güvenlik ve sistem durumu izleme özelliklerine sahip temel bir SKU ve standart SKU sunmaktadır. Bu farklılıklar, [SKU karşılaştırma](skus.md) makalemiz bölümünde açıklanmaktadır.

 ## <a name="how-can-i-upgrade-from-a-basic-to-a-standard-load-balancer"></a>Bir temel bilgisayardan bir Standart Load Balancer nasıl yükseltebilirim?
Bir Load Balancer SKU yükseltmesi hakkında otomatik betik ve yönergeler için [temel ve standart sürümüne yükseltme](upgrade-basic-standard.md) makalesine bakın.

 ## <a name="what-are-the-different-load-balancing-options-in-azure"></a>Azure 'daki farklı yük dengeleme seçenekleri nelerdir?
Kullanılabilir Yük Dengeleme Hizmetleri ve her biri için önerilen kullanımlar için [yük dengeleyici teknoloji kılavuzuna](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview) bakın.

## <a name="where-can-i-find-load-balancer-arm-templates"></a>Load Balancer ARM şablonlarını nerede bulabilirim?
Ortak dağıtımların ARM şablonları için [Azure Load Balancer hızlı başlangıç şablonları listesine](https://docs.microsoft.com/azure/templates/microsoft.network/loadbalancers#quickstart-templates) bakın.

## <a name="how-are-inbound-nat-rules-different-from-load-balancing-rules"></a>Gelen NAT kuralları Yük Dengeleme kurallarından farklı mı?
NAT kuralları, trafiğin yönlendirileceği bir arka uç kaynağını belirtmek için kullanılır. Örneğin, belirli bir VM 'ye RDP trafiği göndermek için belirli bir yük dengeleyici bağlantı noktası yapılandırma. Yük Dengeleme kuralları, trafiği yönlendirmek için bir arka uç kaynakları havuzunu belirtmek için kullanılır ve her bir örnekteki yükün yükünü dengeleyin. Örneğin, bir yük dengeleyici kuralı, TCP paketlerini bir Web sunucuları havuzunda yük dengeleyicinin 80 numaralı bağlantı noktasında yönlendirebilir.

## <a name="what-is-ip-1686312916"></a>IP 168.63.129.16 nedir?
Azure altyapısı olarak etiketlenen konağın sanal IP adresi, Azure sistem durumu araştırmalarının gerçekleştiği Load Balancer. Arka uç örnekleri yapılandırılırken, bu IP adresinden gelen trafiğin sistem durumu araştırmasına başarıyla yanıt vermesi gerekir. Bu kural Load Balancer ön uç 'nize erişimle etkileşime girmiyor. Azure Load Balancer kullanmıyorsanız, bu kuralı geçersiz kılabilirsiniz. [Burada](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags)Service Tags hakkında daha fazla bilgi edinebilirsiniz.

## <a name="can-i-use-global-vnet-peering-with-basic-load-blancer"></a>Temel yük Blancer ile küresel VNET eşlemesini kullanabilir miyim?
Hayır. Temel Load Balancer genel VNET eşlemesini desteklemez. Bunun yerine bir Standart Load Balancer kullanabilirsiniz. Sorunsuz yükseltme için [temel 'Ten standart sürümüne yükseltme](upgrade-basic-standard.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki Adımlar
Sorunuz yukarıdaki listede yoksa, bu sayfayla ilgili geri bildirimde bulunmak için lütfen sorularınızı gönderin. Bu, tüm değerli müşteri sorularımızın cevaplanmasını sağlamak için ürün ekibine yönelik bir GitHub sorunu oluşturur.
