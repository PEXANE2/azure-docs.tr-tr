---
title: 'Azure ExpressRoute: devreler ve akran'
description: Bu sayfa, ExpressRoute devrelerine ve yönlendirme etki alanlarına/bakmaya genel bir bakış sağlar.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: mialdrid
ms.openlocfilehash: c68ffd019937f902567c3deda8d879448dc082da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281359"
---
# <a name="expressroute-circuits-and-peering"></a>ExpressRoute devreleri ve eşleme

ExpressRoute devreleri, şirket içi altyapınızı bir bağlantı sağlayıcısı aracılığıyla Microsoft'a bağlar. Bu makale, ExpressRoute devrelerini ve yönlendirme etki alanlarını/bakışları anlamanıza yardımcı olur. Aşağıdaki şekil WAN ve Microsoft arasındaki bağlantının mantıksal bir gösterimini gösterir.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

> [!IMPORTANT]
> Azure genel bakışları amortismana uğradı ve yeni ExpressRoute devreleri için kullanılamaz. Yeni devreler Microsoft'un eşleneme ve özel bakışdestekler.  
>

## <a name="expressroute-circuits"></a><a name="circuits"></a>ExpressRoute bağlantı hatları

ExpressRoute devresi, bir bağlantı sağlayıcısı aracılığıyla şirket içi altyapınızla Microsoft bulut hizmetleri arasındaki mantıksal bağlantıyı temsil eder. Birden çok ExpressRoute devresi sipariş edebilirsiniz. Her devre aynı veya farklı bölgelerde olabilir ve farklı bağlantı sağlayıcıları aracılığıyla tesislerinize bağlanabilir.

ExpressRoute devreleri herhangi bir fiziksel varlıkla eşleşmez. Bir devre, hizmet anahtarı (s-tuşu) olarak adlandırılan standart bir GUID tarafından benzersiz olarak tanımlanır. Hizmet anahtarı, bağlantı sağlayıcısı Microsoft ile sizin aranız arasında değiş tokuş edilen tek bilgidir. S-key güvenlik amacıyla bir sır değildir. ExpressRoute devresi ile s tuşu arasında 1:1 eşleme vardır.

Yeni ExpressRoute devreleri iki bağımsız eşleme içerebilir: Özel akranve Microsoft bakış. Mevcut ExpressRoute devreleri üç eşleme içerebilir: Azure Genel, Azure Özel ve Microsoft. Her bir bakış, her biri yüksek kullanılabilirlik için gereksiz olarak yapılandırılan bir çift bağımsız BGP oturumudur. ExpressRoute devresi ile yönlendirme etki alanları arasında 1:N (1 <= N <= 3) eşleme vardır. Bir ExpressRoute devresi, ExpressRoute devresi başına herhangi bir, iki veya üç eşlemenin etkinleştirilmiş olabilir.

Her devre sabit bant genişliğine (50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 10 Gbps) sahiptir ve bir bağlantı sağlayıcısına ve bir eşleme konumuna eşlenir. Seçtiğiniz bant genişliği tüm devre eşlemeleri arasında paylaşılır

### <a name="quotas-limits-and-limitations"></a><a name="quotas"></a>Kotalar, sınırlar ve sınırlamalar

Varsayılan kotalar ve limitler her ExpressRoute devresi için geçerlidir. Kotalarla ilgili güncel bilgiler için [Azure Abonelik ve Hizmet Sınırları, Kotalar ve Kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md) sayfasına bakın.

## <a name="expressroute-peering"></a><a name="routingdomains"></a>ExpressRoute bakış

ExpressRoute devresi, onunla ilişkili birden çok yönlendirme etki alanına/eşleme alanına sahiptir: Azure genel, Azure özel ve Microsoft. Her eşleme, yüksek kullanılabilirlik için bir çift yönlendiricide (etkin etkin veya yük paylaşımı yapılandırmasında) aynı şekilde yapılandırılır. Azure hizmetleri, IP adresleme düzenlerini temsil etmek üzere *Azure geneli* ve *Azure özel* olarak sınıflandırılır.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="azure-private-peering"></a><a name="privatepeering"></a>Azure özel eşlemesi

Sanal ağ içinde dağıtılan sanal makineler (IaaS) ve bulut hizmetleri (PaaS) gibi Azure bilgi işlem hizmetleri özel eşleme etki alanı üzerinden bağlanabilir. Özel bakan etki alanı, çekirdek ağınızın Microsoft Azure'a güvenilir bir uzantısı olarak kabul edilir. Çekirdek ağınızla Azure sanal ağlarınız (VNet) arasında çift yönlü bağlantı ayarlayabilirsiniz. Bu bakış, sanal makinelere ve bulut hizmetlerine doğrudan özel IP adreslerine bağlanmanızı sağlar.  

Birden fazla sanal ağı özel eşalan etki alanına bağlayabilirsiniz. Sınırlar ve sınırlamalar hakkında bilgi için [SSS sayfasını](expressroute-faqs.md) inceleyin. Limitler hakkında güncel bilgi almak için [Azure Abonelik ve Hizmet Sınırları, Kotalar ve Kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md) sayfasını ziyaret edebilirsiniz.  Yönlendirme yapılandırması hakkında ayrıntılı bilgi için [Yönlendirme](expressroute-routing.md) sayfasına bakın.

### <a name="microsoft-peering"></a><a name="microsoftpeering"></a>Microsoft'a bakış

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

Microsoft çevrimiçi hizmetlerine bağlantı (Office 365 ve Azure PaaS hizmetleri) Microsoft'un bakışları aracılığıyla gerçekleşir. Microsoft peering yönlendirme etki alanı aracılığıyla WAN ve Microsoft bulut hizmetleriniz arasında çift yönlü bağlantı sağlarız. Microsoft bulut hizmetlerine yalnızca sizin veya bağlantı sağlayıcınıza ait genel IP adresleri üzerinden bağlanmanız ve tanımlanan tüm kurallara uymanız gerekir. Daha fazla bilgi için [ExpressRoute ön koşullar](expressroute-prerequisites.md) sayfasına bakın.

Desteklenen hizmetler, maliyetler ve yapılandırma ayrıntıları hakkında daha fazla bilgi için [SSS sayfasına](expressroute-faqs.md) bakın. Microsoft'a bakış desteği sunan bağlantı sağlayıcıları listesinde bilgi için [ExpressRoute Konumları](expressroute-locations.md) sayfasına bakın.

## <a name="peering-comparison"></a><a name="peeringcompare"></a>Eşleme karşılaştırması

Aşağıdaki tablo üç eşlemeleri karşılaştırır:

[!INCLUDE [peering comparison](../../includes/expressroute-peering-comparison.md)]

ExpressRoute devrenizin bir parçası olarak yönlendirme etki alanlarından bir veya daha fazlasını etkinleştirebilirsiniz. Tek bir yönlendirme etki alanında birleştirmek istiyorsanız, tüm yönlendirme etki alanlarının aynı VPN'e konmasını seçebilirsiniz. Bunları diyagrama benzer şekilde farklı yönlendirme etki alanlarında da koyabilirsiniz. Önerilen yapılandırma, özel eşlemenin doğrudan çekirdek ağa bağlı olması ve ortak ve Microsoft eşleme bağlantılarının DMZ'nize bağlı olmasıdır.

Her eşleme ayrı BGP oturumları (her eşleme türü için bir çift) gerektirir. BGP oturum çiftleri son derece kullanılabilir bir bağlantı sağlar. Katman 2 bağlantı sağlayıcıları aracılığıyla bağlantı kuruyorsanız, yönlendirmeyi yapılandırmaktan ve yönetmekten siz sorumlusunuz. ExpressRoute'u ayarlamak için [iş akışlarını](expressroute-workflows.md) inceleyerek daha fazla bilgi edinebilirsiniz.

## <a name="expressroute-health"></a><a name="health"></a>ExpressRoute durumu

ExpressRoute devreleri kullanılabilirlik, VNet'lere bağlantı ve [Ağ Performans Monitörü](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM) kullanılarak bant genişliği kullanımı için izlenebilir.

NPM, Azure özel eşleme ve Microsoft'un bakış durumunu izler. Daha fazla bilgi için [yazımıza](https://azure.microsoft.com/blog/monitoring-of-azure-expressroute-in-preview/) göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* Bir hizmet sağlayıcı bulun. Bkz. [ExpressRoute hizmet sağlayıcıları ve konumları.](expressroute-locations.md)
* Tüm önkoşulların sağlandığından emin olun. Bkz. [ExpressRoute önkoşulları](expressroute-prerequisites.md).
* ExpressRoute bağlantınızı yapılandırın.
  * [ExpressRoute devreleri oluşturup yönetme](expressroute-howto-circuit-portal-resource-manager.md)
  * [ExpressRoute işlem hatları için yönlendirmeyi (eşleme) yapılandırma](expressroute-howto-routing-portal-resource-manager.md)
