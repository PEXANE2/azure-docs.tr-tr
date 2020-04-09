---
title: Azure Güvenlik Duvarı nedir?
description: Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 04/08/2020
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: 60d936d9c2785e4723cdc09e55927fe13af8d8a1
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80892317"
---
# <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

![ICSA belgelendirmesi](media/overview/icsa-cert-firewall-small.png)

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe ve sınırsız bulut ölçeklenebilirliğine sahip, tam durum bilgisi olan bir hizmet olarak güvenlik duvarıdır.

![Güvenlik duvarına genel bakış](media/overview/firewall-threat.png)

Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz. Azure Güvenlik Duvarı, dış güvenlik duvarlarının sanal ağınızdan kaynaklanan trafiği tanımlamasına olanak tanımak amacıyla sanal ağ kaynaklarınız için statik genel bir IP adresi kullanır.  Hizmet, günlük ve analiz için Azure İzleyici ile tamamen tümleşik çalışır.

Azure Güvenlik Duvarı şu özellikleri sunar:

## <a name="built-in-high-availability"></a>Yerleşik yüksek kullanılabilirlik

Yüksek kullanılabilirlik yerleşiktir, bu nedenle ek yük dengeleyicileri gerekmez ve yapılandırmanız gereken hiçbir şey yoktur.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları

Azure Güvenlik Duvarı, daha fazla kullanılabilirlik için birden çok Kullanılabilirlik Bölgesine yayılacak şekilde dağıtım sırasında yapılandırılabilir. Kullanılabilirlik Bölgeleri ile kullanılabilirliğiniz %99,99'a yükselir. Daha fazla bilgi için Azure Güvenlik Duvarı [Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/azure-firewall/v1_0/)bakın. İki veya daha fazla Kullanılabilirlik Bölgesi seçildiğinde %99,99 çalışma süresi SLA sunulur.

Ayrıca, hizmet standardını %99,95 SLA kullanarak Azure Güvenlik Duvarını sadece yakınlık nedenleriyle belirli bir bölgeyle ilişkilendirebilirsiniz.

Kullanılabilirlik Bölgesinde dağıtılan bir güvenlik duvarı için ek bir ücret yoktur. Ancak, Kullanılabilirlik Bölgeleri ile ilişkili gelen ve giden veri aktarımları için ek maliyetler vardır. Daha fazla bilgi için [Bant Genişliği fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/bandwidth/)bakın.

Azure Güvenlik Duvarı Kullanılabilirlik Bölgeleri, Kullanılabilirlik Bölgelerini destekleyen bölgelerde kullanılabilir. Daha fazla bilgi için [Azure'daki Kullanılabilirlik Bölgeleri nelerdir?](../availability-zones/az-overview.md#services-support-by-region)

> [!NOTE]
> Kullanılabilirlik Bölgeleri yalnızca dağıtım sırasında yapılandırılabilir. Varolan bir güvenlik duvarını Kullanılabilirlik Bölgelerini içerecek şekilde yapılandıramazsınız.

Kullanılabilirlik Bölgeleri hakkında daha fazla bilgi için [Azure'daki Kullanılabilirlik Bölgeleri nelerdir?](../availability-zones/az-overview.md)

## <a name="unrestricted-cloud-scalability"></a>Kısıtlamasız bulut ölçeklenebilirliği

Azure Güvenlik Duvarı, değişen ağ trafiği akışlarıyla başa çıkmak için gerek duyduğunuz kadar ölçeklenebilir, bu sayede trafiğinizin en yoğun olduğu durum için bütçe yapmak zorunda kalmazsınız.

## <a name="application-fqdn-filtering-rules"></a>Uygulama FQDN filtreleme kuralları

Giden HTTP/S trafiğini veya Azure SQL trafiğini (önizleme) joker kartlar da dahil olmak üzere tam nitelikli alan adları (FQDN) belirli bir listeyle sınırlandırabilirsiniz. Bu özellik SSL sonlandırma gerektirmez.

## <a name="network-traffic-filtering-rules"></a>Ağ trafiği filtreleme kuralları

Ağ filtreleme kurallarını kaynak ve hedef IP adresine, bağlantı noktasına ve protokole göre merkezi olarak oluşturabilir, *izin verebilir* veya *reddedebilirsiniz*. Azure Güvenlik Duvarı tamamen durum bilgisine sahiptir; bu nedenle, farklı türden bağlantıların geçerli paketlerini tanıyabilir. Kurallar, birden çok abonelik ve sanal ağda zorlanır ve günlüğe kaydedilir.

## <a name="fqdn-tags"></a>FQDN etiketleri

FQDN etiketleri, güvenlik duvarınızda tanınmış Azure hizmet ağı trafiğine izin vermenizi kolaylaştırır. Örneğin Windows Update ağ trafiğine güvenlik duvarınızda izin vermek istediğiniz varsayalım. Bir uygulama kuralı oluşturup Windows Update etiketini eklersiniz. Artık Windows Update’in ağ trafiği, güvenlik duvarınızdan geçebilir.

## <a name="service-tags"></a>Hizmet etiketleri

Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz veya etikette hangi IP adreslerinin yer alan olduğunu belirtemezsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir.

## <a name="threat-intelligence"></a>Tehdit bilgileri

Tehdit istihbaratı tabanlı filtreleme, güvenlik duvarınızın bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen trafiği uyarması ve reddetmesi için etkinleştirilebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklanmaktadır.

## <a name="outbound-snat-support"></a>Giden SNAT desteği

Tüm giden sanal ağ trafiği IP adresleri Azure Güvenlik Duvarı genel IP’sine çevrilir (Kaynak Ağ Adresi Çevirisi). Sanal ağınızdan kaynaklanan uzak İnternet hedeflerine yönelik trafiği tanımlayabilir ve buna izin verebilirsiniz. Azure Güvenlik Duvarı, hedef IP [IANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP aralığı olduğunda SNAT değildir. 

Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı trafiği AzureFirewallSubnet'teki güvenlik duvarı özel IP adreslerinden birine snat eder. Azure Güvenlik Duvarı'nı ortak IP adres aralığınızı SNAT **olarak değil** olarak yapılandırabilirsiniz. Daha fazla bilgi için Azure [Güvenlik Duvarı SNAT özel IP adres aralıklarına](snat-private-range.md)bakın.

## <a name="inbound-dnat-support"></a>Gelen DNAT desteği

Güvenlik duvarı genel IP adresinize gelen Internet ağ trafiği çevrilmiş (Hedef Ağ Adresi Çevirisi) ve sanal ağlarınızdaki özel IP adreslerine filtrelenir.

## <a name="multiple-public-ip-addresses"></a>Birden çok genel IP adresi

Birden çok genel IP adresini (en fazla 100) güvenlik duvarınızla ilişkilendirebilirsiniz.

Bu, aşağıdaki senaryoları sağlar:

- **DNAT** - Birden çok standart bağlantı noktası örneğini arka uç sunucularınıza çevirebilirsiniz. Örneğin, iki genel IP adresiniz varsa, her iki IP adresi için de TCP bağlantı noktası 3389'u (RDP) çevirebilirsiniz.
- **SNAT** - Giden SNAT bağlantıları için ek bağlantı noktaları kullanılabilir ve bu da SNAT bağlantı noktası tükenmesi potansiyelini azaltır. Şu anda, Azure Güvenlik Duvarı bağlantı için kullanılacak kaynak genel IP adresini rasgele seçer. Ağınızda herhangi bir alt akış filtresi varsa, güvenlik duvarınızla ilişkili tüm genel IP adreslerine izin verebilirsiniz.

## <a name="azure-monitor-logging"></a>Azure İzleyici günlükleri

Tüm etkinlikler Azure Monitor ile entegre edilerek günlükleri bir depolama hesabında arşivlemenize, olayları Etkinlik Hub'ınıza aktarmanıza veya Azure Monitor günlüklerine göndermenize olanak tanır.

## <a name="certifications"></a>Sertifikalar

Azure Güvenlik Duvarı, Ödeme Kartı Endüstrisi (PCI), Hizmet Organizasyonu Denetimleri (SOC), Uluslararası Standartlaştırma Örgütü (ISO) ve ICSA Labs uyumludur. Daha fazla bilgi için Azure [Güvenlik Duvarı uyumluluk sertifikalarına](compliance-certifications.md)bakın.


## <a name="known-issues"></a>Bilinen sorunlar

Azure Güvenlik Duvarındaki bilinen sorunlar şunlardır:

|Sorun  |Açıklama  |Risk azaltma  |
|---------|---------|---------|
TCP/UDP dışı protokollere (örneğin ICMP) yönelik ağ filtreleme kuralları İnternet'e bağlı trafik için çalışmaz|TCP/UDP dışı protokoller için ağ filtreleme kuralları, ortak IP adresinizde SNAT ile çalışmaz. TCP/UDP dışı protokoller, uç alt ağlarla sanal ağlar arasında desteklenir.|Azure Güvenlik Duvarı, [bugün IP protokolleri için SNAT desteği olmayan](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) Standart Load Balancer kullanır. Bu senaryoyu gelecekteki bir sürümde destekleyecek seçenekleri araştırıyoruz.|
|ICMP için eksik PowerShell ve CLI desteği|Azure PowerShell ve CLI, AĞ kurallarında geçerli bir protokol olarak ICMP'yi desteklemez.|Yine de portal ve REST API üzerinden bir protokol olarak ICMP kullanmak mümkündür. Yakında PowerShell ve CLI ICMP eklemek için çalışıyoruz.|
|FQDN etiketleri bir protokol: bağlantı noktası ayarlamayı gerektirir|FQDN etiketleri ile uygulama kuralları bağlantı noktası gerektirir: protokol tanımı.|Bağlantı noktası:protokol değeri olarak **https** kullanabilirsiniz. FQDN etiketleri kullanıldığında bu alanı isteğe bağlı hale getirmek için çalışıyoruz.|
|Güvenlik duvarının farklı bir kaynak grubuna veya aboneliğe taşınmış|Güvenlik duvarının farklı bir kaynak grubuna veya aboneliğe taşınmıştırılırılırılırğ|Bu işlevselliği desteklemek yol haritamızda yer almaktadır. Bir güvenlik duvarını başka bir kaynak grubuna veya aboneliğe taşımak için geçerli örneği silmeniz ve yeni kaynak grubunda veya abonelikte yeniden oluşturmanız gerekir.|
|Tehdit istihbaratı uyarıları maskelenebilir|Giden filtreleme maskeleri için hedef 80/443 ile ağ kuralları, yalnızca modu uyarmak için yapılandırıldığında tehdit istihbarat uyarıları.|Uygulama kurallarını kullanarak 80/443 için giden filtreleme oluşturun. Veya tehdit istihbarat modunu Uyarı ve Reddet olarak **değiştirin.**|
|Azure Güvenlik Duvarı yalnızca ad çözümlemesi için Azure DNS kullanır|Azure Güvenlik Duvarı, Yalnızca Azure DNS kullanarak FQDN'leri çözer. Özel bir DNS sunucusu desteklenmez. DNS çözünürlüğü üzerinde diğer alt ağlar üzerinde hiçbir etkisi yoktur.|Bu sınırlamayı gevşetmek için çalışıyoruz.|
|Azure Güvenlik Duvarı SNAT/DNAT özel IP hedefleri için çalışmıyor|Azure Güvenlik Duvarı SNAT/DNAT desteği Internet çıkış/giriş ile sınırlıdır. SNAT/DNAT şu anda özel IP hedefleri için çalışmıyor. Örneğin, konuştu.|Bu geçerli bir sınırlamadır.|
|İlk genel IP yapılandırması kaldıramıyor|Her Azure Güvenlik Duvarı genel IP adresi bir *IP yapılandırmasına*atanır.  İlk IP yapılandırması güvenlik duvarı dağıtımı sırasında atanır ve genellikle güvenlik duvarı alt ağına bir başvuru da içerir (şablon dağıtımı yoluyla açıkça farklı yapılandırıldıkça). Güvenlik duvarının ayrılmasını yapacağından bu IP yapılandırmasını silemezsiniz. Güvenlik duvarının kullanabileceği en az bir genel IP adresi varsa, bu IP yapılandırmasıyla ilişkili genel IP adresini değiştirmeye veya kaldırmaya devam edebilirsiniz.|Bu tasarım gereğidir.|
|Kullanılabilirlik bölgeleri yalnızca dağıtım sırasında yapılandırılabilir.|Kullanılabilirlik bölgeleri yalnızca dağıtım sırasında yapılandırılabilir. Güvenlik duvarı dağıtıldıktan sonra Kullanılabilirlik Bölgelerini yapılandıramazsınız.|Bu tasarım gereğidir.|
|Gelen bağlantılarda SNAT|DNAT'ye ek olarak, güvenlik duvarı genel IP adresi (gelen) üzerinden yapılan bağlantılar, güvenlik duvarı özel IP'lerinden birine sayılmaktadır. Simetrik yönlendirmeyi sağlamak için bugün bu gereksinim (Aktif/Etkin NPA'lar için de geçerlidir.|HTTP/S'nin orijinal kaynağını korumak için [XFF](https://en.wikipedia.org/wiki/X-Forwarded-For) üstbilgilerini kullanmayı düşünün. Örneğin, güvenlik duvarının önünde [Azure Ön Kapı](../frontdoor/front-door-http-headers-protocol.md#front-door-to-backend) veya Azure Uygulama Ağ [Geçidi](../application-gateway/rewrite-http-headers.md) gibi bir hizmet kullanın. Ayrıca, Azure Ön Kapı'nın bir parçası olarak WAF'ı ve güvenlik duvarına zincir ekleyebilirsiniz.
|SQL FQDN filtreleme desteği yalnızca proxy modunda (port 1433)|Azure SQL Veritabanı, Azure SQL Veri Ambarı ve Azure SQL Yönetilen Örnek için:<br><br>Önizleme sırasında, SQL FQDN filtreleme yalnızca proxy modunda desteklenir (bağlantı noktası 1433).<br><br>Azure SQL IaaS için:<br><br>Standart olmayan bağlantı noktaları kullanıyorsanız, uygulama kurallarında bu bağlantı noktalarını belirtebilirsiniz.|Yeniden yönlendirme modunda KI SQL için (Azure içinden bağlanıyorsanız varsayılan), bunun yerine Azure Güvenlik Duvarı ağ kurallarının bir parçası olarak SQL hizmet etiketini kullanarak erişime filtre uygulayabilirsiniz.
|TCP bağlantı noktası 25'te giden trafiğe izin verilmiyor| TCP bağlantı noktası 25 kullanan giden SMTP bağlantıları engellendi. Bağlantı noktası 25 öncelikle kimlik doğrulamamış e-posta teslimi için kullanılır. Bu, sanal makineler için varsayılan platform davranışıdır. Daha fazla bilgi için [Azure'da giden SMTP bağlantısı sorunlarıyla](../virtual-network/troubleshoot-outbound-smtp-connectivity.md)ilgili daha fazla Sorun Giderme sorunu görün. Ancak, sanal makinelerin aksine, bu işlevselliği Azure Güvenlik Duvarı'nda etkinleştirmek şu anda mümkün değildir.|SMTP sorun giderme makalesinde belgelenmiş olarak e-posta göndermek için önerilen yöntemi izleyin. Veya, varsayılan rotanızdan güvenlik duvarına giden SMTP erişimine ihtiyaç olan sanal makineyi hariç tinler. Bunun yerine, giden erişimi doğrudan Internet'e yapılandırın.
|Etkin FTP desteklenmiyor|Etkin FTP, FTP PORT komutunu kullanarak FTP sıçrama saldırılarına karşı korunmak için Azure Güvenlik Duvarı'nda devre dışı bırakılır.|Bunun yerine Pasif FTP kullanabilirsiniz. Güvenlik duvarında TCP bağlantı noktaları 20 ve 21'i hala açıkça açmanız gerekir.
|SNAT bağlantı noktası kullanım ölçümü %0'ı gösterir|Azure Güvenlik Duvarı SNAT bağlantı noktası kullanım ölçümü, SNAT bağlantı noktaları kullanıldığında bile %0 kullanım gösterebilir. Bu durumda, güvenlik duvarı sistem durumu ölçümünün bir parçası olarak metrik kullanmak yanlış bir sonuç sağlar.|Bu sorun giderilmiş ve Mayıs 2020 için üretime rollout hedeflenmiştir. Bazı durumlarda, güvenlik duvarı yeniden dağıtımı sorunu giderir, ancak tutarlı değildir. Ara geçici çözüm olarak, *yalnızca durum=bozulmuş*, *durum=sağlıksız*için değil aramak için güvenlik duvarı durumu durumunu kullanın. Port tükenmesi *bozulmuş*olarak gösterecektir. Güvenlik duvarı sağlığını etkilemek için daha fazla ölçüm olduğunda *sağlıklı değil,* gelecekteki kullanım için ayrılmıştır.
|DNAT, Zorunlu Tünelleme özelliğiyle desteklenmiyor|Zorunlu Tünelleme etkinleştirilmiş olan güvenlik duvarları, asimetrik yönlendirme nedeniyle Internet'ten gelen erişimi destekleyemez.|Bu asimetrik yönlendirme nedeniyle tasarım gereğidir. Gelen bağlantıların dönüş yolu, bağlantının kurulduğunu görmeyen şirket içi güvenlik duvarından geçer.
|Giden Pasif FTP, birden çok genel IP adresine sahip Güvenlik Duvarları için çalışmaz.|Pasif FTP, kontrol ve veri kanalları için farklı bağlantılar kurar. Birden çok genel IP adresine sahip bir Güvenlik Duvarı veri giden gönderdiğinde, kaynak IP adresi için ortak IP adreslerinden birini rasgele seçer. FTP, veri ve denetim kanalları farklı kaynak IP adresleri kullandığında başarısız olur.|Açık bir SNAT yapılandırması planlanır. Bu arada, bu durumda tek bir IP adresi kullanmayı düşünün.|
|NetworkRuleHit ölçümü bir protokol boyutu eksik|ApplicationRuleHit ölçümü filtretabanlı protokolün filtrelemesine izin verir, ancak bu özellik ilgili NetworkRuleHit ölçümünde eksiktir.|Bir düzeltme araştırılıyor.|
|Yapılandırma güncelleştirmeleri ortalama beş dakika sürebilir.|Azure Güvenlik Duvarı yapılandırma güncelleştirmesi ortalama üç ila beş dakika sürebilir ve paralel güncelleştirmeler desteklenmez.|Bir düzeltme araştırılıyor.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma](tutorial-firewall-deploy-portal.md)
- [Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma](deploy-template.md)
- [Azure Güvenlik Duvarı test ortamı oluşturma](scripts/sample-create-firewall-test.md)
