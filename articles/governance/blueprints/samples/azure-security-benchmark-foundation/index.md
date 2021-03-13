---
title: Azure Güvenlik kıyaslaması temel şeması örnek genel bakış
description: Genel bakış ve Azure Security kıyaslama Foundation şema örneği mimarisi.
ms.date: 03/12/2021
ms.topic: sample
ms.openlocfilehash: 9630915328b430c409c48e13e0d22f64dbcc99ea
ms.sourcegitcommit: ec39209c5cbef28ade0badfffe59665631611199
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2021
ms.locfileid: "103232078"
---
# <a name="overview-of-the-azure-security-benchmark-foundation-blueprint-sample"></a>Azure Security kıyaslama Foundation şema örneğine genel bakış

Azure Güvenlik kıyaslaması temel şeması örneği, güvenli ve uyumlu bir Azure ortamı oluşturmanıza yardımcı olacak bir dizi temel altyapı deseni sağlar. Şema, acalacaklı veya uyumluluk gereksinimlerine sahip senaryolara çözüm sunan bulut tabanlı bir mimari dağıtmanıza yardımcı olur. Bu temel şema örneği, [Azure Güvenlik kıyaslaması örnek şema](../azure-security-benchmark.md)'in bir uzantısıdır. Ağ sınırlarını, izlemeyi ve diğer kaynakları, [Azure Güvenlik kıyaslamasında](../../../../security/benchmarks/index.yml)tanımlanan ilkeler ve diğer guardraylarla birlikte dağıtır ve yapılandırır.

## <a name="architecture"></a>Mimari

Bu şema örneği tarafından oluşturulan temel ortam, bir [hub ve bağlı bileşen modelinin](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)mimari ilkelerine dayanır.
Şema, ek/isteğe bağlı yönetim, bakım, yönetim ve bağlantı altyapısını barındırmak üzere bağlantı, yönetim ve bağlantı kutusu alt ağları için Azure savunma, ağ geçidi ve güvenlik duvarı gibi ortak ve paylaşılan kaynakları, hizmetleri ve yapıları içeren bir hub sanal ağı dağıtır. Bir veya daha fazla bağlı bileşen sanal ağı, Web ve veritabanı hizmetleri gibi ana uygulama iş yüklerine dağıtılır. Bağlı bileşen sanal ağları, sorunsuz ve güvenli bağlantı için Azure sanal ağ eşlemesi kullanılarak hub sanal ağına bağlanır. Örnek şema yeniden atayarak veya el ile bir Azure sanal ağı oluşturarak ve bunu hub sanal ağıyla eşleyerek ek bağlı bileşen eklenebilir. Bağlı olan sanal ağlara ve alt ağlara yönelik tüm dış bağlantılar, hub sanal ağı ve güvenlik duvarı, ağ geçidi ve yönetim bağlantı kutuları aracılığıyla yönlendirmek üzere yapılandırılmıştır.

:::image type="content" source="../../media/azure-security-benchmark-foundation/architecture.png" alt-text="Azure Güvenlik kıyaslaması temel şeması örnek mimari diyagramı" border="false":::

Bu şema, güvenli, izlenen, kurumsal özellikli bir temel sağlamak için çeşitli Azure hizmetleri dağıtır. Bu ortam şunlardan oluşur:

- Kaynak günlüklerinin, etkinlik günlüklerinin, ölçümlerin ve ağların trafik akışlarının, kolay sorgulama, analiz, arşivleme ve uyarı için merkezi bir konumda depolandığından emin olmak için [Azure Izleyici günlükleri](../../../../azure-monitor/logs/data-platform-logs.md) ve bir Azure depolama hesabı.
- Azure kaynakları için tehdit koruması sağlamak üzere [Azure Güvenlik Merkezi](../../../../security-center/security-center-introduction.md) (Standart sürüm).
- Hub 'daki [Azure sanal ağı](../../../../virtual-network/virtual-networks-overview.md) , bir şirket içi ağa bağlantı için alt ağları, Internet bağlantısı için//için bir giriş ve çıkış yığınını ve ek yönetim veya yönetim hizmetlerinin dağıtılması için isteğe bağlı alt ağları destekler. Sanal Ağa gelen bağlı bileşen, uygulama iş yüklerini barındırmak için alt ağlar içerir. İlgili senaryoları desteklemek için gerektiğinde dağıtımdan sonra ek alt ağlar oluşturulabilir.
- Tüm giden internet trafiğini yönlendirmek ve gelen internet trafiğini geçiş kutusu aracılığıyla etkinleştirmek için [Azure Güvenlik Duvarı](../../../../firewall/overview.md) . (Varsayılan güvenlik duvarı kuralları, tüm internet gelen ve giden trafiği engelle ve kural, varsa dağıtımdan sonra yapılandırılması gerekir.)
- Tüm internet gelen ve giden trafiği engelleyecek şekilde yapılandırılan tüm alt ağlara (örneğin, Azure savunma, ağ geçidi ve Azure Güvenlik Duvarı) atanmış [ağ güvenlik grupları](../../../../virtual-network/network-security-group-how-it-works.md) (NSG 'ler).
- Azure sanal makinelerinin gruplanmasına olanak tanımak için [uygulama güvenlik grupları](../../../../virtual-network/application-security-groups.md) ortak ağ güvenlik ilkelerini uygular.
- Tüm giden internet trafiğini, güvenlik duvarı üzerinden alt ağlardan yönlendirmek için [Rota tabloları](../../../../virtual-network/manage-route-table.md) . (Azure Güvenlik Duvarı ve NSG kurallarının, bağlantıyı açmak için dağıtımdan sonra yapılandırılması gerekir.)
- Azure sanal ağındaki kaynakların ölçümlerini izlemek, tanılamak ve görüntülemek için [Azure Ağ İzleyicisi](../../../../network-watcher/network-watcher-monitoring-overview.md) .
- DDoS saldırılarına karşı Azure kaynaklarını korumak için [Azure DDoS koruma standardı](../../../../ddos-protection/ddos-protection-overview.md) .
- Genel IP adresi, aracı veya özel istemci yazılımı gerektirmeyen bir sanal makineye sorunsuz ve güvenli bağlantı sağlamak için [Azure](../../../../bastion/bastion-overview.md) savunma.
- Azure [VPN Gateway](../../../../vpn-gateway/vpn-gateway-about-vpngateways.md) , bir Azure sanal ağı ile şirket içi bir konum arasında ortak Internet üzerinden şifrelenmiş trafik etkinleştirmek için.

> [!NOTE] 
> Azure Güvenlik kıyaslama altyapısı, iş yükleri için temel bir mimari yerleştirir. Yukarıdaki mimari diyagramı, alt ağların potansiyel kullanımını göstermek için çeşitli önemli kaynaklar içerir. Yine de bu temel mimaride iş yüklerini dağıtmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Security kıyaslama Foundation şema örneğinin genel bakış ve mimarisini gözden geçirdiniz.

> [!div class="nextstepaction"]
> [Azure Güvenlik kıyaslama Foundation şema-Deploy adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.