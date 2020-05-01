---
title: Azure Güvenlik Duvarı nedir?
description: Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc
ms.date: 04/23/2020
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: a0bd89a2ffdc15e192e51660c58c1d66efa1284a
ms.sourcegitcommit: acc558d79d665c8d6a5f9e1689211da623ded90a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82598462"
---
# <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

![ICSA sertifikası](media/overview/icsa-cert-firewall-small.png)

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe ve sınırsız bulut ölçeklenebilirliğine sahip, tam durum bilgisi olan bir hizmet olarak güvenlik duvarıdır.

![Güvenlik duvarına genel bakış](media/overview/firewall-threat.png)

Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz. Azure Güvenlik Duvarı, dış güvenlik duvarlarının sanal ağınızdan kaynaklanan trafiği tanımlamasına olanak tanımak amacıyla sanal ağ kaynaklarınız için statik genel bir IP adresi kullanır.  Hizmet, günlük ve analiz için Azure İzleyici ile tamamen tümleşik çalışır.

Azure Güvenlik Duvarı şu özellikleri sunar:

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

FQDN etiketleri, güvenlik duvarınız aracılığıyla iyi bilinen Azure hizmeti ağ trafiğine izin vermeyi kolaylaştırır. Örneğin Windows Update ağ trafiğine güvenlik duvarınızda izin vermek istediğiniz varsayalım. Bir uygulama kuralı oluşturup Windows Update etiketini eklersiniz. Artık Windows Update’in ağ trafiği, güvenlik duvarınızdan geçebilir.

## <a name="service-tags"></a>Hizmet etiketleri

Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz ve bir etiket içinde hangi IP adreslerinin ekleneceğini belirtebilirsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir.

## <a name="threat-intelligence"></a>Tehdit bilgileri

Güvenlik duvarınızın, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen/giden trafiği uyarmasını ve reddetmesi için tehdit zekası tabanlı filtreleme etkinleştirilebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklıdır.

## <a name="outbound-snat-support"></a>Giden SNAT desteği

Tüm giden sanal ağ trafiği IP adresleri Azure Güvenlik Duvarı genel IP’sine çevrilir (Kaynak Ağ Adresi Çevirisi). Sanal ağınızdan kaynaklanan uzak İnternet hedeflerine yönelik trafiği tanımlayabilir ve buna izin verebilirsiniz. Hedef IP, [ıANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP aralığı olduğunda Azure GÜVENLIK duvarı SNAT değildir. 

Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı, trafiği AzureFirewallSubnet içindeki güvenlik duvarı özel IP adreslerinden birine karşı bir şekilde SNAT olarak kullanır. Azure Güvenlik duvarını, genel IP adresi aralığınızı **SNAT olarak** yapılandırmak için yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Güvenlik DUVARı SNAT özel IP adresi aralıkları](snat-private-range.md).

## <a name="inbound-dnat-support"></a>Gelen DNAT desteği

Güvenlik duvarınızın genel IP adresine gelen Internet ağ trafiği çevrilir (hedef ağ adresi çevirisi) ve sanal ağlarınızdaki özel IP adreslerine filtrelenir.

## <a name="multiple-public-ip-addresses"></a>Birden çok genel IP adresi

Güvenlik duvarınızla birden çok genel IP adresini (100 'e kadar) ilişkilendirebilirsiniz.

Bu, aşağıdaki senaryolara izin vermez:

- **DNAT** -birden çok standart bağlantı noktası örneğini arka uç sunucularınıza çevirebilirsiniz. Örneğin, iki genel IP adresiniz varsa, TCP bağlantı noktası 3389 ' ü (RDP) her iki IP adresi için de çevirebilirsiniz.
- **SNAT** -giden SNAT bağlantıları için ek bağlantı noktaları kullanılabilir ve bu, SNAT bağlantı noktası tükenmesi potansiyelini azaltır. Azure Güvenlik Duvarı şu anda bir bağlantı için kullanılacak kaynak genel IP adresini rastgele seçer. Ağınızda herhangi bir aşağı akış filtresi varsa, güvenlik duvarınızdan ilişkili tüm genel IP adreslerine izin vermeniz gerekir.

## <a name="azure-monitor-logging"></a>Azure İzleyici günlükleri

Tüm olaylar Azure Izleyici ile tümleşiktir ve günlükleri bir depolama hesabında arşivlemenize, Olay Hub 'ınıza olayları akışlarınıza veya Azure Izleyici günlüklerine gönderebilmenizi sağlar.

## <a name="certifications"></a>Sertifikalar

Azure Güvenlik Duvarı, ödeme kartı sektör (PCI), hizmet organizasyonu denetimleri (SOC), Uluslararası Standardizasyon Teşkilatı (ISO) ve ıCSA Labs ile uyumludur. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı uyumluluk sertifikaları](compliance-certifications.md).


## <a name="known-issues"></a>Bilinen sorunlar

Azure Güvenlik Duvarındaki bilinen sorunlar şunlardır:

|Sorun  |Açıklama  |Risk azaltma  |
|---------|---------|---------|
TCP/UDP dışı protokollere (örneğin ICMP) yönelik ağ filtreleme kuralları İnternet'e bağlı trafik için çalışmaz|TCP/UDP olmayan protokoller için ağ filtreleme kuralları, SNAT ile genel IP adresiniz arasında çalışmaz. TCP/UDP dışı protokoller, uç alt ağlarla sanal ağlar arasında desteklenir.|Azure Güvenlik Duvarı, [bugün IP protokolleri için SNAT desteği olmayan](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) Standart Load Balancer kullanır. Gelecekteki bir sürümde bu senaryoyu desteklemeye yönelik seçenekleri araştırıyoruz.|
|ICMP için eksik PowerShell ve CLI desteği|Azure PowerShell ve CLı, ağ kurallarında geçerli bir protokol olarak ıCMP 'yi desteklemez.|Portal ve REST API aracılığıyla bir protokol olarak ıCMP kullanmak yine de mümkündür. PowerShell ve CLı için yakında ıCMP eklemek üzere çalışıyoruz.|
|FQDN etiketleri bir protokol: bağlantı noktası ayarlamayı gerektirir|FQDN etiketleriyle uygulama kuralları için bağlantı noktası: protokol tanımı gerekir.|Bağlantı noktası:protokol değeri olarak **https** kullanabilirsiniz. FQDN etiketleri kullanıldığında bu alanı isteğe bağlı hale getirmek için çalışıyoruz.|
|Bir güvenlik duvarını farklı bir kaynak grubuna veya aboneliğe taşıma desteklenmiyor|Bir güvenlik duvarının farklı bir kaynak grubuna veya aboneliğe taşınması desteklenmez.|Bu işlevi desteklemek, yol haritamız üzerinde. Bir güvenlik duvarını başka bir kaynak grubuna veya aboneliğe taşımak için geçerli örneği silmeniz ve yeni kaynak grubunda veya abonelikte yeniden oluşturmanız gerekir.|
|Tehdit bilgileri uyarıları maskeli olabilir|Giden filtreleme maskeleri için hedef 80/443 olan ağ kuralları yalnızca uyarı moduna yapılandırıldığında tehdit bilgileri uyarılarını arar.|Uygulama kurallarını kullanarak 80/443 için giden filtreleme oluşturun. Ya da tehdit zekası modunu **uyarı ve reddetme**olarak değiştirin.|
|Azure Güvenlik Duvarı yalnızca ad çözümlemesi için Azure DNS kullanır|Azure Güvenlik Duvarı yalnızca Azure DNS kullanarak FQDN 'leri çözer. Özel bir DNS sunucusu desteklenmez. Diğer alt ağlarda DNS çözümlemesi üzerinde hiçbir etkisi yoktur.|Bu sınırlamayı rahat hale getiriyoruz.|
|Azure Güvenlik Duvarı DNAT, özel IP hedefleri için çalışmıyor|Azure Güvenlik Duvarı DNAT desteği Internet çıkış/giriş ile sınırlıdır. DNAT Şu anda özel IP hedefleri için çalışmıyor. Örneğin, bağlı bileşene bağlı olarak.|Bu geçerli bir kısıtlamadır.|
|İlk genel IP yapılandırması kaldırılamıyor|Her bir Azure Güvenlik Duvarı genel IP adresi bir *IP yapılandırmasına*atanır.  İlk IP yapılandırması, güvenlik duvarı dağıtımı sırasında atanır ve genellikle güvenlik duvarı alt ağına (bir şablon dağıtımı aracılığıyla açıkça farklı şekilde yapılandırılmamışsa) bir başvuru içerir. Güvenlik duvarını serbest bırakacağından bu IP yapılandırmasını silemezsiniz. Güvenlik duvarının kullanılabilir en az bir genel IP adresi varsa, bu IP yapılandırmasıyla ilişkili genel IP adresini değiştirmeye veya kaldırmaya devam edebilirsiniz.|Bu tasarım gereğidir.|
|Kullanılabilirlik alanları yalnızca dağıtım sırasında yapılandırılabilir.|Kullanılabilirlik alanları yalnızca dağıtım sırasında yapılandırılabilir. Bir güvenlik duvarı dağıtıldıktan sonra Kullanılabilirlik Alanları yapılandıramazsınız.|Bu tasarım gereğidir.|
|Gelen bağlantılarda SNAT|DNAT 'nin yanı sıra, güvenlik duvarı genel IP adresi (gelen) ile kurulan bağlantılar, güvenlik duvarı özel IP 'lerinden birine karşı denetlenir. Simetrik yönlendirmeyi sağlamak için bugün bu gereksinim (etkin/etkin NVA 'lar için de).|HTTP/S için özgün kaynağı korumak üzere [XFF](https://en.wikipedia.org/wiki/X-Forwarded-For) üst bilgilerini kullanmayı göz önünde bulundurun. Örneğin, [Azure ön kapısı](../frontdoor/front-door-http-headers-protocol.md#front-door-to-backend) veya [Azure Application Gateway](../application-gateway/rewrite-http-headers.md) gibi bir hizmeti güvenlik duvarının önünde kullanın. Ayrıca, Azure ön kapısının parçası olarak WAF 'yi ve güvenlik duvarını de zincirde ekleyebilirsiniz.
|SQL FQDN filtrelemesi yalnızca Proxy modunda desteklenir (bağlantı noktası 1433)|Azure SQL veritabanı, Azure SQL veri ambarı ve Azure SQL yönetilen örneği için:<br><br>Önizleme sırasında SQL FQDN filtrelemesi yalnızca Proxy modunda desteklenir (bağlantı noktası 1433).<br><br>Azure SQL IaaS için:<br><br>Standart olmayan bağlantı noktaları kullanıyorsanız, bu bağlantı noktalarını uygulama kurallarında belirtebilirsiniz.|SQL yeniden yönlendirme modunda (Azure içinden bağlanıyorsanız varsayılan), Azure Güvenlik Duvarı ağ kurallarının bir parçası olarak SQL hizmeti etiketini kullanarak erişimi filtreleyebilirsiniz.
|TCP bağlantı noktası 25 ' i giden trafiğe izin verilmez| TCP bağlantı noktası 25 kullanan giden SMTP bağlantıları engellenir. Bağlantı noktası 25 öncelikle kimliği doğrulanmamış e-posta teslimi için kullanılır. Bu, sanal makineler için varsayılan platform davranışıdır. Daha fazla bilgi için bkz. [Azure 'da gıden SMTP bağlantısı sorunlarını giderme](../virtual-network/troubleshoot-outbound-smtp-connectivity.md). Ancak, sanal makinelerden farklı olarak, Azure Güvenlik duvarında bu işlevselliği etkinleştirmek mümkün değildir. Not: kimliği doğrulanmış SMTP (bağlantı noktası 587) veya SMTP 'yi 25 dışında bir bağlantı noktası üzerinden izin vermek için, SMTP incelemesi Şu anda desteklenmediğinden bir ağ kuralı ve uygulama kuralı yapılandırmadığınızdan emin olun.|SMTP sorun giderme makalesinde belirtildiği gibi, e-posta göndermek için önerilen yöntemi izleyin. Ya da, giden SMTP erişimine gereken sanal makineyi varsayılan yönlendirinizden güvenlik duvarına dışlayın. Bunun yerine, giden erişimi doğrudan internet 'e yapılandırın.
|Etkin FTP desteklenmiyor|Etkin FTP FTP bağlantı noktası komutu kullanılarak FTP sıçrama saldırılarına karşı korumak için Azure Güvenlik duvarında devre dışı bırakılmıştır.|Bunun yerine Pasif FTP kullanabilirsiniz. Hala güvenlik duvarında 20 ve 21 numaralı TCP bağlantı noktalarını açık bir şekilde açmanız gerekir.
|SNAT bağlantı noktası kullanım ölçümü %0 gösterir|Azure Güvenlik Duvarı SNAT bağlantı noktası kullanım ölçümü, SNAT bağlantı noktaları kullanıldığında bile %0 kullanım gösterebilir. Bu durumda, güvenlik duvarı sistem durumu ölçümünün bir parçası olarak ölçüm kullanılması yanlış bir sonuç verir.|Bu sorun düzeltildi ve üretime dağıtımı 2020 Mayıs ' e yöneliktir. Bazı durumlarda, güvenlik duvarı yeniden dağıtımı sorunu çözer, ancak tutarlı değildir. Ara geçici çözüm olarak, durum = *düşürülmüş*durumunu aramak için yalnızca güvenlik duvarı sistem durumunu kullanın, *durum = sağlıksız*için değildir. Bağlantı noktası tükenmesi *düşürüldü*olarak görünür. *Sağlıklı değil* , güvenlik duvarı sistem durumunu etkilemek için daha fazla ölçüm olduğunda gelecekte kullanılmak üzere ayrılmıştır.
|Zorunlu tünelleme etkinken DNAT desteklenmez|Zorlamalı tünelleme etkinken dağıtılan güvenlik duvarları, asimetrik yönlendirme nedeniyle Internet 'ten gelen erişimi destekleyemez.|Asimetrik yönlendirme nedeniyle bu tasarıma sahiptir. Gelen bağlantılar için dönüş yolu, kurulan bağlantıyı görmeyen şirket içi güvenlik duvarı aracılığıyla geçer.
|Giden Pasif FTP birden çok genel IP adresi olan güvenlik duvarları için çalışmaz.|Pasif FTP, denetim ve veri kanalları için farklı bağlantılar oluşturur. Birden çok genel IP adresine sahip bir güvenlik duvarı giden verileri gönderdiğinde, kaynak IP adresi için genel IP adreslerinden birini rastgele seçer. Veriler ve denetim kanalları farklı kaynak IP adresleri kullandıklarında FTP başarısız olur.|Açık bir SNAT yapılandırması planlanmaktadır. Bu sırada, bu durumda tek bir IP adresi kullanmayı göz önünde bulundurun.|
|NetworkRuleHit ölçümünde protokol boyutu eksik|ApplicationRuleHit metrik, filtreleme tabanlı protokole izin veriyor, ancak ilgili NetworkRuleHit ölçümünde bu yetenek yok.|Bir düzelme araştırılır.|
|64000 ve 65535 arasındaki bağlantı noktalarıyla NAT kuralları desteklenmez|Azure Güvenlik Duvarı, ağ ve uygulama kurallarında 1-65535 aralığında bulunan tüm bağlantı noktalarına izin verir, ancak NAT kuralları yalnızca 1-63999 aralığındaki bağlantı noktalarını destekler.|Bu geçerli bir kısıtlamadır.
|Yapılandırma güncelleştirmeleri ortalama beş dakika sürebilir.|Bir Azure Güvenlik Duvarı yapılandırma güncelleştirmesi ortalama üç ila beş dakika sürebilir ve paralel güncelleştirmeler desteklenmez.|Bir düzelme araştırılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma](tutorial-firewall-deploy-portal.md)
- [Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma](deploy-template.md)
- [Azure Güvenlik Duvarı test ortamı oluşturma](scripts/sample-create-firewall-test.md)
