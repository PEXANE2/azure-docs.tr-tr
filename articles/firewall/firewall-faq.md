---
title: Azure Güvenlik Duvarı SSS
description: Azure Güvenlik Duvarı için SSS. Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı ağ güvenliği hizmeti.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: victorh
ms.openlocfilehash: af66da115e228efe39e4cd5dda3c494b71428676
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113567"
---
# <a name="azure-firewall-faq"></a>Azure Güvenlik Duvarı SSS

## <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirlik ve sınırsız bulut ölçeklenebilirliği ile hizmet olarak tamamen görkemli bir güvenlik duvarıdır. Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz.

## <a name="what-capabilities-are-supported-in-azure-firewall"></a>Azure Güvenlik Duvarı'nda hangi özellikler desteklenir?

* Hizmet olarak durum lu güvenlik duvarı
* Sınırsız bulut ölçeklenebilirliği ile yerleşik yüksek kullanılabilirlik
* FQDN filtreleme
* FQDN etiketleri
* Ağ trafiği filtreleme kuralları
* Giden SNAT desteği
* Gelen DNAT desteği
* Azure abonelikleri ve VNET'ler arasında merkezi olarak uygulama ve ağ bağlantısı ilkeleri oluşturun, uygulayın ve günlüğe kaydedin
* Günlük ve analitik için Azure Monitör ile tam entegre

## <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Azure Güvenlik Duvarı için tipik dağıtım modeli nedir?

Azure Güvenlik Duvarı'nı herhangi bir sanal ağa dağıtabilirsiniz, ancak müşteriler genellikle bu alanı merkezi bir sanal ağa dağıtabilir ve hub ve kollu bir modelde diğer sanal ağları eşler. Daha sonra, bu merkezi güvenlik duvarı sanal ağına işaret etmek için bakan sanal ağlardan varsayılan rotayı ayarlayabilirsiniz. Global VNet eşlemedesteklenir, ancak olası performans ve bölgeler arasında gecikme sorunları nedeniyle önerilmez. En iyi performans için, bölge başına bir güvenlik duvarı dağıtın.

Bu modelin avantajı, farklı abonelikler arasında birden çok kollu VNET'ler üzerinde merkezi olarak denetim uygulayabilme yeteneğidir. Her VNet'te ayrı ayrı bir güvenlik duvarı dağıtmanız gerekmedin. Maliyet tasarrufu, müşteri trafiği desenleri temel alınarak ortak emsal maliyetine göre ölçülmelidir.

## <a name="how-can-i-install-the-azure-firewall"></a>Azure Güvenlik Duvarını nasıl yükleyebilirim?

Azure portalını, PowerShell'i, REST API'yi kullanarak veya şablonları kullanarak Azure Güvenlik Duvarı'nı ayarlayabilirsiniz. Bkz. Öğretici: Azure güvenlik duvarını adım adım yönergeler için [Azure portalını kullanarak dağıtın ve yapılandırın.](tutorial-firewall-deploy-portal.md)

## <a name="what-are-some-azure-firewall-concepts"></a>Bazı Azure Güvenlik Duvarı kavramları nelerdir?

Azure Güvenlik Duvarı, kural ve kural koleksiyonlarını destekler. Kural koleksiyonu, aynı sırayı ve önceliği paylaşan bir kural kümesidir. Kural koleksiyonları öncelik sırasına göre yürütülür. Ağ kuralı koleksiyonları uygulama kuralı koleksiyonlarından daha yüksek önceliğe kadardır ve tüm kurallar sonlanır.

Üç tür kural koleksiyonu vardır:

* *Uygulama kuralları*: Bir alt ağdan erişilebilen tam nitelikli alan adlarını (FQDN'leri) yapılandırın.
* *Ağ kuralları*: Kaynak adresleri, protokoller, hedef bağlantı noktaları ve hedef adresleri içeren kuralları yapılandırın.
* *NAT kuralları*: DNAT kurallarını gelen Internet bağlantılarına izin verecek şekilde yapılandırın.

## <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Azure Güvenlik Duvarı gelen trafik filtrelemesi destekliyor mu?

Azure Güvenlik Duvarı gelen ve giden filtrelemeyi destekler. Gelen koruma genellikle HTTP/S olmayan protokoller için kullanılır. Örneğin RDP, SSH ve FTP protokolleri. En iyi gelen HTTP/S koruması için [Azure Web Uygulaması Güvenlik Duvarı (WAF)](../web-application-firewall/overview.md)gibi bir web uygulaması güvenlik duvarı kullanın.

## <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Azure Güvenlik Duvarı tarafından hangi günlük ve analiz hizmetleri desteklenir?

Azure Güvenlik Duvarı, güvenlik duvarı günlüklerini görüntülemek ve analiz etmek için Azure Monitor ile tümleştirilir. Günlükler Günlük Analitiği, Azure Depolama veya Etkinlik Hub'larına gönderilebilir. Log Analytics'te veya Excel ve Power BI gibi farklı araçlarla analiz edilebilirler. Daha fazla bilgi için [Bkz. Öğretici: Azure Güvenlik Duvarı günlüklerini izleyin.](tutorial-diagnostics.md)

## <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Azure Güvenlik Duvarı pazardaki NV'ler gibi mevcut hizmetlerden nasıl farklı çalışır?

Azure Güvenlik Duvarı, belirli müşteri senaryolarını ele alabilecek temel bir güvenlik duvarı hizmetidir. Üçüncü taraf NV'ler ve Azure Güvenlik Duvarı'nın bir karışımına sahip olması beklenir. Birlikte daha iyi çalışmak temel bir önceliktir.

## <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Application Gateway WAF ile Azure Güvenlik Duvarı arasındaki fark nedir?

Web Uygulama Güvenlik Duvarı (WAF), web uygulamalarınızın ortak açıklardan ve güvenlik açıklarına karşı merkezi olarak gelen koruma sağlayan Uygulama Ağ Geçidi'nin bir özelliğidir. Azure Güvenlik Duvarı, HTTP/S olmayan protokoller (örneğin RDP, SSH, FTP), tüm bağlantı noktaları ve protokoller için giden ağ düzeyinde koruma ve giden HTTP/S için uygulama düzeyinde koruma sağlar.

## <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Ağ Güvenlik Grupları (NSGs) ve Azure Güvenlik Duvarı arasındaki fark nedir?

Azure Güvenlik Duvarı hizmeti ağ güvenlik grubu işlevselliğini tamamlar. Birlikte, daha iyi "derinlemesine savunma" ağ güvenliği sağlar. Ağ güvenlik grupları, her abonelikteki sanal ağlardaki kaynaklarla trafiği sınırlamak için dağıtılmış ağ katmanı trafiği filtresi sağlar. Azure Güvenlik Duvarı, farklı abonelikler ve sanal ağlar da ağ ve uygulama düzeyinde koruma sağlayan, hizmet olarak tamamen durumlu, merkezi bir ağ güvenlik duvarıdır.

## <a name="are-network-security-groups-nsgs-supported-on-the-azure-firewall-subnet"></a>Ağ Güvenlik Grupları (NSG'ler) Azure Güvenlik Duvarı alt ağında desteklenir mi?

Azure Güvenlik Duvarı, NIC düzeyindeNG'lere sahip platform koruması (görüntülenemiyor) dahil olmak üzere birden çok koruma katmanına sahip yönetilen bir hizmettir.  Azure Güvenlik Duvarı alt ağına alt ağ düzeyinde NSG'ler gerekmez ve hizmet kesintisi olmaması nı sağlamak için devre dışı bırakılır.

## <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Hizmet bitiş noktalarımla Azure Güvenlik Duvarı'nı nasıl ayarlarım?

PaaS hizmetlerine güvenli erişim için hizmet bitiş noktaları öneririz. Azure Güvenlik Duvarı alt netindeki hizmet uç noktalarını etkinleştirmeyi ve bağlı konuşan sanal ağlarda devre dışı bırakmayı seçebilirsiniz. Bu şekilde her iki özellikten de yararlanırsınız: hizmet uç noktası güvenliği ve tüm trafik için merkezi günlük.

## <a name="what-is-the-pricing-for-azure-firewall"></a>Azure Güvenlik Duvarı'nın fiyatlandırması nedir?

Bkz. [Azure Güvenlik Duvarı Fiyatlandırması.](https://azure.microsoft.com/pricing/details/azure-firewall/)

## <a name="how-can-i-stop-and-start-azure-firewall"></a>Azure Güvenlik Duvarı'nı nasıl durdurup başlatabilirim?

Azure PowerShell *anlaşma bulma* ve *ayırma* yöntemlerini kullanabilirsiniz.

Örneğin:

```azurepowershell
# Stop an existing firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzFirewall -AzureFirewall $azfw
```

```azurepowershell
# Start a firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$vnet = Get-AzVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzFirewall -AzureFirewall $azfw
```

> [!NOTE]
> Bir güvenlik duvarı ve genel IP'yi özgün kaynak grubuna ve aboneye yeniden tahsis etmeniz gerekir.

## <a name="what-are-the-known-service-limits"></a>Bilinen hizmet limitleri nelerdir?

Azure Güvenlik Duvarı hizmet sınırları için Bkz. [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar.](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-firewall-limits)

## <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>Azure Güvenlik Duvarı hub sanal ağda iki kollu sanal ağ arasındaki ağ trafiğini iletebilir ve filtreleyebilir mi?

Evet, iki kollu sanal ağ arasındaki trafiği yönlendirmek ve filtrelemek için bir hub sanal ağındaAzure Güvenlik Duvarı'nı kullanabilirsiniz. Konuşan sanal ağların her birinde, bu senaryonun düzgün çalışması için Varsayılan ağ geçidi olarak Azure Güvenlik Duvarı'nı gösteren bir UDR olmalıdır.

## <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network-or-peered-virtual-networks"></a>Azure Güvenlik Duvarı, aynı sanal ağdaki alt ağlar veya eşlenen sanal ağlar arasındaki ağ trafiğini iletebilir ve filtreleyebilir mi?

Evet. Ancak, Aynı VNET'teki alt ağlar arasındaki trafiği yeniden yönlendirecek ŞEKILDE ÜD'leri yapılandırılması ek dikkat gerektirir. UDR için hedef öneki olarak VNET adres aralığını kullanmak yeterli olmakla birlikte, azure güvenlik duvarı örneği aracılığıyla aynı alt ağdaki tüm trafiği bir makineden başka bir makineye yönlendirir. Bunu önlemek için, Bir sonraki **atlama vnet**türü ile UDR alt net için bir rota ekleyin. Bu yolları yönetmek hantal ve hataya yatkın olabilir. Dahili ağ bölümlemesi için önerilen yöntem, ÜT'ler gerektirmeyen Ağ Güvenlik Grupları kullanmaktır.

## <a name="does-azure-firewall-outbound-snat-between-private-networks"></a>Azure Güvenlik Duvarı özel ağlar arasında giden SNAT mı?

Azure Güvenlik Duvarı, hedef IP adresi [IANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP aralığı olduğunda SNAT yapmaz. Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı trafiği AzureFirewallSubnet'teki güvenlik duvarı özel IP adreslerinden birine gider. Azure Güvenlik Duvarı'nı ortak IP adres aralığınızı SNAT **olarak değil** olarak yapılandırabilirsiniz. Daha fazla bilgi için Azure [Güvenlik Duvarı SNAT özel IP adres aralıklarına](snat-private-range.md)bakın.

## <a name="is-forced-tunnelingchaining-to-a-network-virtual-appliance-supported"></a>Ağ Sanal Cihaz'a zorunlu tünel leme/zincirleme desteklenir mi?

Zorunlu tünelleme desteklenir. Daha fazla bilgi için Azure [Güvenlik Duvarı zorunlu tünel (önizleme)](forced-tunneling.md)bölümüne bakın. 

Azure Güvenlik Duvarı'nın doğrudan Internet bağlantısı olması gerekir. AzureFirewallSubnet'iniz BGP üzerinden şirket içi ağınıza varsayılan bir rota öğrenirse, doğrudan Internet bağlantısını korumak için **Internet** olarak ayarlanmış **NextHopType** değeriyle 0.0.0.0/0 UDR ile bunu geçersiz kılmanız gerekir.

Yapılandırmanız şirket içi bir ağa zorunlu tünel kazma gerektiriyorsa ve Internet hedefleriniz için hedef IP önekleri belirleyebiliyorsanız, bu aralıkları AzureFirewallSubnet'te kullanıcı tanımlı bir rota üzerinden bir sonraki atlama olarak şirket içi ağla yapılandırabilirsiniz. Veya, bu yolları tanımlamak için BGP kullanabilirsiniz.

## <a name="are-there-any-firewall-resource-group-restrictions"></a>Güvenlik duvarı kaynak grubu kısıtlamaları var mı?

Evet. Güvenlik duvarı, VNet ve genel IP adresinin tümü aynı kaynak grubunda olmalıdır.

## <a name="when-configuring-dnat-for-inbound-internet-network-traffic-do-i-also-need-to-configure-a-corresponding-network-rule-to-allow-that-traffic"></a>Gelen Internet ağı trafiği için DNAT yapılandırırken, bu trafiğe izin verecek karşılık gelen bir ağ kuralını da yapılandırmam gerekir mi?

Hayır. NAT kuralları, çevrilen trafiğe izin vermek için dolaylı olarak karşılık gelen bir ağ kuralı ekler. Bu davranışı, çevrilen trafikle eşleşen reddetme kuralları olan bir ağ kural koleksiyonunu açıkça ekleyerek geçersiz kılabilirsiniz. Azure Güvenlik Duvarı kural işleme mantığı hakkında daha fazla bilgi için bkz: [Azure Güvenlik Duvarı kural işleme mantığı](rule-processing.md).

## <a name="how-do-wildcards-work-in-an-application-rule-target-fqdn"></a>Bir uygulama kuralı hedefi FQDN joker karakterler nasıl çalışır?

***.contoso.com**yapılandırırsanız, *anyvalue*.contoso.com izin verir, ancak contoso.com (etki alanı apeks) sağlar. Etki alanı apeksine izin vermek istiyorsanız, bunu açıkça hedef FQDN olarak yapılandırmanız gerekir.

## <a name="what-does-provisioning-state-failed-mean"></a>Sağlama *durumu nedir: Başarısız* lık ne anlama gelir?

Bir yapılandırma değişikliği uygulandığında, Azure Güvenlik Duvarı tüm altta yatan arka uç örneklerini güncelleştirmeye çalışır. Nadir durumlarda, bu arka uç örneklerinden biri yeni yapılandırmayla güncelleştirilemeyebilir ve güncelleştirme işlemi başarısız bir sağlama durumuyla durur. Azure Güvenlik Duvarınız hala çalışıyor, ancak uygulanan yapılandırma, bazı örneklerin diğerlerinin güncelleştirilmiş kural kümesine sahip olduğu önceki yapılandırmaya sahip olduğu tutarsız bir durumda olabilir. Bu durumda, işlem başarılı olana ve Güvenlik Duvarınız *Başarılı* bir sağlama durumunda olana kadar yapılandırmanızı bir kez daha güncelleştirmeyi deneyin.

## <a name="how-does-azure-firewall-handle-planned-maintenance-and-unplanned-failures"></a>Azure Güvenlik Duvarı planlı bakımı ve planlanmamış hataları nasıl işler?
Azure Güvenlik Duvarı, etkin bir yapılandırmada birkaç arka uç düğümünden oluşur.  Herhangi bir planlı bakım için, düğümleri incelikle güncellemek için bağlantı boşaltma mantığına sahibiz.  Güncelleştirmeler, bozulma riskini daha da sınırlamak için Azure bölgelerinin her biri için iş dışı saatlerde planlanır.  Planlanmamış sorunlar için, başarısız düğümü değiştirmek için yeni bir düğüm anlık.  Yeni düğüme bağlantı genellikle hata tarihinden itibaren 10 saniye içinde yeniden kurulur.

## <a name="how-does-connection-draining-work"></a>Bağlantı drenajı nasıl çalışır?

Herhangi bir planlı bakım için, bağlantı drenaj mantığı arka uç düğümlerini düzgün bir şekilde güncelleştirir. Azure Güvenlik Duvarı, varolan bağlantıların kapanması için 90 saniye bekler. Gerekirse, istemciler otomatik olarak başka bir arka uç düğümüne bağlantı yeniden kurabilir.

## <a name="is-there-a-character-limit-for-a-firewall-name"></a>Güvenlik duvarı adı için karakter sınırı var mı?

Evet. Güvenlik duvarı adı için 50 karakter sınırı vardır.

## <a name="why-does-azure-firewall-need-a-26-subnet-size"></a>Azure Güvenlik Duvarı'nın neden bir /26 alt net boyutuna ihtiyacı var?

Azure Güvenlik Duvarı ölçeklendikçe daha fazla sanal makine örneği sağlamalıdır. /26 adres alanı, güvenlik duvarının ölçekleme için yeterli IP adresine sahip olmasını sağlar.

## <a name="does-the-firewall-subnet-size-need-to-change-as-the-service-scales"></a>Hizmet ölçeklendikçe güvenlik duvarı alt ağ boyutunun değişmesi gerekiyor mu?

Hayır. Azure Güvenlik Duvarı'nın /26'dan büyük bir alt ağa ihtiyacı yoktur.

## <a name="how-can-i-increase-my-firewall-throughput"></a>Güvenlik duvarı çıktımı nasıl artırabilirim?

Azure Güvenlik Duvarı'nın ilk iş verme kapasitesi 2,5 - 3 Gbps'dir ve 30 Gbps'ye kadar ölçeklenir. CPU kullanımına ve iş başına göre otomatik olarak ölçeklendirilir.

## <a name="how-long-does-it-take-for-azure-firewall-to-scale-out"></a>Azure Güvenlik Duvarı'nın ölçeklendirmesi ne kadar sürer?

Azure Güvenlik Duvarı, ortalama iş ortası veya CPU tüketimi %60 olduğunda kademeli olarak ölçeklenir. Ölçeklendirme 5-7 dakika sürer. Performans testi yaparken, en az 10 ila 15 dakika test ettiğinizden emin olun ve yeni oluşturulan Güvenlik Duvarı düğümlerinden yararlanmak için yeni bağlantılar başlatın.

## <a name="does-azure-firewall-allow-access-to-active-directory-by-default"></a>Azure Güvenlik Duvarı varsayılan olarak Active Directory'ye erişime izin veriyor mu?

Hayır. Azure Güvenlik Duvarı varsayılan olarak Etkin Dizin erişimini engeller. Erişime izin vermek için AzureActiveDirectory hizmet etiketini yapılandırın. Daha fazla bilgi için Azure [Güvenlik Duvarı hizmet etiketlerini](service-tags.md)görün.

## <a name="can-i-exclude-a-fqdn-or-an-ip-address-from-azure-firewall-threat-intelligence-based-filtering"></a>Azure Güvenlik Duvarı Tehdit İstihbaratı tabanlı filtrelemeden Bir FQDN veya IP adresini hariç tutabilir miyim?

Evet, bunu yapmak için Azure PowerShell'i kullanabilirsiniz:

```azurepowershell
# Add a Threat Intelligence Whitelist to an Existing Azure Firewall

## Create the Whitelist with both FQDN and IPAddresses

$fw = Get-AzFirewall -Name "Name_of_Firewall" -ResourceGroupName "Name_of_ResourceGroup"
$fw.ThreatIntelWhitelist = New-AzFirewallThreatIntelWhitelist `
   -FQDN @("fqdn1", "fqdn2", …) -IpAddress @("ip1", "ip2", …)

## Or Update FQDNs and IpAddresses separately

$fw = Get-AzFirewall -Name "Name_of_Firewall" -ResourceGroupName "Name_of_ResourceGroup"
$fw.ThreatIntelWhitelist.FQDNs = @("fqdn1", "fqdn2", …)
$fw.ThreatIntelWhitelist.IpAddress = @("ip1", "ip2", …)

Set-AzFirewall -AzureFirewall $fw
```

## <a name="why-can-a-tcp-ping-and-similar-tools-successfully-connect-to-a-target-fqdn-even-when-no-rule-on-azure-firewall-allows-that-traffic"></a>Azure Güvenlik Duvarı'ndaki kural bu trafiğe izin vermese bile, TCP ping ve benzeri araçlar neden hedef FQDN'ye başarılı bir şekilde bağlanabilir?

Bir TCP ping aslında hedef FQDN bağlanmıyor. Bunun nedeni, Azure Güvenlik Duvarı'nın saydam proxy'si giden trafik için 80/443 bağlantı noktasında niçin dinler. TCP ping, güvenlik duvarıyla bağlantı kurar ve bu da paketi düşürür ve bağlantıyı kaydeder. Bu davranışın herhangi bir güvenlik etkisi yoktur. Ancak, karışıklığı önlemek için bu davranışta olası değişiklikleri araştırıyoruz.

## <a name="are-there-limits-for-the-number-of-ip-addresses-supported-by-ip-groups"></a>IP Grupları tarafından desteklenen IP adreslerinin sayısı için sınırlar var mı?

Evet. Daha fazla bilgi için Azure [abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-firewall-limits)