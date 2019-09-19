---
title: 'Önkoşullar-ExpressRoute: Azure | Microsoft Docs'
description: Bu sayfada bir Azure ExpressRoute bağlantı hattı sipariş etmeden önce karşılanması gereken gereksinimlerin listesi bulunmaktadır. Bu, bir denetim listesi içerir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 4ef1a768356078c160b798c1a96412adda0330f6
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71123286"
---
# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute önkoşulları ve denetim listesi
ExpressRoute kullanarak Microsoft bulut hizmetlerine bağlanmak için, aşağıdaki bölümlerde listelenen gereksinimlerin karşılanmış olduğunu doğrulamanız gerekir.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure hesabı
* Geçerli ve etkin bir Microsoft Azure hesabı. ExpressRoute bağlantı hattı kurmak için bu hesap gereklidir. ExpressRoute bağlantı hatları Azure aboneliklerinin içindeki kaynaklardır. Bağlantı, Office 365 gibi Azure olmayan Microsoft bulut hizmetleriyle sınırlı olsa bile bir Azure aboneliği gereksinimidir.
* Etkin bir Office 365 aboneliği (Office 365 hizmetlerini kullanıyorsanız) Daha fazla bilgi için bu makaledeki Office 365 özgü gereksinimler bölümüne bakın.

## <a name="connectivity-provider"></a>Bağlantı sağlayıcısı

* Microsoft bulut’a bağlanmak için [ExpressRoute bağlantı ortağı](expressroute-locations.md#partners) ile çalışabilirsiniz. Şirket içi ağınız ve Microsoft arasında bağlantıyı [üç şekilde](expressroute-introduction.md) kurabilirsiniz.
* Sağlayıcınız ExpressRoute bağlantı ortağı değilse bile [bulut değişim sağlayıcısı](expressroute-locations.md#connectivity-through-exchange-providers) yoluyla Microsoft bulut’a bağlanabilirsiniz.

## <a name="network-requirements"></a>Ağ gereksinimleri
* **Her eşleme konumundaki artıklık**: Microsoft, her ExpressRoute bağlantı hattının Microsoft 'un yönlendiricileri ve eşleme yönlendiricileri arasında ayarlanmasını gerektirir ( [bir bulut değişimine yalnızca bir fiziksel bağlantınız](expressroute-faqs.md#onep2plink)olduğunda bile).
* **Olağanüstü durum kurtarma Için yedeklilik**: Microsoft, tek bir hata noktası oluşmasını önlemek için farklı eşleme konumlarında en az iki ExpressRoute devrenizin ayarlamanızı kesinlikle önerir.
* **Yönlendirme**: Microsoft Cloud için nasıl bağlandığınıza bağlı olarak, siz veya sağlayıcınız ayarlamak ve yönetmek için BGP oturumları gereken [Yönlendirme etki alanları](expressroute-circuit-peerings.md). Bazı Ethernet bağlantı sağlayıcıları veya Bulut exchange sağlayıcıları olarak BGP Yönetimi sunabilir bir değer ekleme hizmeti.
* **NAT**: Microsoft, yalnızca Microsoft eşlemesi aracılığıyla genel IP adreslerini kabul eder. Şirket içi ağınızda özel IP adresleri kullanıyorsanız, özel IP adreslerini genel IP adreslerine çevirmek siz veya sağlayıcınız gereken [NAT kullanarak](expressroute-nat.md).
* **QoS**: Skype Kurumsal, farklı bir QoS işlemi gerektiren çeşitli hizmetlere (örneğin, ses, video, metin) sahiptir. [Qos gereksinimleri](expressroute-qos.md)ne siz ve sağlayıcınız tarafından uyulmalıdır.
* **Ağ Güvenliği**: ExpressRoute ile Microsoft Bulut'a bağlanırken [ağ güvenliğine](../best-practices-network-security.md) dikkat edin.

## <a name="office-365"></a>Office 365
ExpressRoute'ta Office 365'i etkinleştirmeyi planlıyorsanız, Office 365 gereksinimleri hakkında daha fazla bilgi içeren aşağıdaki belgelere göz atın.

* [Office 365 için ExpressRoute’a Genel Bakış](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Office 365 için ExpressRoute ile Yönlendirme](https://support.office.com/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
* [ExpressRoute ile yüksek kullanılabilirlik ve yük devretme](https://aka.ms/erhighavailability)
* [Office 365 URL’leri ve IP adresi aralıkları](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Office 365 için ağ planlama ve performans ayarı](https://support.office.com/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Ağ bant genişliği hesaplayıcıları ve araçları](https://support.office.com/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
* [Office 365’i şirket içi ortamlarla tümleştirme](https://support.office.com/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)
* [Office 365'te ExpressRoute ileri düzey eğitim videoları](https://channel9.msdn.com/series/aer/)

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute hakkında daha fazla bilgi için, bkz. [ExpressRoute SSS](expressroute-faqs.md).
* ExpressRoute bağlantı sağlayıcısı bulun. Bkz. [ExpressRoute ortakları ve eşleme konumları](expressroute-locations.md).
* [Yönlendirme](expressroute-routing.md), [NAT](expressroute-nat.md) ve [QoS](expressroute-qos.md) için gereksinimlere bakın.
* ExpressRoute bağlantınızı yapılandırın.
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)
