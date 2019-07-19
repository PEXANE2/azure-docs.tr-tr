---
title: Azure sanal ağ için IPv6 'ya genel bakış (Önizleme)
titlesuffix: Azure Virtual Network
description: Bir Azure sanal ağındaki IPv6 uç noktaları ve veri yollarının IPv6 açıklaması.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.workload: infrastructure-services
ms.date: 07/15/2019
ms.author: kumud
ms.openlocfilehash: 5093b74484cd04a0c0c7afed8e2ebc725af033f5
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249827"
---
# <a name="what-is-ipv6-for-azure-virtual-network-preview"></a>Azure sanal ağ için IPv6 nedir? (Önizleme)

Azure sanal ağ (VNet) için IPv6, Azure 'daki uygulamaları hem bir sanal ağ hem de Internet üzerinden IPv6 ve IPv4 bağlantısıyla barındırmanızı sağlar. Genel IPv4 adreslerinin tükenmesi nedeniyle, yeni taşınabilirlik ve Nesnelerin İnterneti (IoT) ağları genellikle IPv6 üzerinde oluşturulmuştur. Uzun süredir oluşturulan ISS ve mobil ağlar IPv6 'ya dönüştürüledir. Yalnızca IPv4 Hizmetleri, hem mevcut hem de gelişen pazarlarda kendilerini gerçek bir dezavantajda bulabilir. Çift yığın IPv4/IPv6 bağlantısı, Azure 'da barındırılan hizmetlerin, hem mevcut IPv4 hem de bu yeni IPv6 cihaz ve ağlarla birlikte erişilebilen, genel olarak kullanılabilen, çift yığılmış hizmetler ile bu teknoloji eksikliğini çapraz gezmesine olanak sağlar.

Azure 'un özgün IPv6 bağlantısı, Azure 'da barındırılan uygulamalar için çift yığın (IPv4/IPv6) Internet bağlantısı sağlamayı kolaylaştırır. Hem gelen hem de giden başlatılan bağlantılar için yük dengeli IPv6 bağlantısı olan VM 'lerin basit dağıtımına izin verir. Bu özellik hala kullanılabilir ve [burada](../load-balancer/load-balancer-ipv6-overview.md)daha fazla bilgi bulunabilir.
Azure sanal ağ için IPv6, tam IPv6 çözümü mimarilerinin Azure 'da dağıtılmasını sağlayan çok daha tam özellikli bir çözümdür.

> [!Important]
> Azure sanal ağ için IPv6 Şu anda genel önizleme aşamasındadır. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Aşağıdaki diyagramda, Azure 'da basit bir çift yığın (IPv4/IPv6) dağıtımı gösterilmektedir:

![IPv6 ağ dağıtım diyagramı](./media/ipv6-support-overview/ipv6-sample-diagram.png)

## <a name="benefits"></a>Avantajlar

Azure sanal ağ IPv6 avantajları:

- Azure 'da barındırılan uygulamalarınızın, büyüyen mobil ve Nesnelerin İnterneti pazarlara ulaşma durumunu genişletmenize yardımcı olur.
- Çift yığılmış IPv4/IPv6 VM 'Leri, en yüksek hizmet dağıtımı esnekliği sağlar. Tek bir hizmet örneği, hem IPv4 hem de IPv6 özellikli Internet istemcileriyle bağlanabilir.
- Uzun süreli, kararlı Azure VM 'den Internet 'e IPv6 bağlantısı üzerinde oluşturulur.
- Internet 'e yönelik IPv6 bağlantısı yalnızca dağıtımınızda açıkça istekte bulunduğunda, varsayılan olarak güvenli hale gelir.

## <a name="capabilities"></a>Özellikler

VNet için IPv6 aşağıdaki özellikleri içerir:

- Azure müşterileri kendi kendi IPv6 sanal ağ adresi alanını, uygulamalarının gereksinimlerini karşılamak için tanımlayabilir veya şirket içi IP alanı ile sorunsuz bir şekilde tümleşebilir.
- İkili yığın (IPv4 ve IPv6) sanal ağları çift yığın alt ağları ile, uygulamaların sanal ağlarında veya Internet 'te hem IPv4 hem de IPv6 kaynaklarıyla bağlanmasını sağlar.
- Kaynaklarınızı ağ güvenlik grupları için IPv6 kurallarıyla koruyun
- Kullanıcı tanımlı yollarla sanal ağınızdaki IPv6 trafiğinin yönlendirilmesini, özellikle de Uygulamanızı artırmak için ağ sanal gereçlerini kullanarak özelleştirin.
- Internet istemcilerinin, IPv6 (AAAA) kayıtları için Azure DNS desteği ile seçim protokolünü kullanarak çift yığın uygulamanıza sorunsuz bir şekilde erişmesini sağlayın. 
- Standart IPv6 genel Load Balancer aşağıdakiler de dahil olmak üzere dayanıklı, ölçeklenebilir uygulamalar oluşturmak için desteklenir:
    - Hangi arka uç havuzu örneklerinin sistem durumu olduğunu ve bu nedenle yeni akışlar alabileceğini belirleyen isteğe bağlı IPv6 sistem durumu araştırması. .  
    - Bu özelliği özel gereksinimlerinize göre ölçeklendirmeye ve ayarlamaya yönelik giden bağlantı üzerinde tam bildirime dayalı denetim sağlayan isteğe bağlı giden kurallar.
    - Tek bir yük dengeleyicinin birden çok IPv6 genel IP adresi kullanmasını sağlayan isteğe bağlı birden çok ön uç yapılandırması-aynı ön uç Protokolü ve bağlantı noktası ön uç adresleri arasında yeniden kullanılabilir.
- Örnek düzeyi genel IP, tek tek VM 'lere doğrudan IPv6 Internet bağlantısı sağlar.
- Yalnızca yükseltme ile mevcut olan IPv4 dağıtımlarına kolayca IPv6 bağlantısı ekleyin.
- Sanal makine ölçek kümeleri ile yüklemeniz için otomatik olarak ölçeklendirilen ikili yığın uygulamaları oluşturun.
- Önizleme için portal desteği çift yığın (IPv4 + IPv6) sanal ağları ve alt ağları, IPv6 ağ güvenlik grubu kurallarını, IPv6 Kullanıcı tanımlı rotaları ve IPv6 genel IP 'lerini etkileşimli olarak oluşturma/düzenleme/silme içerir.  

## <a name="limitations"></a>Sınırlamalar
Azure sanal ağ için IPv6 önizleme sürümü aşağıdaki sınırlamalara sahiptir:
- Azure sanal ağ için IPv6 (Önizleme) tüm genel Azure bölgelerinde bulunur, ancak yalnızca küresel Azure 'da kamu bulutları değildir.
- Standart Load Balancer bileşenleri için portal desteği yalnızca görüntüleme amaçlıdır.  Ancak, Azure PowerShell ve komut satırı arabirimi (CLı) kullanarak Standart Load Balancer dağıtımlar için tam destek ve belgeler (örneklerle birlikte) kullanılabilir.   
- Önizleme için ağ Izleyicisi desteği NSG akış günlükleri ve ağ paketi yakalamaları ile sınırlıdır.
- Sanal ağ eşlemesi (bölgesel veya genel) önizlemede desteklenmez.
- Standart IPv6 dış Load Balancer kullanırken aşağıdaki sınırlar geçerlidir: 
  - Giden kuralları birden çok ön uç genel IP 'ye başvurabilir, ancak bir IPv6 genel ön **ekine başvuramaz.** IP genel ön eki yalnızca IPv4 öneklerini destekler.
  - IPv6 Yük Dengeleme kuralları, *kayan IP* **özelliğini kullanmayabilir** . Arka uç örneklerinde bağlantı noktası yeniden kullanımı yalnızca IPv4 ile desteklenir.
- Internet 'e yönelik bir IPv6 adresi bloğunu ayırma, Azure genel IP adresi öneki özelliği tarafından desteklenmez.

## <a name="pricing"></a>Fiyatlandırma

IPv6 Azure kaynakları ve bant genişliği, IPv4 ile aynı ücretler üzerinden ücretlendirilir. IPv6 için ek veya farklı ücretler yoktur. [Genel IP adresleri](https://azure.microsoft.com/pricing/details/ip-addresses/), [ağ bant genişliği](https://azure.microsoft.com/pricing/details/bandwidth/)veya [Load Balancer](https://azure.microsoft.com/pricing/details/load-balancer/)fiyatlandırmasıyla ilgili ayrıntıları bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure PowerShell kullanarak bir IPv6 çift yığın uygulaması dağıtmayı](virtual-network-ipv4-ipv6-dual-stack-powershell.md)öğrenin.
- [Azure CLI kullanarak IPv6 ikili yığın uygulaması dağıtmayı](virtual-network-ipv4-ipv6-dual-stack-cli.md)öğrenin.
