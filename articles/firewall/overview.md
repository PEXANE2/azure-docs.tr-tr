---
title: Azure Güvenlik Duvarı nedir?
description: Azure Güvenlik duvarı özellikleri hakkında bilgi edinin.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 10/08/2019
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: 5be1a673ac456b0896ce83afcb469e4ac6b8b40a
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001333"
---
# <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

Azure Güvenlik Duvarı, Azure sanal ağ kaynaklarınızı koruyan, yönetilen, bulut tabanlı bir ağ güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe sahip ve Kısıtlanmamış bulut ölçeklenebilirliğine sahip bir hizmet olarak tam durum bilgisi olmayan bir güvenlik duvarıdır.

![Güvenlik duvarına genel bakış](media/overview/firewall-threat.png)

Abonelikler ve sanal ağlar arasında merkezi olarak uygulama ve ağ bağlantısı ilkeleri oluşturabilir, uygulayabilir ve bunları günlüğe kaydedebilirsiniz. Azure Güvenlik Duvarı, sanal ağınızdan kaynaklanan trafiği belirlemek için güvenlik duvarlarının dışında, sanal ağ kaynaklarınız için statik bir genel IP adresi kullanır.  Hizmet, günlüğe kaydetme ve analiz için Azure Izleyici ile tamamen tümleşiktir.

Azure Güvenlik Duvarı aşağıdaki özellikleri sunar:

## <a name="built-in-high-availability"></a>Yerleşik yüksek kullanılabilirlik

Yüksek kullanılabilirlik yerleşik olarak bulunur, bu nedenle ek yük dengeleyiciler gerekli değildir ve yapılandırmanız gereken bir şey yoktur.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları

Azure Güvenlik Duvarı, daha fazla kullanılabilirlik için dağıtım sırasında birden fazla Kullanılabilirlik Alanları yayılarak yapılandırılabilir. Kullanılabilirlik Alanları, kullanılabilirliği% 99,99 çalışma süresine kadar artar. Daha fazla bilgi için bkz. Azure Güvenlik Duvarı [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/). İki veya daha fazla Kullanılabilirlik Alanları seçildiğinde% 99,99 çalışma süresi SLA 'Sı sunulur.

Ayrıca, Azure Güvenlik duvarını belirli bir bölgeye yalnızca yakınlık nedenleriyle, hizmet standardı% 99,95 SLA ile ilişkilendirebilirsiniz.

Bir kullanılabilirlik alanında dağıtılan bir güvenlik duvarı için ek maliyet yoktur. Ancak, Kullanılabilirlik Alanları ilişkili gelen ve giden veri aktarımları için ek maliyetler de mevcuttur. Daha fazla bilgi için bkz. [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/).

Azure Güvenlik Duvarı Kullanılabilirlik Alanları, Kullanılabilirlik Alanları destekleyen bölgelerde kullanılabilir. Daha fazla bilgi için bkz. [Azure 'da kullanılabilirlik alanları nedir?](../availability-zones/az-overview.md#services-support-by-region)

> [!NOTE]
> Kullanılabilirlik Alanları, yalnızca dağıtım sırasında yapılandırılabilir. Mevcut bir güvenlik duvarını Kullanılabilirlik Alanları içerecek şekilde yapılandıramazsınız.

Kullanılabilirlik Alanları hakkında daha fazla bilgi için bkz. [Azure 'da kullanılabilirlik alanları nedir?](../availability-zones/az-overview.md)

## <a name="unrestricted-cloud-scalability"></a>Kısıtlanmamış bulut ölçeklenebilirliği

Azure Güvenlik Duvarı, değişen ağ trafiği akışlarını karşılamak için ihtiyaç duyduğunuz kadar ölçeklenebilir, böylece tepe trafiğiniz için bütçe oluşturmanız gerekmez.

## <a name="application-fqdn-filtering-rules"></a>Uygulama FQDN 'SI filtreleme kuralları

Giden HTTP/S trafiğini veya Azure SQL trafiğini (Önizleme) joker karakterler dahil olmak üzere, tam etki alanı adları (FQDN) listesiyle sınırlayabilirsiniz. Bu özellik SSL sonlandırması gerektirmez.

## <a name="network-traffic-filtering-rules"></a>Ağ trafiği Filtreleme kuralları

Kaynak ve hedef IP adresi, bağlantı noktası ve protokole göre merkezi olarak *izin ver* veya *Reddet* ağ filtreleme kuralları oluşturabilirsiniz. Azure Güvenlik Duvarı tam olarak durum bilgisi olduğundan, farklı bağlantı türleri için meşru paketleri ayırabilirler. Kurallar, birden fazla aboneliğe ve sanal ağa uygulanır ve günlüğe kaydedilir.

## <a name="fqdn-tags"></a>FQDN etiketleri

FQDN etiketleri, güvenlik duvarınız aracılığıyla iyi bilinen Azure hizmeti ağ trafiğine izin vermeyi kolaylaştırır. Örneğin, güvenlik duvarınız aracılığıyla Windows Update ağ trafiğine izin vermek istediğinizi varsayalım. Bir uygulama kuralı oluşturup Windows Update etiketini dahil edersiniz. Artık Windows Update ağ trafiği güvenlik duvarınız üzerinden akabilir.

## <a name="service-tags"></a>Hizmet etiketleri

Hizmet etiketi, güvenlik kuralı oluşturma karmaşıklığını en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz ve bir etiket içinde hangi IP adreslerinin ekleneceğini belirtebilirsiniz. Microsoft, hizmet etiketiyle çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir.

## <a name="threat-intelligence"></a>Tehdit bilgileri

Güvenlik duvarınızın, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen/giden trafiği uyarmasını ve reddetmesi için tehdit zekası tabanlı filtreleme etkinleştirilebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklıdır.

## <a name="outbound-snat-support"></a>Giden SNAT desteği

Tüm giden sanal ağ trafiği IP adresleri, Azure Güvenlik Duvarı genel IP 'ye çevrilir (kaynak ağ adresi çevirisi). Sanal ağınızdan gelen trafiği uzak Internet hedeflerine tanımlayabilir ve bunlara izin verebilirsiniz. Hedef IP, [ıANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP aralığı olduğunda Azure GÜVENLIK duvarı SNAT değildir. Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı, trafiği AzureFirewallSubnet içindeki güvenlik duvarı özel IP adreslerinden birine karşı bir şekilde SNAT olarak kullanır.

## <a name="inbound-dnat-support"></a>Gelen DNAT desteği

Güvenlik duvarınızın genel IP adresine gelen ağ trafiği çevrilir (hedef ağ adresi çevirisi) ve sanal ağlarınızdaki özel IP adreslerine filtrelenir.

## <a name="multiple-public-ip-addresses"></a>Birden çok genel IP adresi

Güvenlik duvarınızla birden çok genel IP adresini (100 'e kadar) ilişkilendirebilirsiniz.

Bu, aşağıdaki senaryolara izin vermez:

- **DNAT** -birden çok standart bağlantı noktası örneğini arka uç sunucularınıza çevirebilirsiniz. Örneğin, iki genel IP adresiniz varsa, TCP bağlantı noktası 3389 ' ü (RDP) her iki IP adresi için de çevirebilirsiniz.
- **SNAT** -giden SNAT bağlantıları için ek bağlantı noktaları kullanılabilir ve bu, SNAT bağlantı noktası tükenmesi potansiyelini azaltır. Azure Güvenlik Duvarı şu anda bir bağlantı için kullanılacak kaynak genel IP adresini rastgele seçer. Ağınızda herhangi bir aşağı akış filtresi varsa, güvenlik duvarınızdan ilişkili tüm genel IP adreslerine izin vermeniz gerekir.

## <a name="azure-monitor-logging"></a>Azure Izleyici günlüğü

Tüm olaylar Azure Izleyici ile tümleşiktir ve günlükleri bir depolama hesabında arşivlemenize, Olay Hub 'ınıza olayları akışlarınıza veya Azure Izleyici günlüklerine gönderebilmenizi sağlar.

## <a name="pci-soc-and-iso-compliant"></a>PCI, SOC ve ISO uyumlu

Azure Güvenlik Duvarı, ödeme kartı sektör (PCI), hizmet kuruluşu denetimleri (SOC) ve Uluslararası Standardizasyon Teşkilatı (ISO) ile uyumludur. Şu anda SOC 1 tür 2, SOC 2 tür 2, SOC 3, PCI DSS ve ISO 27001, 27018, 20000-1, 22301, 9001, 27017 desteklemektedir.

Daha fazla bilgi için bkz. [Microsoft Uyumluluk Kılavuzu](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide).

## <a name="known-issues"></a>Bilinen sorunlar

Azure Güvenlik Duvarı aşağıdaki bilinen sorunlara sahiptir:

|Sorun  |Açıklama  |Azaltma  |
|---------|---------|---------|
TCP/UDP olmayan protokoller için ağ filtreleme kuralları (örneğin, ıCMP) Internet 'e göre bağlantılı trafik için çalışmıyor|TCP/UDP olmayan protokoller için ağ filtreleme kuralları, SNAT ile genel IP adresiniz arasında çalışmaz. TCP olmayan/UDP protokolleri, bağlı ağlar ve sanal ağlar arasında desteklenir.|Azure Güvenlik Duvarı, [günümüzde IP protokolleri IÇIN SNAT 'yi desteklemeyen](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#limitations)standart Load Balancer kullanır. Gelecekteki bir sürümde bu senaryoyu desteklemeye yönelik seçenekleri araştırıyoruz.|
|ICMP için eksik PowerShell ve CLı desteği|Azure PowerShell ve CLı, ağ kurallarında geçerli bir protokol olarak ıCMP 'yi desteklemez.|Portal ve REST API aracılığıyla bir protokol olarak ıCMP kullanmak yine de mümkündür. PowerShell ve CLı için yakında ıCMP eklemek üzere çalışıyoruz.|
|FQDN etiketleri protokol gerektiriyor: bağlantı noktası ayarlanacak|FQDN etiketleriyle uygulama kuralları için bağlantı noktası: protokol tanımı gerekir.|**Https** 'yi bağlantı noktası: protokol değeri olarak kullanabilirsiniz. FQDN etiketleri kullanıldığında bu alanı isteğe bağlı hale getirmek için çalışıyoruz.|
|Bir güvenlik duvarını farklı bir kaynak grubuna veya aboneliğe taşıma desteklenmiyor|Bir güvenlik duvarının farklı bir kaynak grubuna veya aboneliğe taşınması desteklenmez.|Bu işlevi desteklemek, yol haritamız üzerinde. Bir güvenlik duvarını farklı bir kaynak grubuna veya aboneliğe taşımak için, geçerli örneği silip yeni kaynak grubunda veya abonelikte yeniden oluşturmanız gerekir.|
|Ağ ve uygulama kurallarında bağlantı noktası aralığı|Bağlantı noktaları, yönetim ve sistem durumu araştırmaları için ayrıldığından yüksek bağlantı noktaları 64.000 ile sınırlıdır. |Bu sınırlamayı rahat hale getiriyoruz.|
|Tehdit bilgileri uyarıları maskeli olabilir|Giden filtreleme maskeleri için hedef 80/443 olan ağ kuralları yalnızca uyarı moduna yapılandırıldığında tehdit bilgileri uyarılarını arar.|Uygulama kurallarını kullanarak 80/443 için giden filtreleme oluşturun. Ya da tehdit zekası modunu **uyarı ve reddetme**olarak değiştirin.|
|Azure Güvenlik Duvarı yalnızca ad çözümlemesi için Azure DNS kullanır|Azure Güvenlik Duvarı yalnızca Azure DNS kullanarak FQDN 'leri çözer. Özel bir DNS sunucusu desteklenmez. Diğer alt ağlarda DNS çözümlemesi üzerinde hiçbir etkisi yoktur.|Bu sınırlamayı rahat hale getiriyoruz.|
|Azure Güvenlik Duvarı SNAT/DNAT özel IP hedefleri için çalışmıyor|Azure Güvenlik Duvarı SNAT/DNAT desteği Internet çıkış/giriş ile sınırlıdır. SNAT/DNAT Şu anda özel IP hedefleri için çalışmıyor. Örneğin, bağlı bileşene bağlı olarak.|Bu geçerli bir kısıtlamadır.|
|İlk genel IP yapılandırması kaldırılamıyor|Her bir Azure Güvenlik Duvarı genel IP adresi bir *IP yapılandırmasına*atanır.  İlk IP yapılandırması, güvenlik duvarı dağıtımı sırasında atanır ve genellikle güvenlik duvarı alt ağına (bir şablon dağıtımı aracılığıyla açıkça farklı şekilde yapılandırılmamışsa) bir başvuru içerir. Güvenlik duvarını serbest bırakacağından bu IP yapılandırmasını silemezsiniz. Güvenlik duvarının kullanılabilir en az bir genel IP adresi varsa, bu IP yapılandırmasıyla ilişkili genel IP adresini değiştirmeye veya kaldırmaya devam edebilirsiniz.|Bu tasarım gereğidir.|
|Kullanılabilirlik alanları yalnızca dağıtım sırasında yapılandırılabilir.|Kullanılabilirlik alanları yalnızca dağıtım sırasında yapılandırılabilir. Bir güvenlik duvarı dağıtıldıktan sonra Kullanılabilirlik Alanları yapılandıramazsınız.|Bu tasarım gereğidir.|
|Gelen bağlantılarda SNAT|DNAT 'nin yanı sıra, güvenlik duvarı genel IP adresi (gelen) ile kurulan bağlantılar, güvenlik duvarı özel IP 'lerinden birine karşı denetlenir. Simetrik yönlendirmeyi sağlamak için bugün bu gereksinim (etkin/etkin NVA 'lar için de).|HTTP/S için özgün kaynağı korumak üzere [XFF](https://en.wikipedia.org/wiki/X-Forwarded-For) üst bilgilerini kullanmayı göz önünde bulundurun. Örneğin, güvenlik duvarının önünde [Azure ön kapısı](../frontdoor/front-door-http-headers-protocol.md#front-door-service-to-backend) gibi bir hizmet kullanın. Ayrıca, Azure ön kapısının parçası olarak WAF 'yi ve güvenlik duvarını de zincirde ekleyebilirsiniz.
|SQL FQDN filtrelemesi yalnızca Proxy modunda desteklenir (bağlantı noktası 1433)|Azure SQL veritabanı, Azure SQL veri ambarı ve Azure SQL yönetilen örneği için:<br><br>Önizleme sırasında SQL FQDN filtrelemesi yalnızca Proxy modunda desteklenir (bağlantı noktası 1433).<br><br>Azure SQL IaaS için:<br><br>Standart olmayan bağlantı noktaları kullanıyorsanız, bu bağlantı noktalarını uygulama kurallarında belirtebilirsiniz.|Azure içinden bağlanılıyorsa varsayılan olan yeniden yönlendirme modunda SQL için, Azure Güvenlik Duvarı ağ kurallarının bir parçası olarak SQL hizmeti etiketini kullanarak erişimi filtreleyebilirsiniz.
|TCP bağlantı noktası 25 ' i giden trafiğe izin verilmez| TCP bağlantı noktası 25 kullanan giden SMTP bağlantıları engellenir. Bağlantı noktası 25 öncelikle kimliği doğrulanmamış e-posta teslimi için kullanılır. Bu, sanal makineler için varsayılan platform davranışıdır. Daha fazla bilgi için bkz. [Azure 'da gıden SMTP bağlantısı sorunlarını giderme](../virtual-network/troubleshoot-outbound-smtp-connectivity.md). Ancak, sanal makinelerden farklı olarak, Azure Güvenlik duvarında bu işlevselliği etkinleştirmek mümkün değildir.|SMTP sorun giderme makalesinde belgelenen şekilde e-posta göndermek için önerilen yöntemi izleyin. Alternatif olarak, giden SMTP erişimine ihtiyacı olan sanal makineyi güvenlik duvarınızdan çıkarın ve bunun yerine doğrudan Internet 'e giden erişimi yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portal kullanarak Azure Güvenlik duvarını dağıtma ve yapılandırma](tutorial-firewall-deploy-portal.md)
- [Şablon kullanarak Azure Güvenlik Duvarı dağıtma](deploy-template.md)
- [Azure Güvenlik Duvarı test ortamı oluşturma](scripts/sample-create-firewall-test.md)
