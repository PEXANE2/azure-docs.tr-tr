---
title: Azure Güvenlik Duvarı özellikleri
description: Azure Güvenlik duvarı özellikleri hakkında bilgi edinin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 06/18/2020
ms.author: victorh
ms.openlocfilehash: eb7cf1899b24ed225941f0a02040206504e6486b
ms.sourcegitcommit: 971a3a63cf7da95f19808964ea9a2ccb60990f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85095593"
---
# <a name="azure-firewall-features"></a>Azure Güvenlik Duvarı özellikleri

[Azure Güvenlik Duvarı](overview.md) , Azure sanal ağ kaynaklarınızı koruyan, yönetilen ve bulut tabanlı bir ağ güvenlik hizmetidir.

![Güvenlik duvarına genel bakış](media/overview/firewall-threat.png)

Azure Güvenlik Duvarı aşağıdaki özellikleri içerir:

- [Yerleşik yüksek kullanılabilirlik](#built-in-high-availability)
- [Kullanılabilirlik Alanları](#availability-zones)
- [Kısıtlamasız bulut ölçeklenebilirliği](#unrestricted-cloud-scalability)
- [Uygulama FQDN filtreleme kuralları](#application-fqdn-filtering-rules)
- [Ağ trafiği filtreleme kuralları](#network-traffic-filtering-rules)
- [FQDN etiketleri](#fqdn-tags)
- [Hizmet etiketleri](#service-tags)
- [Tehdit bilgileri](#threat-intelligence)
- [Giden SNAT desteği](#outbound-snat-support)
- [Gelen DNAT desteği](#inbound-dnat-support)
- [Birden çok genel IP adresi](#multiple-public-ip-addresses)
- [Azure İzleyici günlükleri](#azure-monitor-logging)
- [Zorlamalı tünel oluşturma](#forced-tunneling)
- [Sertifikalar](#certifications)

## <a name="built-in-high-availability"></a>Yerleşik yüksek kullanılabilirlik

Yüksek kullanılabilirlik yerleşik olarak bulunur, bu nedenle ek yük dengeleyiciler gerekli değildir ve yapılandırmanız gereken bir şey yoktur.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları

Azure Güvenlik Duvarı, daha fazla kullanılabilirlik için dağıtım sırasında birden fazla Kullanılabilirlik Alanları yayılarak yapılandırılabilir. Kullanılabilirlik Alanları, kullanılabilirliği% 99,99 çalışma süresine kadar artar. Daha fazla bilgi için bkz. Azure Güvenlik Duvarı [hizmet düzeyi sözleşmesi (SLA)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/). İki veya daha fazla Kullanılabilirlik Alanları seçildiğinde% 99,99 çalışma süresi SLA 'Sı sunulur.

Ayrıca, Azure Güvenlik duvarını belirli bir bölgeye yalnızca yakınlık nedenleriyle, hizmet standardı% 99,95 SLA ile ilişkilendirebilirsiniz.

Bir kullanılabilirlik alanında dağıtılan bir güvenlik duvarı için ek maliyet yoktur. Ancak, Kullanılabilirlik Alanları ilişkili gelen ve giden veri aktarımları için ek maliyetler de mevcuttur. Daha fazla bilgi için bkz. [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/).

Azure Güvenlik Duvarı Kullanılabilirlik Alanları, Kullanılabilirlik Alanları destekleyen bölgelerde kullanılabilir. Daha fazla bilgi için bkz. [Azure 'da kullanılabilirlik alanları destekleyen bölgeler](../availability-zones/az-region.md)

> [!NOTE]
> Kullanılabilirlik Alanları, yalnızca dağıtım sırasında yapılandırılabilir. Mevcut bir güvenlik duvarını Kullanılabilirlik Alanları içerecek şekilde yapılandıramazsınız.

Kullanılabilirlik Alanları hakkında daha fazla bilgi için bkz. [Azure 'Da bölgeler ve kullanılabilirlik alanları](../availability-zones/az-overview.md)

## <a name="unrestricted-cloud-scalability"></a>Kısıtlamasız bulut ölçeklenebilirliği

Azure Güvenlik Duvarı, değişen ağ trafiği akışlarıyla başa çıkmak için gerek duyduğunuz kadar ölçeklenebilir, bu sayede trafiğinizin en yoğun olduğu durum için bütçe yapmak zorunda kalmazsınız.

## <a name="application-fqdn-filtering-rules"></a>Uygulama FQDN filtreleme kuralları

Giden HTTP/S trafiğini veya Azure SQL trafiğini (Önizleme) joker karakterler dahil olmak üzere, tam etki alanı adları (FQDN) listesiyle sınırlayabilirsiniz. Bu özellik TLS sonlandırmasını gerektirmez.

## <a name="network-traffic-filtering-rules"></a>Ağ trafiği filtreleme kuralları

Ağ filtreleme kurallarını kaynak ve hedef IP adresine, bağlantı noktasına ve protokole göre merkezi olarak oluşturabilir, *izin verebilir* veya *reddedebilirsiniz*. Azure Güvenlik Duvarı tamamen durum bilgisine sahiptir; bu nedenle, farklı türden bağlantıların geçerli paketlerini tanıyabilir. Kurallar, birden çok abonelik ve sanal ağda zorlanır ve günlüğe kaydedilir.

## <a name="fqdn-tags"></a>FQDN etiketleri

[FQDN etiketleri](fqdn-tags.md) , güvenlik duvarınız aracılığıyla Iyi bilinen Azure hizmeti ağ trafiğine izin vermeyi kolaylaştırır. Örneğin Windows Update ağ trafiğine güvenlik duvarınızda izin vermek istediğiniz varsayalım. Bir uygulama kuralı oluşturup Windows Update etiketini eklersiniz. Artık Windows Update’in ağ trafiği, güvenlik duvarınızdan geçebilir.

## <a name="service-tags"></a>Hizmet etiketleri

[Hizmet etiketi](service-tags.md) , güvenlik kuralı oluşturma karmaşıklığını en aza indirmeye yardımcı olmak IÇIN bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz ve bir etiket içinde hangi IP adreslerinin ekleneceğini belirtebilirsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir.

## <a name="threat-intelligence"></a>Tehdit bilgileri

Güvenlik duvarınızın, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen/giden trafiği uyarmasını ve reddetmesi için [tehdit zekası](threat-intel.md)tabanlı filtreleme etkinleştirilebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklıdır.

## <a name="outbound-snat-support"></a>Giden SNAT desteği

Tüm giden sanal ağ trafiği IP adresleri Azure Güvenlik Duvarı genel IP’sine çevrilir (Kaynak Ağ Adresi Çevirisi). Sanal ağınızdan kaynaklanan uzak İnternet hedeflerine yönelik trafiği tanımlayabilir ve buna izin verebilirsiniz. Hedef IP, [ıANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP aralığı olduğunda Azure GÜVENLIK duvarı SNAT değildir. 

Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı, trafiği AzureFirewallSubnet içindeki güvenlik duvarı özel IP adreslerinden birine karşı bir şekilde SNAT olarak kullanır. Azure Güvenlik duvarını, genel IP adresi aralığınızı **SNAT olarak** yapılandırmak için yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Güvenlik DUVARı SNAT özel IP adresi aralıkları](snat-private-range.md).

## <a name="inbound-dnat-support"></a>Gelen DNAT desteği

Güvenlik duvarınızın genel IP adresine gelen Internet ağ trafiği çevrilir (hedef ağ adresi çevirisi) ve sanal ağlarınızdaki özel IP adreslerine filtrelenir.

## <a name="multiple-public-ip-addresses"></a>Birden çok genel IP adresi

Güvenlik duvarınızla [birden çok genel IP adresini](deploy-multi-public-ip-powershell.md) (250 'e kadar) ilişkilendirebilirsiniz.

Bu, aşağıdaki senaryolara izin vermez:

- **DNAT** -birden çok standart bağlantı noktası örneğini arka uç sunucularınıza çevirebilirsiniz. Örneğin, iki genel IP adresiniz varsa, TCP bağlantı noktası 3389 ' ü (RDP) her iki IP adresi için de çevirebilirsiniz.
- **SNAT** -giden SNAT bağlantıları için ek bağlantı noktaları kullanılabilir ve bu, SNAT bağlantı noktası tükenmesi potansiyelini azaltır. Azure Güvenlik Duvarı şu anda bir bağlantı için kullanılacak kaynak genel IP adresini rastgele seçer. Ağınızda herhangi bir aşağı akış filtresi varsa, güvenlik duvarınızdan ilişkili tüm genel IP adreslerine izin vermeniz gerekir. Bu yapılandırmayı basitleştirmek için bir [genel IP adresi ön eki](../virtual-network/public-ip-address-prefix.md) kullanmayı düşünün.

## <a name="azure-monitor-logging"></a>Azure İzleyici günlükleri

Tüm olaylar Azure Izleyici ile tümleşiktir ve günlükleri bir depolama hesabında arşivlemenize, Olay Hub 'ınıza olayları akışlarınıza veya Azure Izleyici günlüklerine gönderebilmenizi sağlar. Daha fazla bilgi için bkz. [öğretici: Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme](tutorial-diagnostics.md).

## <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

Azure Güvenlik Duvarı 'nı, Internet 'e yönelik tüm trafiği doğrudan Internet 'e gitmek yerine belirlenen bir sonraki atlamaya yönlendirmek üzere yapılandırabilirsiniz. Örneğin, Internet 'e geçirilmeden önce ağ trafiğini işlemek için şirket içi bir uç güvenlik duvarına veya diğer ağ sanal gerecine (NVA) sahip olabilirsiniz. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı Zorlamalı tünel](forced-tunneling.md).

## <a name="certifications"></a>Sertifikalar

Azure Güvenlik Duvarı, ödeme kartı sektör (PCI), hizmet organizasyonu denetimleri (SOC), Uluslararası Standardizasyon Teşkilatı (ISO) ve ıCSA Labs ile uyumludur. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı uyumluluk sertifikaları](compliance-certifications.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı kural işleme mantığı](rule-processing.md)