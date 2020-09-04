---
title: Azure Güvenlik Duvarı nedir?
description: Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: overview
ms.custom: mvc, contperfq1
ms.date: 08/25/2020
ms.author: victorh
Customer intent: As an administrator, I want to evaluate Azure Firewall so I can determine if I want to use it.
ms.openlocfilehash: 0572613fe33d525ed1a5a42c627de3ce1049a290
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89434053"
---
# <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

![ICSA sertifikası](media/overview/icsa-cert-firewall-small.png)

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe ve sınırsız bulut ölçeklenebilirliğine sahip, tam durum bilgisi olan bir hizmet olarak güvenlik duvarıdır.

![Güvenlik duvarına genel bakış](media/overview/firewall-threat.png)

Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz. Azure Güvenlik Duvarı, dış güvenlik duvarlarının sanal ağınızdan kaynaklanan trafiği tanımlamasına olanak tanımak amacıyla sanal ağ kaynaklarınız için statik genel bir IP adresi kullanır.  Hizmet, günlük ve analiz için Azure İzleyici ile tamamen tümleşik çalışır.

## <a name="features"></a>Özellikler

Azure Güvenlik duvarı özellikleri hakkında bilgi edinmek için bkz. [Azure Güvenlik Duvarı Özellikleri](features.md).

## <a name="known-issues"></a>Bilinen sorunlar

Azure Güvenlik Duvarındaki bilinen sorunlar şunlardır:

|Sorun  |Açıklama  |Risk azaltma  |
|---------|---------|---------|
TCP/UDP dışı protokollere (örneğin ICMP) yönelik ağ filtreleme kuralları İnternet'e bağlı trafik için çalışmaz|TCP/UDP olmayan protokoller için ağ filtreleme kuralları, SNAT ile genel IP adresiniz arasında çalışmaz. TCP/UDP dışı protokoller, uç alt ağlarla sanal ağlar arasında desteklenir.|Azure Güvenlik Duvarı, [bugün IP protokolleri için SNAT desteği olmayan](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) Standart Load Balancer kullanır. Gelecekteki bir sürümde bu senaryoyu desteklemeye yönelik seçenekleri araştırıyoruz.|
|ICMP için eksik PowerShell ve CLI desteği|Azure PowerShell ve CLı, ağ kurallarında geçerli bir protokol olarak ıCMP 'yi desteklemez.|Portal ve REST API aracılığıyla bir protokol olarak ıCMP kullanmak yine de mümkündür. PowerShell ve CLı için yakında ıCMP eklemek üzere çalışıyoruz.|
|FQDN etiketleri bir protokol: bağlantı noktası ayarlamayı gerektirir|FQDN etiketleriyle uygulama kuralları için bağlantı noktası: protokol tanımı gerekir.|Bağlantı noktası:protokol değeri olarak **https** kullanabilirsiniz. FQDN etiketleri kullanıldığında bu alanı isteğe bağlı hale getirmek için çalışıyoruz.|
|Bir güvenlik duvarını farklı bir kaynak grubuna veya aboneliğe taşıma desteklenmiyor|Bir güvenlik duvarının farklı bir kaynak grubuna veya aboneliğe taşınması desteklenmez.|Bu işlevi desteklemek, yol haritamız üzerinde. Bir güvenlik duvarını başka bir kaynak grubuna veya aboneliğe taşımak için geçerli örneği silmeniz ve yeni kaynak grubunda veya abonelikte yeniden oluşturmanız gerekir.|
|Tehdit bilgileri uyarıları maskeli olabilir|Giden filtreleme maskeleri için hedef 80/443 olan ağ kuralları yalnızca uyarı moduna yapılandırıldığında tehdit bilgileri uyarılarını arar.|Uygulama kurallarını kullanarak 80/443 için giden filtreleme oluşturun. Ya da tehdit zekası modunu **uyarı ve reddetme**olarak değiştirin.|
|Azure Güvenlik Duvarı DNAT, özel IP hedefleri için çalışmıyor|Azure Güvenlik Duvarı DNAT desteği Internet çıkış/giriş ile sınırlıdır. DNAT Şu anda özel IP hedefleri için çalışmıyor. Örneğin, bağlı bileşene bağlı olarak.|Bu geçerli bir kısıtlamadır.|
|İlk genel IP yapılandırması kaldırılamıyor|Her bir Azure Güvenlik Duvarı genel IP adresi bir *IP yapılandırmasına*atanır.  İlk IP yapılandırması, güvenlik duvarı dağıtımı sırasında atanır ve genellikle güvenlik duvarı alt ağına (bir şablon dağıtımı aracılığıyla açıkça farklı şekilde yapılandırılmamışsa) bir başvuru içerir. Güvenlik duvarını serbest bırakacağından bu IP yapılandırmasını silemezsiniz. Güvenlik duvarının kullanılabilir en az bir genel IP adresi varsa, bu IP yapılandırmasıyla ilişkili genel IP adresini değiştirmeye veya kaldırmaya devam edebilirsiniz.|Bu tasarım gereğidir.|
|Kullanılabilirlik alanları yalnızca dağıtım sırasında yapılandırılabilir.|Kullanılabilirlik alanları yalnızca dağıtım sırasında yapılandırılabilir. Bir güvenlik duvarı dağıtıldıktan sonra Kullanılabilirlik Alanları yapılandıramazsınız.|Bu tasarım gereğidir.|
|Gelen bağlantılarda SNAT|DNAT 'nin yanı sıra, güvenlik duvarı genel IP adresi (gelen) ile kurulan bağlantılar, güvenlik duvarı özel IP 'lerinden birine karşı denetlenir. Simetrik yönlendirmeyi sağlamak için bugün bu gereksinim (etkin/etkin NVA 'lar için de).|HTTP/S için özgün kaynağı korumak üzere [XFF](https://en.wikipedia.org/wiki/X-Forwarded-For) üst bilgilerini kullanmayı göz önünde bulundurun. Örneğin, [Azure ön kapısı](../frontdoor/front-door-http-headers-protocol.md#front-door-to-backend) veya [Azure Application Gateway](../application-gateway/rewrite-http-headers.md) gibi bir hizmeti güvenlik duvarının önünde kullanın. Ayrıca, Azure ön kapısının parçası olarak WAF 'yi ve güvenlik duvarını de zincirde ekleyebilirsiniz.
|SQL FQDN filtrelemesi yalnızca Proxy modunda desteklenir (bağlantı noktası 1433)|Azure SQL veritabanı, Azure SYNAPSE Analytics ve Azure SQL yönetilen örneği için:<br><br>Önizleme sırasında SQL FQDN filtrelemesi yalnızca Proxy modunda desteklenir (bağlantı noktası 1433).<br><br>Azure SQL IaaS için:<br><br>Standart olmayan bağlantı noktaları kullanıyorsanız, bu bağlantı noktalarını uygulama kurallarında belirtebilirsiniz.|SQL yeniden yönlendirme modunda (Azure içinden bağlanıyorsanız varsayılan), Azure Güvenlik Duvarı ağ kurallarının bir parçası olarak SQL hizmeti etiketini kullanarak erişimi filtreleyebilirsiniz.
|TCP bağlantı noktası 25 ' i giden trafiğe izin verilmez| TCP bağlantı noktası 25 kullanan giden SMTP bağlantıları engellenir. Bağlantı noktası 25 öncelikle kimliği doğrulanmamış e-posta teslimi için kullanılır. Bu, sanal makineler için varsayılan platform davranışıdır. Daha fazla bilgi için bkz. [Azure 'da gıden SMTP bağlantısı sorunlarını giderme](../virtual-network/troubleshoot-outbound-smtp-connectivity.md). Ancak, sanal makinelerden farklı olarak, Azure Güvenlik duvarında bu işlevselliği etkinleştirmek mümkün değildir. Not: kimliği doğrulanmış SMTP (bağlantı noktası 587) veya SMTP 'yi 25 dışında bir bağlantı noktası üzerinden izin vermek için, SMTP incelemesi Şu anda desteklenmediğinden bir ağ kuralı ve uygulama kuralı yapılandırmadığınızdan emin olun.|SMTP sorun giderme makalesinde belirtildiği gibi, e-posta göndermek için önerilen yöntemi izleyin. Ya da, giden SMTP erişimine gereken sanal makineyi varsayılan yönlendirinizden güvenlik duvarına dışlayın. Bunun yerine, giden erişimi doğrudan internet 'e yapılandırın.
|Etkin FTP desteklenmiyor|Etkin FTP FTP bağlantı noktası komutu kullanılarak FTP sıçrama saldırılarına karşı korumak için Azure Güvenlik duvarında devre dışı bırakılmıştır.|Bunun yerine Pasif FTP kullanabilirsiniz. Hala güvenlik duvarında 20 ve 21 numaralı TCP bağlantı noktalarını açık bir şekilde açmanız gerekir.
|SNAT bağlantı noktası kullanım ölçümü %0 gösterir|Azure Güvenlik Duvarı SNAT bağlantı noktası kullanım ölçümü, SNAT bağlantı noktaları kullanıldığında bile %0 kullanım gösterebilir. Bu durumda, güvenlik duvarı sistem durumu ölçümünün bir parçası olarak ölçüm kullanılması yanlış bir sonuç verir.|Bu sorun düzeltildi ve üretime dağıtımı 2020 Mayıs ' e yöneliktir. Bazı durumlarda, güvenlik duvarı yeniden dağıtımı sorunu çözer, ancak tutarlı değildir. Ara geçici çözüm olarak, durum = *düşürülmüş*durumunu aramak için yalnızca güvenlik duvarı sistem durumunu kullanın, *durum = sağlıksız*için değildir. Bağlantı noktası tükenmesi *düşürüldü*olarak görünür. *Sağlıklı değil* , güvenlik duvarı sistem durumunu etkilemek için daha fazla ölçüm olduğunda gelecekte kullanılmak üzere ayrılmıştır.
|Zorunlu tünelleme etkinken DNAT desteklenmez|Zorlamalı tünelleme etkinken dağıtılan güvenlik duvarları, asimetrik yönlendirme nedeniyle Internet 'ten gelen erişimi destekleyemez.|Asimetrik yönlendirme nedeniyle bu tasarıma sahiptir. Gelen bağlantılar için dönüş yolu, kurulan bağlantıyı görmeyen şirket içi güvenlik duvarı aracılığıyla geçer.
|Giden Pasif FTP birden çok genel IP adresi olan güvenlik duvarları için çalışmıyor|Pasif FTP, denetim ve veri kanalları için farklı bağlantılar oluşturur. Birden çok genel IP adresine sahip bir güvenlik duvarı giden verileri gönderdiğinde, kaynak IP adresi için genel IP adreslerinden birini rastgele seçer. Veriler ve denetim kanalları farklı kaynak IP adresleri kullandıklarında FTP başarısız olur.|Açık bir SNAT yapılandırması planlanmaktadır. Bu sırada, bu durumda tek bir IP adresi kullanmayı göz önünde bulundurun.|
|NetworkRuleHit ölçümünde protokol boyutu eksik|ApplicationRuleHit metrik, filtreleme tabanlı protokole izin veriyor, ancak ilgili NetworkRuleHit ölçümünde bu yetenek yok.|Bir düzelme araştırılır.|
|64000 ve 65535 arasındaki bağlantı noktalarıyla NAT kuralları desteklenmez|Azure Güvenlik Duvarı, ağ ve uygulama kurallarında 1-65535 aralığında bulunan tüm bağlantı noktalarına izin verir, ancak NAT kuralları yalnızca 1-63999 aralığındaki bağlantı noktalarını destekler.|Bu geçerli bir kısıtlamadır.
|Yapılandırma güncelleştirmeleri ortalama beş dakika sürebilir|Bir Azure Güvenlik Duvarı yapılandırma güncelleştirmesi ortalama üç ila beş dakika sürebilir ve paralel güncelleştirmeler desteklenmez.|Bir düzelme araştırılır.|
|Azure Güvenlik Duvarı, HTTPS ve MSSQL trafiğini filtrelemek için SNı TLS üst bilgilerini kullanır|Tarayıcı veya sunucu yazılımı sunucu adı göstergesi (SNı) uzantısını desteklemiyorsa, Azure Güvenlik Duvarı üzerinden bağlanamazsınız.|Tarayıcı veya sunucu yazılımı SNı desteklemiyorsa, bağlantıyı bir uygulama kuralı yerine bir ağ kuralı kullanarak kontrol edebilirsiniz. SNı 'yi destekleyen yazılımlar için bkz. [sunucu adı belirtme](https://wikipedia.org/wiki/Server_Name_Indication) .|
|Özel DNS (Önizleme) zorlamalı tünelleme ile çalışmıyor|Zorlamalı tünel etkinleştirilirse, özel DNS (Önizleme) çalışmaz.|Bir düzelme araştırılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portalını kullanarak Azure Güvenlik Duvarı'nı dağıtma ve yapılandırma](tutorial-firewall-deploy-portal.md)
- [Şablon kullanarak Azure Güvenlik Duvarı’nı dağıtma](deploy-template.md)
- [Azure Güvenlik Duvarı test ortamı oluşturma](scripts/sample-create-firewall-test.md)
