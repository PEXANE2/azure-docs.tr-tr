---
title: SD-WAN bağlantı mimarisi
titleSuffix: Azure Virtual WAN
description: Azure sanal WAN ile özel SD-WAN ile bağlantı kurma hakkında bilgi edinin
services: virtual-wan
author: skishen525
ms.service: virtual-wan
ms.topic: article
ms.date: 05/12/2020
ms.author: sukishen
ms.openlocfilehash: 17db9d615dcdd78ca50f88b5dad72d596bf29276
ms.sourcegitcommit: 90d2d95f2ae972046b1cb13d9956d6668756a02e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2020
ms.locfileid: "83402471"
---
# <a name="sd-wan-connectivity-architecture-with-azure-virtual-wan"></a>SD-Azure sanal WAN ile WAN bağlantısı mimarisi

Azure sanal WAN, tek bir işlemsel arabirimle birlikte birçok bulut bağlantısı ve güvenlik hizmetini birlikte getiren bir ağ hizmetidir. Bu hizmetler dalı (siteden siteye VPN aracılığıyla), uzak kullanıcı (Noktadan siteye VPN), özel (ExpressRoute) bağlantısı, sanal ağlar için bulut geçişli bağlantı, VPN ve ExpressRoute ınterconnectivity, yönlendirme, Azure Güvenlik Duvarı ve özel bağlantı için şifreleme içerir.

Azure sanal WAN, yazılım tanımlı bir WAN (SD-WAN) olsa da, şirket tabanlı SD-WAN teknolojileri ve hizmetleriyle sorunsuz bir şekilde bağlantı sağlamak için de tasarlanmıştır. Bu tür hizmetlerin çoğu [sanal WAN](virtual-wan-locations-partners.md) ekosistemimiz ve Azure ağ tarafından yönetilen hizmetler iş ortakları [(Msps)](../networking/networking-partners-msp.md)tarafından sunulur. Özel WAN ' larını SD-WAN ' a dönüştüren kuruluşlar, özel SD-WAN ' larını Azure sanal WAN ile birbirine bağlama sırasında seçeneklere sahiptir. Kuruluşlar şu seçeneklerden birini seçebilir:

* Doğrudan Interconnect modeli
* Dolaylı Interconnect modeli
* Sık kullanılan yönetilen hizmet sağlayıcısı [MSP](../networking/networking-partners-msp.md) kullanılarak YÖNETILEN karma WAN modeli

Bu durumların tümünde, SD-WAN ile sanal WAN 'ın iç bağlantısı bağlantı tarafından benzerdir, ancak düzenleme ve işletimsel tarafta farklılık gösterebilir.

## <a name="direct-interconnect-model"></a><a name="direct"></a>Doğrudan Interconnect modeli

![Doğrudan Interconnect modeli](./media/sd-wan-connectivity-architecture/direct.png)

Bu mimari modelinde, SD-WAN dalı müşteri-şirket Ekipmanı (CPE), IPSec bağlantıları üzerinden sanal WAN hub 'larına doğrudan bağlanır. Bu dal, özel SD-WAN aracılığıyla diğer dallara da bağlanabilir veya dal bağlantısı için sanal WAN özelliğinden yararlanabilir. Azure 'da iş yüklerine erişmesi gereken dallar, sanal WAN hub 'ları içinde sonlandırılan IPSec tüneli aracılığıyla doğrudan ve güvenli bir şekilde Azure 'a erişebilir.

SD-WAN CPE iş ortakları, ilgili CPE cihazlarından normalde sıkıcı ve hataya açık IPSec bağlantısını otomatikleştirmek için Otomasyonu etkinleştirebilir. Otomasyon, SD-WAN denetleyicisinin sanal WAN sitelerini yapılandırmak için sanal WAN API 'SI aracılığıyla Azure ile iletişim kurmasına olanak tanır ve gerekli IPSec tünel yapılandırmasını şube CPEs 'ye gönderir. Çeşitli SD-WAN iş ortakları tarafından sanal WAN iç bağlantı Otomasyonu açıklaması için bkz. [Otomasyon yönergeleri](virtual-wan-configure-automation-providers.md) .

SD-WAN CPE, trafik iyileştirmesinin yanı sıra yol seçiminin uygulandığı ve uygulanacağı yerde olmaya devam eder. 

Bu modelde, sanal WAN bağlantısı IPSec üzerinden yapıldığından ve sanal WAN VPN ağ geçidinde IPSec VPN sonlandırıldığı için, gerçek zamanlı trafik özelliklerine dayanan bazı satıcıya özel trafik iyileştirmesi desteklenmiyor olabilir. Örneğin, şubedeki dinamik yol seçimi, şube cihazı başka bir SD-WAN düğümüyle çeşitli ağ paketi bilgilerini değiş tokuş eden, bu nedenle dalda dinamik olarak önceliklendirilmiş çeşitli trafik için kullanılacak en iyi bağlantıyı tanımlayarak uygulanabilir. Bu özellik, son mil iyileştirmesinin (en yakın Microsoft POP 'una dallan) gerekli olduğu alanlarda yararlı olabilir.

Sanal WAN ile kullanıcılar Azure yol seçimini alabilir ve bu, CPE 'ten sanal WAN VPN ağ geçitleri arasında birden çok ISS bağlantısı arasında ilke tabanlı yol seçimidir. Sanal WAN, aynı SD-WAN dalından birden çok bağlantı (yol) kurulumuna izin verir; her bağlantı, SD-WAN CPE 'ın benzersiz bir genel IP 'sinden iki farklı Azure sanal WAN VPN Gateway örneğine bir çift tünelden bağlantı temsil eder. SD-WAN satıcıları, en iyi Azure yolunu, kendi ilke altyapısı tarafından CPE bağlantılarında ayarlanan trafik ilkelerine bağlı olarak Azure 'a uygulayabilirler.

## <a name="indirect-interconnect-model"></a><a name="indirect"></a>Dolaylı Interconnect modeli

![Dolaylı Interconnect modeli](./media/sd-wan-connectivity-architecture/indirect.png)

Bu mimari modelinde SD-WAN dalı CPEs, sanal WAN hub 'larına dolaylı olarak bağlanır. Şekilde gösterildiği gibi, bir SD-WAN sanal CPE, bir kurumsal VNet 'te dağıtılır. Bu sanal CPE, IPSec kullanılarak sanal WAN hub 'ına bağlanır. Sanal CPE, Azure 'da SD-WAN ağ geçidi olarak görev yapar. Azure 'da iş yüklerine erişmesi gereken dallar, bu sanal ağ geçidi aracılığıyla bunlara erişebilecek.

Azure 'a yönelik bağlantı, v-CPE ağ geçidi (NVA) ile oluşturulduğundan, Azure iş yüküne sanal ağları ve diğer SD-WAN dallarına giden tüm trafik, NVA aracılığıyla gider. Bu modelde, Kullanıcı yüksek kullanılabilirlik, ölçeklenebilirlik ve yönlendirme dahil olmak üzere SD-WAN NVA 'yı yönetmeden ve işlemekten sorumludur.
  
## <a name="managed-hybrid-wan-model"></a><a name="hybrid"></a>Yönetilen karma WAN modeli

![Yönetilen karma WAN modeli](./media/sd-wan-connectivity-architecture/hybrid.png)

Bu mimari modelinde, kuruluşlar yönetilen hizmet sağlayıcısı (MSP) iş ortağı tarafından sunulan yönetilen bir SD-WAN hizmetinden faydalanabilir. Bu model, yukarıda açıklanan doğrudan veya dolaylı modellerle benzerdir. Ancak, bu modelde SD-WAN tasarımı, düzenleme ve işlemler SD-WAN sağlayıcısı tarafından dağıtılır.

[Azure ağ oluşturma iş ortakları](../networking/networking-partners-msp.md) , kurumsal müşterinin Azure ABONELIĞINDEKI SD-WAN ve sanal WAN hizmetini uygulamak ve müşterinin adına uçtan uca karma WAN çalıştırmak için [Azure athouse](https://azure.microsoft.com/services/azure-lighthouse/) kullanabilir. Bu MSPs 'ler Ayrıca sanal WAN 'a Azure ExpressRoute uygulayabilir ve bunu uçtan uca yönetilen bir hizmet olarak işleyebilir.

## <a name="additional-information"></a>Ek Bilgi

* [SSS Ekle](virtual-wan-faq.md)
* [Uzak bağlantı çözme](work-remotely-support.md)
