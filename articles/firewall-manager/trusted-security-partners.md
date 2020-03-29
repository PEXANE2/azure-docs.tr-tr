---
title: Azure Güvenlik Duvarı Yöneticisi güvenilen güvenlik ortakları nelerdir (önizleme)
description: Azure Güvenlik Duvarı Yöneticisi'nin güvendiği güvenlik ortakları hakkında bilgi edinin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: victorh
ms.openlocfilehash: b92242ce9086579d0397f78853402cfc08453f68
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75436784"
---
# <a name="what-are-trusted-security-partners-preview"></a>Güvenilen güvenlik iş ortakları (önizleme) nedir?

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Güvenlik Duvarı Yöneticisi'ndeki *güvenilir güvenlik ortakları (önizleme),* kullanıcılarınız için Internet erişimini korumak için tanıdık, en iyi nesil, üçüncü taraf güvenliğinizi hizmet (SECaaS) teklifleri olarak kullanmanıza olanak tanır.

Hızlı bir yapılandırma yla, desteklenen bir güvenlik ortağıyla hub'ı güvenli hale getirebilir ve Sanal Ağlar (VNet) veya bölge içindeki şube konumlarınızdan Internet trafiğini yönlendirebilir ve filtreleyebilirsiniz. Bu, Kullanıcı Tanımlı Rotalar (ÜDR) kurmadan ve yönetmeden otomatik rota yönetimi kullanılarak yapılır.

Bu bölgelerde dünyanın her yerindeki kullanıcılarınız için bağlantı ve güvenlik elde etmek için seçtiğiniz güvenlik ortağıyla birlikte yapılandırılmış güvenli hub'ları birden çok Azure bölgesinde dağıtabilirsiniz. Güvenlik iş ortağının Internet/SaaS uygulama trafiği ve azure güvenlik duvarı teklifini güvenli hub'larda özel trafik için kullanabilme özelliği sayesinde, artık küresel olarak dağıtılan kullanıcılarınıza ve uygulamalarınız için yakın olan Azure'da güvenlik avantajınızı oluşturmaya başlayabilirsiniz.

Bu önizleme için desteklenen güvenlik ortakları **ZScaler** ve **iboss'lardır.** Desteklenen bölgeler WestCentralUS, NorthCentralUS, WestUS, WestUS2 ve EastUS'dur.

![Güvenilen güvenlik iş ortakları](media/trusted-security-partners/trusted-security-partners.png)

## <a name="key-scenarios"></a>Önemli senaryolar

Aşağıdaki senaryolarda Internet trafiğini filtrelemek için güvenlik ortaklarını kullanabilirsiniz:

- Sanal Ağ (VNet) Internet için

   Azure'da çalışan bulut iş yükleri için gelişmiş kullanıcı tarafından çalışan Internet korumasından yararlanın.

- Şubeden İnternete

   Şube için üçüncü taraf NSaaS filtresini Internet senaryolarına kolayca eklemek için Azure bağlantınızdan ve genel dağıtımınızdan yararlanın. Azure Virtual WAN'ı kullanarak küresel geçiş ağınızı ve güvenlik avantajınızı oluşturabilirsiniz.

Aşağıdaki senaryolar desteklenir:
-   VNet'ten Internet'e üçüncü taraf iş ortağı teklifi aracılığıyla.
-   Üçüncü taraf iş ortağı teklifi yle Internet'e şube.
-   Azure Güvenlik Duvarı üzerinden üçüncü taraf iş ortağı, diğer özel trafiğin (Spoke-to-Spoke, Spoke to-Branchs, Branch to-Spokes) aracılığıyla Internet'e şube.

Aşağıdaki senaryo desteklenmez:

- İş ortağı teklifi yle Internet'ten Internet'e VNet, özel trafik için Azure Güvenlik Duvarı ile birleştirilemez. Aşağıdaki sınırlamaları görün.

## <a name="current-limitations"></a>Geçerli sınırlamalar

- VNet to Internet için, özel trafik için Azure Güvenlik Duvarı ve Internet trafiği için bir iş ortağı teklifi eklemeyi karıştıramazsınız. Internet trafiğini Azure Güvenlik Duvarı'na veya güvenli sanal hub'da sunan bir üçüncü taraf güvenlik ortağı gönderebilirsiniz, ancak her ikisini birden gönderemezsiniz. 
- Sanal hub başına en fazla bir güvenlik ortağı dağıtabilirsiniz. Sağlayıcıyı değiştirmeniz gerekiyorsa, varolan iş ortağını kaldırmanız ve yeni bir iş ortağı eklemeniz gerekir.

## <a name="best-practices-for-internet-traffic-filtering-in-secured-virtual-hubs"></a>Güvenli sanal hub'larda Internet trafiği filtreleme için en iyi uygulamalar

İnternet trafiği genellikle web trafiğini içerir. Ancak, Office 365 (O365) ve Azure Depolama, Azure Sql vb. gibi Azure herkese açık PaaS hizmetleri gibi SaaS uygulamalarına yönelik trafiği de içerir. Bu hizmetlere trafik işlemek için en iyi uygulama önerileri şunlardır:

### <a name="handling-azure-paas-traffic"></a>Azure PaaS trafiğini işleme
 
- Trafiğiniz çoğunlukla Azure PaaS'tan oluşuyorsa ve uygulamalarınız için kaynak erişimi IP adresleri, FQDN'ler, Hizmet etiketleri veya FQDN etiketleri kullanılarak filtreuygulanabiliyorsa, koruma için Azure Güvenlik Duvarı'nı kullanın.

- Trafiğiniz SaaS uygulama erişiminden oluşuyorsa veya kullanıcı tarafından filtreleme (örneğin, sanal masaüstü altyapınız (VDI) iş yükleri için) gerekiyorsa veya gelişmiş Internet filtreleme özelliklerine ihtiyacınız varsa hub'larınızda bir üçüncü taraf iş ortağı çözümü kullanın.

![Azure Güvenlik Duvarı Yöneticisi için tüm senaryolar](media/trusted-security-partners/all-scenarios.png)

## <a name="handling-office-365-o365-traffic"></a>Office 365 (O365) trafiğini işleme

Genel olarak dağıtılan şube konum senaryolarında, kalan Internet trafiğini Azure güvenli hub'ınıza göndermeden önce Office 365 trafiğini doğrudan şubeye yönlendirmeniz gerekir.

Office 365 için ağ gecikmesi ve performansı başarılı kullanıcı deneyimi için çok önemlidir. Bu hedeflere en iyi performans ve kullanıcı deneyimi etrafında ulaşmak için, müşterilerin Internet trafiğinin geri kalanını Azure üzerinden yönlendirmeyi düşünmeden önce Office 365'i doğrudan ve yerel kaçış ı uygulaması gerekir.

[Office 365 ağ bağlantısı ilkeleri,](https://docs.microsoft.com/office365/enterprise/office-365-network-connectivity-principles) anahtar Office 365 ağ bağlantılarının kullanıcı dalından veya mobil aygıttan yerel olarak ve doğrudan Internet üzerinden en yakın Microsoft ağ durumu noktasına yönlendirilmesini sağlar.

Ayrıca Office 365 bağlantıları gizlilik ve performans nedenleriyle verimli, özel protokoller kullanmak için güçlü bir şekilde şifrelenir. Bu, bu bağlantıları geleneksel ağ düzeyi güvenlik çözümlerine tabi taşırık pratik ve etkili hale getirir. Bu nedenlerden dolayı, müşterilerin trafiğin geri kalanını Azure üzerinden göndermeden önce Office 365 trafiğini doğrudan şubelerden göndermelerini öneririz. Microsoft, Azure ve Office 365 ile tümleşen ve müşterilerin Office 365 doğrudan ve yerel Internet çıkışLarını etkinleştirmesini kolaylaştıran birkaç SD-WAN çözüm sağlayıcısıyla ortaklık yaptı. Ayrıntılar için [O365 politikalarımı Virtual WAN üzerinden nasıl ayarlarım?](https://docs.microsoft.com/azure/virtual-wan/virtual-wan-office365-overview)


## <a name="next-steps"></a>Sonraki adımlar

[Azure Güvenlik Duvarı Yöneticisi'ni kullanarak güvenli bir hub'da güvenilir bir güvenlik teklifi dağıtın.](deploy-trusted-security-partner.md)
