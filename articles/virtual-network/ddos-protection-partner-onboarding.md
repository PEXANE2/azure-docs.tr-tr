---
title: Azure DDoS Koruma Standardı ile ortaklık
description: Azure DDoS Koruma Standardı tarafından etkinleştirilen iş ortaklığı fırsatlarını anlayın.
ms.service: virtual-network
documentationcenter: na
author: anupamvi
mms.devlang: na
ms.topic: article
ms.date: 01/28/2020
ms.author: kumud
ms.openlocfilehash: 39cb2f90b49725c78746df69505fad7160e1db92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76849688"
---
# <a name="partnering-with-azure-ddos-protection-standard"></a>Azure DDoS Koruma Standardı ile ortaklık
Bu makalede, Azure DDoS Koruma Standardı tarafından etkinleştirilen iş ortaklığı fırsatları açıklanmaktadır. Bu makale, ürün yöneticilerinin ve iş geliştirme rollerinin yatırım yollarını anlamalarına ve ortak değer teklifleri hakkında bilgi sağlamalarına yardımcı olmak üzere tasarlanmıştır.

## <a name="background"></a>Arka plan
Dağıtılmış hizmet reddi (DDoS) saldırıları, uygulamalarını buluta taşıması nedeniyle müşterilerin dile getirdiği en yüksek kullanılabilirlik ve güvenlik sorunlarından biridir. Gasp ve hacktivism DDoS saldırıları arkasındaki ortak motivasyonları olmak ile, onlar sürekli türü, ölçek ve nispeten kolay ve ucuz başlatmak için sıklığı artmaktadır.

Azure DDoS Koruması, Azure ağının küresel ölçeğinden yararlanarak en gelişmiş DDoS tehditlerine karşı karşı önlemler sağlar. Hizmet, sanal ağlarda dağıtılan uygulamalar ve kaynaklar için gelişmiş DDoS azaltma özellikleri sağlar.

Teknoloji ortakları, DDoS saldırıları nedeniyle kullanılabilirlik ve güvenilirlik endişelerini gidermek için müşterilerinin kaynaklarını Azure DDoS Koruma Standardı ile yerel olarak koruyabilir.

## <a name="introduction-to-azure-ddos-protection-standard"></a>Azure DDoS Koruma Standardına Giriş
Azure DDoS Koruma Standardı, Katman 3 ve Katman 4 DDoS saldırılarına karşı gelişmiş DDoS azaltma özellikleri sağlar. DDoS Koruma Standardı hizmetinin temel özellikleri aşağıda verilmiştir.

### <a name="adaptive-real-time-tuning"></a>Uyarlanabilir gerçek zamanlı ayarı
Azure DDoS Koruma Standardı, korunan her uygulama için, uygulamanın trafik profili desenlerine göre DDoS azaltma ilkesi eşiklerini otomatik olarak atanır. Hizmet, iki kavrayış kullanarak bu özelleştirmeyi gerçekleştirir:

- Katman 3 ve 4 için müşteri başına (IP başına) trafik desenlerinin otomatik olarak öğrenilir.
- Azure ölçeğinin önemli miktarda trafiği absorbe etmesine olanak sağladığını göz önünde bulundurarak yanlış pozitifleri en aza indirmek.

![Uyarlanabilir gerçek zamanlı atokalma](./media/ddos-protection-partner-onboarding/real-time-tuning.png)

### <a name="attack-analytics-telemetry-monitoring-and-alerting"></a>Saldırı analizi, telemetri, izleme ve uyarı
Azure DDoS Koruması, herhangi bir kullanıcı müdahalesi olmadan DDoS saldırılarını tanımlar ve azaltır.

- Korumalı kaynak Azure Güvenlik Merkezi kapsamındaki abonelikteyse, DDoS Koruma Standardı, korumalı uygulamaya karşı bir DDoS saldırısı algılandığında ve azaltıldığında Güvenlik Merkezi'ne otomatik olarak bir uyarı gönderir.
- Alternatif olarak, korumalı bir genel IP için etkin bir azaltma olduğunda haberdar olmak için, ddos saldırısı altında metrik [üzerinde bir uyarı yapılandırabilir](manage-ddos-protection.md#configure-alerts-for-ddos-protection-metrics) veya yapılandırabilirsiniz.
- Ayrıca, diğer DDoS ölçümleri için uyarılar oluşturmayı ve saldırının ölçeğini, bırakılan trafiği, saldırı vektörlerini, en iyi katkıda bulunanları ve diğer ayrıntıları anlamak için [saldırı analizini yapılandırmayı](manage-ddos-protection.md#configure-ddos-attack-analytics) seçebilirsiniz.

![DDoS ölçümleri](./media/ddos-protection-partner-onboarding/ddos-metrics.png)

### <a name="ddos-rapid-response-drr"></a>DDoS hızlı yanıt (DRR)
DDoS Protection Standard müşterileri etkin bir saldırı sırasında [Hızlı Yanıt ekibine](https://azure.microsoft.com/blog/ddos-protection-attack-analytics-rapid-response/) erişebilir. DRR, saldırı soruşturması, saldırı sırasında ki özel azaltımlar ve saldırı sonrası analizler konusunda yardımcı olabilir.

### <a name="sla-guarantee-and-cost-protection"></a>SLA garantisi ve maliyet koruması
DDoS Koruma Standardı hizmeti %99,99 SLA kapsamındadır ve maliyet koruması belgelenmiş bir saldırı sırasında ölçeklendirmek için kaynak kredisi sağlar. Daha fazla bilgi [için Azure DDoS Koruması için SLA'ya](https://azure.microsoft.com/support/legal/sla/ddos-protection/v1_0/)bakın.

## <a name="featured-partner-scenarios"></a>Öne çıkan iş ortağı senaryoları
Azure DDoS Koruma Standardı ile tümleştirerek elde edebilirsiniz önemli yararları şunlardır:

- İş ortaklarının müşterilerine sunduğu hizmetler (yük bakiyesi, web uygulama güvenlik duvarı, güvenlik duvarı, vb.) arka uçta Azure DDoS Koruma Standardı tarafından otomatik olarak korunur (beyaz etiketli).
- İş ortakları, azure DDoS Koruma Standardı saldırı analizine ve telemetriye kendi ürünleriyle entegre edilebildikleri erişime sahip olarak birleşik bir müşteri deneyimi sunar.  
- İş ortakları, DDoS ile ilgili sorunlar için Azure hızlı yanıt yokluğunda bile DDoS hızlı yanıt desteğine erişebilir.
- Ortakların korumalı uygulamaları, DDoS saldırıları durumunda DDoS SLA garantisi ve maliyet koruması ile yedeklenir.

## <a name="technical-integration-overview"></a>Teknik entegrasyona genel bakış
Azure DDoS Koruma Standardı iş ortaklığı fırsatları Azure portalı, API'ler ve CLI/PS aracılığıyla kullanılabilir.

### <a name="integrate-with-ddos-protection-standard"></a>DDoS Koruma Standardı ile tümleştirme
İş ortaklarının Azure DDoS Koruma Standardı ile tümleştirmeyi yapılandırması için aşağıdaki adımlar gereklidir:
1. İstediğiniz (iş ortağı) aboneliğinizde bir DDoS Koruma Planı oluşturun. Adım adım talimatlar için [bkz.](manage-ddos-protection.md#create-a-ddos-protection-plan)
   > [!NOTE]
   > Belirli bir kiracı için yalnızca 1 DDoS Koruma Planı oluşturulması gerekir. 
2. Yük bakiyesi, güvenlik duvarları ve web uygulaması güvenlik duvarı gibi (iş ortağı) aboneliklerinizde herkese açık bitiş noktası olan bir hizmet dağıtın. 
3. İlk adımda oluşturulan DDoS Koruma Planı'nı kullanarak ortak uç noktaları olan hizmetin sanal ağında Azure DDoS Koruma Standardını etkinleştirin. Adım adım talimatlar için [bkz.](manage-ddos-protection.md#enable-ddos-for-an-existing-virtual-network)
   > [!IMPORTANT] 
   > Azure DDoS Koruma Standardı sanal ağda etkinleştirildikten sonra, bu sanal ağdaki tüm genel IP'ler otomatik olarak korunur. Bu genel IP'lerin kaynağı Azure (istemci aboneliği) içinde veya Azure dışında olabilir. 
4. İsteğe bağlı olarak, Azure DDoS Koruma Standardı telemetrisini ve saldırı analitiğini uygulamaya özel müşteriye yönelik panonuza entegre edin. Telemetri kullanımı hakkında daha fazla bilgi için [bkz.](manage-ddos-protection.md#use-ddos-protection-telemetry) Saldırı analizini yapılandırma hakkında daha fazla bilgi için [bkz.](manage-ddos-protection.md#configure-ddos-attack-analytics)

### <a name="onboarding-guides-and-technical-documentation"></a>Onboarding kılavuzları ve teknik dokümantasyon

- [Azure DDoS Koruması ürün sayfası](https://azure.microsoft.com/services/ddos-protection/)
- [Azure DDoS Koruma belgeleri](ddos-protection-overview.md)
- [Azure DDoS Koruma API başvurusu](https://docs.microsoft.com/rest/api/virtualnetwork/ddosprotectionplans)
- [Azure sanal ağ API başvurusu](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks)

### <a name="get-help"></a>Yardım alın

- Azure DDoS Koruma Standardı ile uygulama, hizmet veya ürün tümleştirmeleri hakkında sorularınız varsa, [Azure güvenlik topluluğuna](https://techcommunity.microsoft.com/t5/security-identity/bd-p/Azure-Security)ulaşın.
- [Stack Taşma](https://stackoverflow.com/tags/azure-ddos/)ile ilgili tartışmaları izleyin.

### <a name="get-to-market"></a>Pazara geçin

- Microsoft ile ortaklık için birincil program [Microsoft İş Ortağı Ağı'dır.](https://partner.microsoft.com/) – Microsoft Graph Security tümleştirmeleri [MPN Bağımsız Yazılım Satıcısı (ISV)](https://partner.microsoft.com/saas-solution-guide) yoluna girer.
- [Microsoft Akıllı Güvenlik İlişkisi,](https://www.microsoft.com/security/business/intelligent-security-association?rtc=1) güvenlik ürünlerinizi zenginleştirmenize ve Microsoft Security ürünlerine tümleştirmelerinizin müşteri tarafından keşfedilebilirliğini artırmaya yardımcı olmak için özel olarak Microsoft Güvenlik İş Ortakları için kullanılan bir programdır.

## <a name="next-steps"></a>Sonraki adımlar
Varolan iş ortağı tümleştirmelerini görüntüleyin:

- [Barracuda WAF-as-a-hizmet](https://www.barracuda.com/waf-as-a-service)
- [Radware'den Azure Bulut WAF](https://www.radware.com/resources/microsoft-azure/)
