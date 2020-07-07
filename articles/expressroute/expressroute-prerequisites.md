---
title: 'Azure ExpressRoute: Önkoşullar'
description: Bu sayfada bir Azure ExpressRoute bağlantı hattı sipariş etmeden önce karşılanması gereken gereksinimlerin listesi bulunmaktadır. Bir denetim listesi içerir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: cherylmc
ms.openlocfilehash: a72eba9bde0745e66bdf8e7efd8eaec7d6a0b186
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74083365"
---
# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute önkoşulları ve denetim listesi
ExpressRoute kullanarak Microsoft bulut hizmetlerine bağlanmak için, aşağıdaki bölümlerde listelenen gereksinimlerin karşılanmış olduğunu doğrulamanız gerekir.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure hesabı
* Geçerli ve etkin bir Microsoft Azure hesabı. ExpressRoute bağlantı hattı kurmak için bu hesap gereklidir. ExpressRoute bağlantı hatları Azure aboneliklerinin içindeki kaynaklardır. Bağlantı, Office 365 gibi Azure olmayan Microsoft bulut hizmetleriyle sınırlı olsa bile bir Azure aboneliği gereksinimidir.
* Etkin bir Office 365 aboneliği (Office 365 hizmetlerini kullanıyorsanız) Daha fazla bilgi için bu makalenin Office 365'e özel gereksinimler bölümünü okuyun.

## <a name="connectivity-provider"></a>Bağlantı sağlayıcısı

* Microsoft bulut’a bağlanmak için [ExpressRoute bağlantı ortağı](expressroute-locations.md#partners) ile çalışabilirsiniz. Şirket içi ağınız ve Microsoft arasında bağlantıyı [üç şekilde](expressroute-introduction.md) kurabilirsiniz.
* Sağlayıcınız ExpressRoute bağlantı ortağı değilse bile [bulut değişim sağlayıcısı](expressroute-locations.md#connectivity-through-exchange-providers) yoluyla Microsoft bulut’a bağlanabilirsiniz.

## <a name="network-requirements"></a>Ağ gereksinimleri
* **Her bir eşleme konumunda artıklık**: Microsoft, her ExpressRoute devresi üzerinde Microsoft 'un yönlendiricileri ve eşleme yönlendiricileri arasında ( [bir bulut değişimine yalnızca bir fiziksel bağlantınız](expressroute-faqs.md#onep2plink)olduğunda bile) ayarlanmış olması gerekir.
* **Olağanüstü durum kurtarma Için yedeklilik**: Microsoft, tek bir başarısızlık noktasını önlemek için farklı eşleme konumlarında en az Iki ExpressRoute devrenizin ayarlamanızı kesinlikle önerir.
* **Yönlendirme**: Microsoft bulut nasıl bağlanadığınıza bağlı olarak, siz veya sağlayıcınızın [yönlendirme etkı alanları](expressroute-circuit-peerings.md)için BGP oturumlarını ayarlaması ve yönetmesi gerekir. Bazı Ethernet bağlantı sağlayıcıları veya bulut Exchange sağlayıcıları, değer ekleme hizmeti olarak BGP yönetimi sunabilir.
* **NAT**: Microsoft genel IP adreslerini sadece Microsoft eşleme yoluyla kabul eder. Şirket içi ağınızda özel IP adresleri kullanıyorsanız, siz veya sağlayıcınızın [NAT kullanarak](expressroute-nat.md)özel IP ADRESLERINI genel IP adreslerine çevirmesi gerekir.
* **QoS**: Skype Kurumsal’da farklı QoS davranışları gerektiren çeşitli hizmetler (ör. ses, video, mesaj) vardır. [Qos gereksinimleri](expressroute-qos.md)ne siz ve sağlayıcınız tarafından uyulmalıdır.
* **Ağ Güvenliği**: ExpressRoute ile Microsoft Bulut'a bağlanırken [ağ güvenliğine](../best-practices-network-security.md) dikkat edin.

## <a name="office-365"></a>Office 365
ExpressRoute'ta Office 365'i etkinleştirmeyi planlıyorsanız, Office 365 gereksinimleri hakkında daha fazla bilgi içeren aşağıdaki belgelere göz atın.

* [Office 365 için ExpressRoute’a Genel Bakış](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Office 365 için ExpressRoute ile yönlendirme](https://support.office.com/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
* [ExpressRoute ile yüksek kullanılabilirlik ve yük devretme](https://aka.ms/erhighavailability)
* [Office 365 URL’leri ve IP adres aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Office 365 için ağ planlama ve performans ayarlama](https://support.office.com/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Ağ bant genişliği hesaplayıcıları ve araçları](https://support.office.com/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
* [Office 365’i şirket içi ortamlarla tümleştirme](https://support.office.com/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)
* [Office 365'te ExpressRoute ileri düzey eğitim videoları](https://channel9.msdn.com/series/aer/)

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
* ExpressRoute bağlantı sağlayıcısı bulun. Bkz. [ExpressRoute ortakları ve eşleme konumları](expressroute-locations.md).
* [Yönlendirme](expressroute-routing.md), [NAT](expressroute-nat.md)ve [QoS](expressroute-qos.md)için gereksinimlere bakın.
* ExpressRoute bağlantınızı yapılandırın.
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)
