---
title: Azure Güvenlik Duvarı Yöneticisi güvenlik iş ortağı sağlayıcıları nelerdir?
description: Azure Güvenlik Duvarı Yöneticisi güvenlik iş ortağı sağlayıcıları hakkında bilgi edinin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 34da82510f96ef7bde65ceec397b048c941e3234
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85563615"
---
# <a name="what-are-security-partner-providers"></a>Güvenlik iş ortağı sağlayıcıları nedir?

Azure Güvenlik Duvarı Yöneticisi 'ndeki *güvenlik iş ortağı sağlayıcıları* , kullanıcılarınız için Internet erişimini korumak üzere tanıdık, en uygun, yerleşik, üçüncü taraf güvenlik hizmeti (SECaaS) tekliflerini kullanmanıza olanak sağlar.

Hızlı yapılandırmayla, desteklenen bir güvenlik ortağıyla bir hub 'ı güvenli hale getirebilirsiniz ve Internet trafiğini sanal ağlarınızdan (VNet) veya bir bölgedeki dal konumlarından yönlendirebilir ve filtreleyebilirsiniz. Bunu otomatik yönlendirme yönetimi ile, Kullanıcı tanımlı yollar (UDRs) ayarlamadan ve yönetmeden yapabilirsiniz.

Kullanıcılarınızın bu bölgelerdeki dünya genelinde her yerde bağlantı ve güvenlik sağlamak için birden fazla Azure bölgesinde tercih ettiğiniz güvenlik ortağıyla yapılandırılmış güvenli hub 'ları dağıtabilirsiniz. Güvenlik iş ortağının Internet/SaaS uygulama trafiği ve güvenli hub 'larda özel trafik için Azure Güvenlik Duvarı tekliflerini kullanma olanağı sayesinde, artık Azure 'da, genel olarak dağıtılmış kullanıcılarınıza ve uygulamalarınıza yakın olan güvenlik ucunu oluşturmaya başlayabilirsiniz.

Desteklenen güvenlik iş ortakları **Zscaler**, **Check Point** (Preview) ve **ıpatron** (Önizleme).

![Güvenlik iş ortağı sağlayıcıları](media/trusted-security-partners/trusted-security-partners.png)

## <a name="key-scenarios"></a>Önemli senaryolar

Aşağıdaki senaryolarda Internet trafiğini filtrelemek için güvenlik iş ortaklarını kullanabilirsiniz:

- Sanal ağ (VNet) ile Internet arasında

   Azure üzerinde çalışan bulut iş yükleriniz için Gelişmiş Kullanıcı tarafından uyumlu Internet korumasından yararlanın.

- Internet 'e dallan

   Şube için üçüncü taraf NSaaS filtrelemesini kolayca Internet senaryolarına eklemek için Azure bağlantınız ve küresel dağıtım özelliğinden yararlanın. Azure sanal WAN kullanarak küresel transit ağınızı ve güvenlik ucunu oluşturabilirsiniz.

Aşağıdaki senaryolar desteklenir:
- Azure Güvenlik Duvarı aracılığıyla bir güvenlik iş ortağı sağlayıcısı ve diğer trafik (bağlı ağa bağlı, dala bağlı, şube ve şube) aracılığıyla Internet 'e VNet/dalı.
- Güvenlik iş ortağı sağlayıcısı aracılığıyla VNet/Internet 'e dallan

## <a name="best-practices-for-internet-traffic-filtering-in-secured-virtual-hubs"></a>Güvenli sanal hub 'larda Internet trafiği Filtreleme için en iyi uygulamalar

Internet trafiği genellikle Web trafiğini içerir. Ayrıca, Office 365 (O365) gibi SaaS uygulamalarına ve Azure depolama, Azure SQL gibi Azure genel PaaS hizmetlerine giden trafiği de içerir. Aşağıda, bu hizmetlere giden trafiği işlemeye yönelik en iyi yöntem önerileri verilmiştir:

### <a name="handling-azure-paas-traffic"></a>Azure PaaS trafiğini işleme
 
- Trafiğiniz çoğunlukla Azure PaaS içeriyorsa ve uygulamalarınıza yönelik kaynak erişiminin IP adresleri, FQDN 'Ler, hizmet etiketleri veya FQDN etiketleri kullanılarak filtrelenebilir olması için Azure Güvenlik Duvarı 'Nı koruma için kullanın.

- Ortamınızdaki bir üçüncü taraf iş ortağı çözümünü, trafiğiniz SaaS uygulama erişimi 'nden oluşuyorsa veya Kullanıcı duyarlı filtrelemeye (örneğin, sanal masaüstü altyapısı (VDı) iş yükleriniz için) veya gelişmiş Internet filtreleme özelliklerine ihtiyacınız varsa kullanın.

![Azure Güvenlik Duvarı Yöneticisi için tüm senaryolar](media/trusted-security-partners/all-scenarios.png)

## <a name="handling-office-365-o365-traffic"></a>Office 365 (O365) trafiğini işleme

Küresel olarak dağıtılan dal konumu senaryolarında, Azure güvenli hub 'ınıza kalan Internet trafiğini göndermeden önce Office 365 trafiğini doğrudan dalda yeniden yönlendirmelisiniz.

Office 365 için, başarılı bir kullanıcı deneyimi için ağ gecikme süresi ve performansı kritik öneme sahiptir. En iyi performans ve Kullanıcı deneyiminden bu hedeflere ulaşmak için müşteriler, Internet trafiğinin geri kalanını Azure üzerinden yönlendirmeyi düşünmeden önce Office 365 Direct ve yerel kaçış işlemlerini gerçekleştirmelidir.

[Office 365 ağ bağlantısı ilkeleri](https://docs.microsoft.com/office365/enterprise/office-365-network-connectivity-principles) , anahtar Office 365 ağ bağlantılarının Kullanıcı dalından veya mobil cihazdan yerel olarak yönlendirilmesi için çağrı ve doğrudan Internet üzerinden Microsoft 'un sahip olduğu en yakın Microsoft ağ iletişim noktası.

Ayrıca, Office 365 bağlantılarının gizliliği için şifrelenmesi ve performans nedenleriyle verimli, özel protokoller kullanılması gerekir. Bu, bu bağlantıları geleneksel ağ düzeyi güvenlik çözümlerine tabi hale getirir. Bu nedenlerden dolayı, müşterilerin Azure üzerinden trafiği geri göndermeden önce doğrudan dallardan Office 365 trafiği göndermesini öneririz. Microsoft, Azure ve Office 365 ile tümleşen birçok SD-WAN çözümü sağlayıcısıyla işbirliği yaptı ve müşterilerin Office 365 Direct ve yerel Internet kırılımı etkinleştirmesini kolaylaştırır. Ayrıntılar için bkz. [sanal WAN aracılığıyla O365 ilkelermi ayarlamak nasıl yaparım??](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-office365-overview)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Güvenlik Duvarı Yöneticisi 'ni kullanarak güvenli bir hub 'da güvenlik ortağı teklifi dağıtın](deploy-trusted-security-partner.md).
