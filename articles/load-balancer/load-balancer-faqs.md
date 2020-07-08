---
title: Sık sorulan sorular-Azure Load Balancer
description: Azure Load Balancer hakkında sık sorulan soruların yanıtları.
services: load-balancer
author: erichrt
ms.service: load-balancer
ms.topic: article
ms.date: 04/22/2020
ms.author: errobin
ms.openlocfilehash: 205a4bd119a7324c4e6524a0e29d432aa57bf315
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85848222"
---
# <a name="load-balancer-frequently-asked-questions"></a>Load Balancer sık sorulan sorular

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

## <a name="can-i-use-global-vnet-peering-with-basic-load-balancer"></a>Temel Load Balancer genel VNET eşlemesini kullanabilir miyim?
Hayır. Temel Load Balancer genel VNET eşlemesini desteklemez. Bunun yerine bir Standart Load Balancer kullanabilirsiniz. Sorunsuz yükseltme için [temel 'Ten standart sürümüne yükseltme](upgrade-basic-standard.md) makalesine bakın.

## <a name="how-can-i-discover-the-public-ip-that-an-azure-vm-uses"></a>Azure VM 'nin kullandığı genel IP 'yi nasıl keşfedebilirim?

Giden bir bağlantının genel kaynak IP adresini belirlemenin birçok yolu vardır. OpenDNS, sanal makinenizin genel IP adresini gösterebilmeniz için bir hizmet sağlar.
Nslookup komutunu kullanarak, myip.opendns.com adı için OpenDNS çözümleyiciye bir DNS sorgusu gönderebilirsiniz. Hizmet, sorguyu göndermek için kullanılan kaynak IP adresini döndürür. Aşağıdaki sorguyu sanal makinenizde çalıştırdığınızda, yanıt o VM için kullanılan genel IP 'dir:

 ```nslookup myip.opendns.com resolver1.opendns.com```

## <a name="how-do-connections-to-azure-storage-in-the-same-region-work"></a>Azure depolama ile aynı bölgedeki bağlantılar nasıl çalışır?
Yukarıdaki senaryolar aracılığıyla giden bağlantıların olması, VM ile aynı bölgedeki depolamaya bağlanmak için gerekli değildir. Bunu istemiyorsanız, yukarıda açıklandığı gibi ağ güvenlik gruplarını (NSG 'ler) kullanın. Diğer bölgelerdeki depolama bağlantısı için giden bağlantı gereklidir. Lütfen aynı bölgedeki bir VM 'den depolamaya bağlanırken, depolama tanılama günlüklerindeki kaynak IP adresinin, sanal makinenizin genel IP adresi değil, iç sağlayıcı adresi olacağını unutmayın. Depolama hesabınıza erişimi aynı bölgedeki bir veya daha fazla sanal ağ alt ağında bulunan VM 'Lerle kısıtlamak istiyorsanız, depolama hesabı güvenlik duvarını yapılandırırken ortak IP adresinizi değil [sanal ağ hizmet uç noktalarını](../virtual-network/virtual-network-service-endpoints-overview.md) kullanın. Hizmet uç noktaları yapılandırıldıktan sonra, sanal ağ özel IP adresinizi, iç sağlayıcı adresinde değil, depolama tanılama günlüklerinizin içinde görürsünüz.

## <a name="next-steps"></a>Sonraki Adımlar
Sorunuz yukarıdaki listede yoksa, bu sayfayla ilgili geri bildirimde bulunmak için lütfen sorularınızı gönderin. Bu, tüm değerli müşteri sorularımızın cevaplanmasını sağlamak için ürün ekibine yönelik bir GitHub sorunu oluşturur.
