---
title: Azure Güvenlik kıyaslaması V2-ağ güvenliği
description: Azure Güvenlik kıyaslaması V2 ağ güvenliği
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 09/13/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 184416794011d259af3568c81e4648d822a2c4a5
ms.sourcegitcommit: 94c750edd4d755d6ecee50ac977328098a277479
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90059437"
---
# <a name="security-control-network-security"></a>Güvenlik denetimi: ağ güvenliği

Ağ güvenliği, Azure ağlarını güvenli hale getirmeye ve korumanıza yönelik denetimleri ele alır. Bu, sanal ağların güvenliğini sağlamayı, özel bağlantılar oluşturmayı, dış saldırıları engellemeyi ve azaltıcı ve DNS güvenliğini içerir.

## <a name="ns-1-implement-security-for-internal-traffic"></a>NS-1: iç trafik için güvenlik uygulama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| NS-1 | 9,2, 9,4, 14,1, 14,2, 14,3 | AC-4, CA-3, SC-7 |

Tüm Azure sanal ağlarının, iş risklerine göre bir kurumsal kesimleme ilkesini izlediğinden emin olun. Kuruluş için daha yüksek risk doğurabilecek tüm sistemleri kendi sanal ağı içinde yalıtılmalı ve bir ağ güvenlik grubu (NSG) ve/veya Azure güvenlik duvarıyla yeterince güvenli hale gelmelidir. 

Uygulamalarınız ve kurumsal segmentasyon stratejinize bağlı olarak, ağ güvenlik grubu kurallarına göre iç kaynaklar arasındaki trafiği kısıtlayın veya buna izin verin. Belirli bir iyi tanımlanmış uygulamalarda (3 katmanlı bir uygulama gibi), bu, yüksek güvenlikli bir "varsayılan olarak Reddet, özel duruma izin ver" yaklaşımına sahip olabilir. Birbirleriyle etkileşim kuran çok sayıda uygulamanız ve uç noktalarınız varsa, bu durum düzgün ölçeklenmeyebilir. Azure Güvenlik Duvarı 'nı, çok sayıda kurumsal segment veya bağlı bileşen (hub/uç topolojisi) üzerinde merkezi yönetim 'in gerekli olduğu koşullarda da kullanabilirsiniz. 

Bağlantı noktalarını ve kaynak IP 'Leri dış ağ trafiği kurallarına göre sınırlayan ağ güvenlik grubu yapılandırmalarının kullanılmasını önermek için Azure Güvenlik Merkezi Uyarlamalı ağ sağlamlaştırma kullanın.

- [Güvenlik kuralları ile ağ güvenlik grubu oluşturma](../../virtual-network/tutorial-filter-network-traffic.md)

- [Azure Güvenlik duvarını dağıtma ve yapılandırma](../../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Güvenlik Merkezi 'nde Uyarlamalı ağ sağlamlaştırma](../../security-center/security-center-adaptive-network-hardening.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)     

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="ns-2-connect-private-networks-together"></a>NS-2: özel ağları birbirine bağlama

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| NS-2 | Yok | CA-3, AC-17, MA-4 |

Azure veri merkezleri ile şirket içi altyapı arasında özel bağlantılar oluşturmak için Azure ExpressRoute veya Azure sanal özel ağ (VPN) kullanın. ExpressRoute bağlantıları, genel İnternet üzerinden geçmez ve tipik İnternet bağlantılarına göre daha fazla güvenilirlik, daha hızlı hız ve daha düşük gecikme süresi sunar. Noktadan siteye VPN ve siteden siteye VPN için, bu VPN seçeneklerinin ve Azure ExpressRoute 'un herhangi bir birleşimini kullanarak şirket içi cihazları veya ağları bir sanal ağa bağlayabilirsiniz.

İki veya daha fazla sanal ağı Azure 'da birlikte bağlamak için sanal ağ eşlemesi ' ni kullanın. Eşlenen sanal ağlar arasındaki ağ trafiği özeldir ve Azure omurga ağında tutulur. 

- [ExpressRoute bağlantı modelleri nelerdir?](../../expressroute/expressroute-connectivity-models.md) 

- [Azure VPN 'ye Genel Bakış](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

- [Sanal ağ eşleme](../../virtual-network/virtual-network-peering-overview.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)    

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

## <a name="ns-3-establish-private-network-access-to-azure-services"></a>NS-3: Azure hizmetlerine özel ağ erişimi oluşturma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| NS-3 | 14,1 | AC-4, CA-3, SC-7 |

İnternet 'ten çıkmadan sanal ağlarınızdan Azure hizmetlerine özel erişimi etkinleştirmek için Azure özel bağlantısı 'nı kullanın. Azure özel bağlantısının henüz kullanılamadığı durumlarda Azure sanal ağ hizmet uç noktalarını kullanın.  Azure sanal ağ hizmet uç noktaları, Azure omurga ağı üzerinden iyileştirilmiş bir yol aracılığıyla hizmetlere güvenli erişim sağlar.  

Özel erişim, Azure hizmetleri tarafından sunulan kimlik doğrulama ve trafik güvenliğine ek olarak ayrıntılı bir savunma ölçümüdür. 

- [Azure özel bağlantısını anlama](../../private-link/private-link-overview.md)

- [Sanal ağ hizmeti uç noktalarını anlama](../../virtual-network/virtual-network-service-endpoints-overview.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)   

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

## <a name="ns-4-protect-applications-and-services-from-external-network-attacks"></a>NS-4: uygulamaları ve Hizmetleri dış ağ saldırılarına karşı koruma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| NS-4 | 9,5, 12,3, 12,9 | SC-5, SC-7 |

Dağıtılmış hizmet reddi (DDoS) saldırıları, uygulamaya özgü saldırılar ve istenmeyen ve potansiyel olarak kötü amaçlı internet trafiği dahil olmak üzere dış ağlardan gelen saldırılara karşı Azure kaynaklarını koruyun. Azure, bunun için yerel özellikleri içerir:
-   Uygulamaları ve Hizmetleri Internet 'ten ve diğer dış konumlardan gelen kötü amaçlı olabilecek trafiğe karşı korumak için Azure Güvenlik Duvarı 'nı kullanın. 

-   Uygulamalarınızı, hizmetlerinizi ve API 'lerinizi uygulama katmanı saldırılarına karşı korumak için Azure Application Gateway, Azure ön kapısında ve Azure Content Delivery Network (CDN) Web uygulaması güvenlik duvarı (WAF) yeteneklerini kullanın. 

-   Azure sanal ağlarınızda DDoS standart korumasını etkinleştirerek varlıklarınızı DDoS saldırılarına karşı koruyun. 

- [Azure Güvenlik Duvarı Belgeleri](/azure/firewall/)

- [Azure WAF dağıtma](../../web-application-firewall/overview.md)

- [Azure portal kullanarak Azure DDoS koruması standardını yönetme](../../virtual-network/manage-ddos-protection.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

Yok

## <a name="ns-5-deploy-intrusion-detectionintrusion-prevention-systems-idsips"></a>NS-5: izinsiz giriş algılama/yetkisiz erişim önleme sistemleri (KIMLIKLER/IP 'ler) dağıtma

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| NS-5 | 12,6, 12,7 | SI-4 |

Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme kullanarak, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen ve giden trafiği engelleme ve/veya engelleyin. IP adresleri ve etki alanları, Microsoft Tehdit Analizi akışından alınır. Yük incelemesi gerektiğinde, üçüncü taraf bir izinsiz giriş algılama/izinsiz giriş dağıtımı, Azure Marketi 'nden yük İnceleme özelliklerine sahip olan sistemleri (KIMLIKLER/IP 'ler) engelleyebilir. Alternatif olarak, ağ tabanlı KIMLIKLER/IP 'ler yerine veya ile birlikte ana bilgisayar tabanlı KIMLIKLERI/IP 'leri veya ana bilgisayar tabanlı bir uç nokta algılama ve yanıt (EDR) çözümünü kullanabilirsiniz.  

Note: KIMLIKLER/IP 'ler için bir yasal veya diğer gereksinimleriniz varsa, SıEM çözümünüz için yüksek kaliteli uyarılar sağlamak üzere her zaman ayarlanmış olduğundan emin olun. 

- [Azure Güvenlik duvarını dağıtma](../../firewall/tutorial-firewall-deploy-portal.md)

- [Azure Marketi, üçüncü taraf KIMLIKLERI özelliklerini içerir](https://azuremarketplace.microsoft.com/marketplace?search=IDS)

- [Microsoft Defender ATP EDR özelliği](/windows/security/threat-protection/microsoft-defender-atp/overview-endpoint-detection-response)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)   

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

## <a name="ns-6-simplify-network-security-rules"></a>NS-6: ağ güvenlik kurallarını basitleştirme

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| NS-6 | 1,5 | IA-4 |

Hizmet etiketlerinden ve uygulama güvenlik gruplarından (ASIG 'ler) yararlanarak ağ güvenlik kurallarını kolaylaştırın. 

Ağ güvenlik gruplarında veya Azure Güvenlik duvarında ağ erişim denetimleri tanımlamak için sanal ağ hizmeti etiketlerini kullanın. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın kaynak veya hedef alanında hizmet etiketi adını belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

Ayrıca, karmaşık güvenlik yapılandırmasını basitleştirmeye yardımcı olması için uygulama güvenlik gruplarını kullanabilirsiniz. Uygulama güvenlik grupları, ağ güvenlik gruplarında açık IP adreslerine göre ilke tanımlamak yerine, bir uygulamanın yapısının doğal bir uzantısı olarak ağ güvenliği yapılandırmanıza olanak tanıyarak, sanal makineleri gruplandırmanızı ve bu gruplara göre ağ güvenlik ilkelerini tanımlamanızı sağlar.

- [Hizmet etiketlerini anlama ve kullanma](../../virtual-network/service-tags-overview.md)

- [Uygulama güvenlik gruplarını anlama ve kullanma](/azure/virtual-network/security-overview#application-security-groups)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)    

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops)

## <a name="ns-7-secure-domain-name-service-dns"></a>NS-7: güvenli etki alanı adı hizmeti (DNS)

| Azure KIMLIĞI | CIS v 7.1 ID 'leri denetler | NıST SP800-53 R4 KIMLIĞI |
|--|--|--|--|
| NS-7 | Yok | SC-20, SC-21 |

DNS güvenliği, DNS yükseltme saldırıları, DNS kirletme ve aldatmacası gibi yaygın saldırılara karşı hafifletmek için DNS güvenliği için en iyi uygulamaları izleyin. 

Yetkili DNS hizmetiniz olarak Azure DNS kullanıldığında, DNS bölgelerinin ve kayıtlarının Azure RBAC ve kaynak kilitleri kullanılarak yanlışlıkla veya kötü amaçlı bir değişiklikten korunmasını sağlayın. 

- [Azure DNS genel bakış](../../dns/dns-overview.md)

- [Güvenli etki alanı adı sistemi (DNS) dağıtım kılavuzu](https://csrc.nist.gov/publications/detail/sp/800-81/2/final)

- [DNS girişlerinin tehlikini önleyin ve alt etki alanı devralmayı önleyin](../fundamentals/subdomain-takeover.md)

**Sorumluluk**: müşteri

**Müşteri güvenlik katılımcıları**:

- [Güvenlik mimarisi](/azure/cloud-adoption-framework/organize/cloud-security-architecture) 

- [Duruş yönetimi](/azure/cloud-adoption-framework/organize/cloud-security-posture-management)   

- [Uygulama güvenliği ve DevOps](/azure/cloud-adoption-framework/organize/cloud-security-application-security-devsecops) 

