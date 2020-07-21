---
title: Azure Güvenlik Duvarı SSS
description: Azure Güvenlik Duvarı hakkında SSS. Azure sanal ağ kaynaklarınızı koruyan, yönetilen, bulut tabanlı bir ağ güvenlik hizmeti.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 07/17/2020
ms.author: victorh
ms.openlocfilehash: b984bb581df54cba79a551dc870786ed228eaa43
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86536978"
---
# <a name="azure-firewall-faq"></a>Azure Güvenlik Duvarı SSS

## <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe sahip ve Kısıtlanmamış bulut ölçeklenebilirliğine sahip, hizmet olarak tam durum bilgisi olan bir güvenlik duvarıdır. Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz.

## <a name="what-capabilities-are-supported-in-azure-firewall"></a>Azure Güvenlik duvarında hangi özellikler destekleniyor?

Azure Güvenlik duvarı özellikleri hakkında bilgi edinmek için bkz. [Azure Güvenlik Duvarı Özellikleri](features.md).

## <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Azure Güvenlik Duvarı için tipik dağıtım modeli nedir?

Azure Güvenlik duvarını herhangi bir sanal ağ üzerinde dağıtabilirsiniz, ancak müşteriler genellikle merkezi bir sanal ağda ve diğer sanal ağlara bir hub ve bağlı bileşen modelinde dağıtır. Daha sonra, eşlenen sanal ağlardan varsayılan yolu bu merkezi güvenlik duvarı sanal ağını işaret etmek üzere ayarlayabilirsiniz. Küresel VNet eşlemesi desteklenir, ancak bölgeler arasında olası performans ve gecikme sorunları nedeniyle önerilmez. En iyi performansı elde etmek için bölge başına bir güvenlik duvarı dağıtın.

Bu modelin avantajı, farklı aboneliklerde birden çok bağlı ağ sanal ağlarına merkezi olarak denetim olanağı sağlar. Ayrıca, her VNet 'te ayrı bir güvenlik duvarı dağıtmanız gerekmiyorsa de maliyet tasarrufları vardır. Maliyet tasarrufları, müşteri trafiği desenlerine bağlı olarak eşleme maliyetini karşılaştırmalı olarak ölçülür.

## <a name="how-can-i-install-the-azure-firewall"></a>Azure Güvenlik duvarını nasıl yükleyebilirim?

Azure portal, PowerShell, REST API veya şablonları kullanarak Azure Güvenlik Duvarı 'nı ayarlayabilirsiniz. Adım adım yönergeler için bkz. [öğretici: Azure Güvenlik Duvarı 'Nı dağıtma ve yapılandırma Azure Portal](tutorial-firewall-deploy-portal.md) .

## <a name="what-are-some-azure-firewall-concepts"></a>Azure Güvenlik Duvarı kavramları nelerdir?

Azure Güvenlik Duvarı, kuralları ve kural koleksiyonlarını destekler. Kural koleksiyonu, aynı sırayı ve önceliği paylaşan bir kurallar kümesidir. Kural koleksiyonları öncelikleri sırasıyla yürütülür. Ağ kuralı koleksiyonları, uygulama kuralı koleksiyonlarından daha yüksek önceliktir ve tüm kurallar sonlandırılıyor.

Üç tür kural koleksiyonu vardır:

* *Uygulama kuralları*: bir alt ağdan erişilebilen tam etki alanı adlarını (FQDN) yapılandırın.
* *Ağ kuralları*: kaynak adreslerini, protokolleri, hedef bağlantı noktalarını ve hedef adreslerini içeren kuralları yapılandırın.
* *NAT kuralları*: gelen Internet bağlantılarına izin vermek için DNAT kurallarını yapılandırın.

## <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Azure Güvenlik Duvarı gelen trafik filtrelemeyi destekliyor mu?

Azure Güvenlik Duvarı gelen ve giden filtrelemeyi destekler. Gelen koruma genellikle HTTP olmayan/S protokolleri için kullanılır. Örneğin, RDP, SSH ve FTP protokolleri. En iyi gelen HTTP/S koruması için [Azure Web uygulaması güvenlik duvarı (WAF)](../web-application-firewall/overview.md)gibi bir Web uygulaması güvenlik duvarı kullanın.

## <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Hangi günlük ve analiz hizmetleri Azure Güvenlik Duvarı tarafından destekleniyor?

Azure Güvenlik Duvarı, güvenlik duvarı günlüklerini görüntülemek ve analiz etmek için Azure Izleyici ile tümleşiktir. Günlükler Log Analytics, Azure depolama veya Event Hubs gönderilebilir. Log Analytics veya Excel ve Power BI gibi farklı araçlarla analiz edilebilir. Daha fazla bilgi için bkz. [öğretici: Azure Güvenlik Duvarı günlüklerini izleme](tutorial-diagnostics.md).

## <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Azure Güvenlik Duvarı Market 'teki NVA 'lar gibi mevcut hizmetlerden farklı şekilde nasıl çalışır?

Azure Güvenlik Duvarı, belirli müşteri senaryolarına yönelik olarak kullanılabilen temel bir güvenlik duvarı hizmetidir. Üçüncü taraf NVA 'lar ve Azure Güvenlik duvarının bir karışımını elde etmeniz beklenmektedir. Birlikte çalışan, temel önceliktir.

## <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Application Gateway WAF ve Azure Güvenlik Duvarı arasındaki fark nedir?

Web uygulaması güvenlik duvarı (WAF), yaygın güvenlik açıklarından ve güvenlik açıklarından Web uygulamalarınızın merkezi bir gelen korumasını sağlayan bir Application Gateway özelliğidir. Azure Güvenlik Duvarı, HTTP/sn olmayan protokoller için gelen koruma sağlar (örneğin, RDP, SSH, FTP), tüm bağlantı noktaları ve protokoller için giden ağ düzeyinde koruma ve giden HTTP/S için uygulama düzeyinde koruma.

## <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Ağ güvenlik grupları (NSG 'ler) ve Azure Güvenlik Duvarı arasındaki fark nedir?

Azure Güvenlik Duvarı hizmeti, ağ güvenlik grubu işlevini tamamlar. Birlikte, daha iyi "derinlemesine savunma" ağ güvenliği sağlar. Ağ güvenlik grupları, her abonelikteki sanal ağlarda bulunan kaynaklarla trafiği sınırlamak için dağıtılmış ağ katmanı trafiği filtrelemesi sağlar. Azure Güvenlik Duvarı, farklı abonelikler ve sanal ağlar arasında ağ ve uygulama düzeyinde koruma sağlayan, hizmet olarak tam durum bilgisi olan merkezi ağ güvenlik duvarıdır.

## <a name="are-network-security-groups-nsgs-supported-on-the-azurefirewallsubnet"></a>Ağ güvenlik grupları (NSG 'ler) AzureFirewallSubnet 'de destekleniyor mu?

Azure Güvenlik Duvarı, NIC düzeyindeki NSG 'ler ile platform koruması (görüntülenemez) dahil olmak üzere birden çok koruma katmanı içeren bir yönetilen hizmettir.  Alt ağ düzeyi NSG 'ler AzureFirewallSubnet üzerinde gerekli değildir ve hizmet kesintisi olmadığından emin olmak için devre dışıdır.

## <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>Azure Güvenlik Duvarı 'Nı hizmet uç noktalarıma göre Nasıl yaparım? mı?

PaaS hizmetlerine güvenli erişim için hizmet uç noktaları önerilir. Azure Güvenlik Duvarı alt ağında hizmet uç noktalarını etkinleştirmeyi ve bağlı bağlı bileşen sanal ağlarında devre dışı bırakmayı tercih edebilirsiniz. Bu şekilde, her iki özellikten de yararlanabilirsiniz: hizmet uç noktası güvenliği ve tüm trafik için merkezi günlük.

## <a name="what-is-the-pricing-for-azure-firewall"></a>Azure Güvenlik Duvarı fiyatlandırması nedir?

Bkz. [Azure Güvenlik Duvarı fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-firewall/).

## <a name="how-can-i-stop-and-start-azure-firewall"></a>Azure Güvenlik duvarını nasıl durdurup başlatabilirim?

Azure PowerShell *serbest bırakma* ve *ayırma* yöntemleri kullanabilirsiniz.

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
> Bir güvenlik duvarını ve genel IP 'yi özgün kaynak grubuna ve aboneliğine yeniden ayırmanız gerekir.

## <a name="what-are-the-known-service-limits"></a>Bilinen hizmet limitleri nelerdir?

Azure Güvenlik Duvarı hizmeti sınırları için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-firewall-limits).

## <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>Bir hub sanal ağında Azure Güvenlik Duvarı 'Nı iletebilir ve iki bağlı bileşen sanal ağı arasındaki ağ trafiğini filtreleyebilir miyim?

Evet, iki bağlı bileşen sanal ağı arasındaki trafiği yönlendirmek ve filtrelemek için bir hub sanal ağında Azure Güvenlik Duvarı 'nı kullanabilirsiniz. Bu senaryonun düzgün çalışması için, bağlı olan sanal ağların her birinde alt ağın Azure Güvenlik duvarını varsayılan bir ağ geçidi olarak işaret eden bir UDR olması gerekir.

## <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network-or-peered-virtual-networks"></a>Azure Güvenlik Duvarı aynı sanal ağdaki veya eşlenen sanal ağlardaki alt ağlar arasında ağ trafiğini iletebilir ve filtreleyebilir mi?

Evet. Ancak, aynı VNET 'teki alt ağlar arasında trafiği yeniden yönlendirmek üzere UDRs 'yi yapılandırmak için ek dikkat gerekir. VNET adres aralığının UDR için hedef ön ek olarak kullanılması yeterli olduğundan, bu, tüm trafiği Azure Güvenlik Duvarı örneği aracılığıyla aynı alt ağdaki başka bir makineye de yönlendirir. Bunu önlemek için, **sanal VNET**'in bir sonraki atlama türü ile UDR 'de alt ağ için bir yol ekleyin. Bu yolların yönetilmesi, bir miktar ve hataya açık olabilir. İç ağ kesimlenmesi için önerilen yöntem, UDRs gerektirmeyen ağ güvenlik gruplarını kullanmaktır.

## <a name="does-azure-firewall-outbound-snat-between-private-networks"></a>Azure Güvenlik Duvarı, özel ağlar arasında SNAT 'ye giden bir mıdır?

Hedef IP adresi, [ıANA RFC 1918](https://tools.ietf.org/html/rfc1918)başına özel bir IP aralığı olduğunda Azure GÜVENLIK duvarı SNAT değildir. Kuruluşunuz özel ağlar için genel bir IP adresi aralığı kullanıyorsa, Azure Güvenlik Duvarı AzureFirewallSubnet 'deki güvenlik duvarı özel IP adreslerinden birine giden trafiği yeniden çıkarır. Azure Güvenlik duvarını, genel IP adresi aralığınızı **SNAT olarak** yapılandırmak için yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Güvenlik DUVARı SNAT özel IP adresi aralıkları](snat-private-range.md).

## <a name="is-forced-tunnelingchaining-to-a-network-virtual-appliance-supported"></a>Bir ağ sanal gerecine Zorlamalı tünel/zincir oluşturma işlemi destekleniyor mu?

Yeni bir güvenlik duvarı oluşturduğunuzda Zorlamalı tünel oluşturma desteklenir. Zorlamalı tünel için mevcut bir güvenlik duvarını yapılandıramazsınız. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı Zorlamalı tünel](forced-tunneling.md). 

Azure Güvenlik duvarının doğrudan Internet bağlantısı olmalıdır. AzureFirewallSubnet, BGP aracılığıyla şirket içi ağınıza varsayılan bir yol öğrenirse, doğrudan Internet bağlantısını sürdürmek için **Nexthoptype** değeri **Internet** olarak ayarlanmış bir 0.0.0.0/0 UDR ile geçersiz kılmanız gerekir.

Yapılandırmanız şirket içi ağa Zorlamalı tünel gerektiriyorsa ve Internet hedeflerine yönelik hedef IP öneklerini belirleyebiliyorsanız, AzureFirewallSubnet üzerinde Kullanıcı tanımlı bir yol aracılığıyla bu aralıkları, şirket içi ağ ile bir sonraki atlama olarak yapılandırabilirsiniz. Ya da bu yolları tanımlamak için BGP kullanabilirsiniz.

## <a name="are-there-any-firewall-resource-group-restrictions"></a>Herhangi bir güvenlik duvarı kaynak grubu kısıtlaması var mı?

Evet. Güvenlik Duvarı, VNet ve genel IP adresi aynı kaynak grubunda olmalıdır.

## <a name="when-configuring-dnat-for-inbound-internet-network-traffic-do-i-also-need-to-configure-a-corresponding-network-rule-to-allow-that-traffic"></a>Gelen Internet ağ trafiği için DNAT yapılandırılırken, bu trafiğe izin vermek için karşılık gelen bir ağ kuralı da yapılandırmam gerekir mi?

Hayır. NAT kuralları, çevrilen trafiğe izin vermek için, karşılık gelen bir ağ kuralını dolaylı olarak ekler. Bu davranışı, çevrilen trafikle eşleşen reddetme kuralları olan bir ağ kural koleksiyonunu açıkça ekleyerek geçersiz kılabilirsiniz. Azure Güvenlik Duvarı kural işleme mantığı hakkında daha fazla bilgi için bkz: [Azure Güvenlik Duvarı kural işleme mantığı](rule-processing.md).

## <a name="how-do-wildcards-work-in-an-application-rule-target-fqdn"></a>Uygulama kuralı hedef FQDN 'de joker karakterler nasıl çalışır?

***. Contoso.com**yapılandırırsanız, *anyvalue*. contoso.com, ancak contoso.com (etki alanı tepesinde) izin verir. Etki alanı tepesinde izin vermek istiyorsanız, onu hedef FQDN olarak açıkça yapılandırmanız gerekir.

## <a name="what-does-provisioning-state-failed-mean"></a>*Sağlama durumu* ne anlama geliyor?

Her bir yapılandırma değişikliği uygulandığında, Azure Güvenlik Duvarı temeldeki tüm arka uç örneklerini güncelleştirmeye çalışır. Nadir durumlarda, bu arka uç örneklerinden biri yeni yapılandırmayla güncelleştiremeyebilir ve güncelleştirme işlemi başarısız bir sağlama durumuyla birlikte durduruluyor. Azure Güvenlik duvarınız hala çalışıyor, ancak uygulanan yapılandırma tutarsız bir durumda olabilir, burada bazı örnekler başkalarının güncelleştirilmiş kural kümesine sahip olduğu önceki yapılandırmaya sahiptir. Bu durumda, işlem başarılı olana ve güvenlik duvarınız *başarılı* bir sağlama durumunda olduğundan, yapılandırmanızı bir kez daha güncelleştirmeyi deneyin.

## <a name="how-does-azure-firewall-handle-planned-maintenance-and-unplanned-failures"></a>Azure Güvenlik Duvarı planlı bakım ve planlanmamış sorunları nasıl işler?
Azure Güvenlik Duvarı, etkin-etkin bir yapılandırmada birkaç arka uç düğümünden oluşur.  Planlı bakım için, düğümleri düzgün şekilde güncelleştirmek için bağlantı boşaltma mantığı vardır.  Güncelleştirmeler, Azure bölgelerinin her biri için iş dışı saatlerde planlanmaktadır ve bu da kesinti riskini daha fazla sınırlar.  Plansız sorunlar için, hatalı düğümü değiştirecek yeni bir düğüm örnekliyoruz.  Yeni düğümle bağlantı, genellikle başarısızlık zamanından 10 saniye içinde yeniden oluşturulur.

## <a name="how-does-connection-draining-work"></a>Bağlantı boşaltma nasıl çalışır?

Planlı bakım için bağlantı boşaltma mantığı, arka uç düğümlerini sorunsuz bir şekilde güncelleştirir. Azure Güvenlik Duvarı, mevcut bağlantıların kapanması için 90 saniye bekler. Gerekirse, istemciler otomatik olarak başka bir arka uç düğümüne yeniden bağlantı kurabilir.

## <a name="is-there-a-character-limit-for-a-firewall-name"></a>Bir güvenlik duvarı adı için bir karakter sınırı var mı?

Evet. Bir güvenlik duvarı adı için 50 karakter sınırı vardır.

## <a name="why-does-azure-firewall-need-a-26-subnet-size"></a>Azure Güvenlik duvarı neden bir/26 alt ağ boyutuna ihtiyaç duyuyor?

Azure Güvenlik duvarının ölçeklendirilen daha fazla sanal makine örneği sağlaması gerekir. A/26 adres alanı, güvenlik duvarının ölçeklendirmeye yetecek sayıda IP adresine sahip olmasını sağlar.

## <a name="does-the-firewall-subnet-size-need-to-change-as-the-service-scales"></a>Hizmet ölçeklendirilirken güvenlik duvarı alt ağı boyutunun değiştirilmesi gerekiyor mu?

Hayır. Azure Güvenlik Duvarı/26 ' dan büyük bir alt ağa gerek yoktur.

## <a name="how-can-i-increase-my-firewall-throughput"></a>Güvenlik Duvarı iş üretimini nasıl artırabilirim?

Azure Güvenlik duvarının ilk üretilen iş verimlilik kapasitesi 2,5-3 Gbps ve 30 Gbps 'e ölçeklendirir. CPU kullanımı ve aktarım hızına göre otomatik olarak ölçeklendirilir.

## <a name="how-long-does-it-take-for-azure-firewall-to-scale-out"></a>Azure Güvenlik duvarının ölçeği ne kadar sürer?

Ortalama üretilen iş veya CPU tüketimi %60 olduğunda Azure Güvenlik Duvarı aşamalı olarak ölçeklendirilir. Ölçeği genişletme beş ila yedi dakika sürer. Performans testi sırasında, en az 10 ila 15 dakika test ettiğinizden emin olun ve yeni oluşturulan güvenlik duvarı düğümlerinden yararlanmak için yeni bağlantılar başlatın.

## <a name="does-azure-firewall-allow-access-to-active-directory-by-default"></a>Azure Güvenlik Duvarı varsayılan olarak Active Directory erişimine izin veriyor mu?

Hayır. Azure Güvenlik Duvarı varsayılan olarak Active Directory erişimi engeller. Erişime izin vermek için AzureActiveDirectory Service etiketini yapılandırın. Daha fazla bilgi için bkz. [Azure Güvenlik Duvarı hizmeti etiketleri](service-tags.md).

## <a name="can-i-exclude-a-fqdn-or-an-ip-address-from-azure-firewall-threat-intelligence-based-filtering"></a>Azure Güvenlik Duvarı tehdit zekası temelinde bir FQDN veya IP adresini dışlayabilir miyim?

Evet, Azure PowerShell kullanarak bunu yapabilirsiniz:

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

## <a name="why-can-a-tcp-ping-and-similar-tools-successfully-connect-to-a-target-fqdn-even-when-no-rule-on-azure-firewall-allows-that-traffic"></a>Azure Güvenlik duvarında hiçbir kural bu trafiğe izin vermediği halde bir TCP ping ve benzer araçlar, bir hedef FQDN 'ye başarıyla bağlanabilir mi?

TCP ping, aslında hedef FQDN 'ye bağlanmamıştır. Bu durum, Azure Güvenlik Duvarı 'nın saydam proxy 'si giden trafik için 80/443 bağlantı noktasını dinlediğinden oluşur. TCP ping, güvenlik duvarıyla bağlantı kurarak paketi bırakır ve bağlantıyı günlüğe kaydeder. Bu davranışın herhangi bir güvenlik etkisi yoktur. Bununla birlikte, karışıklığı önlemek için bu davranıştaki olası değişiklikleri araştırıyoruz.

## <a name="are-there-limits-for-the-number-of-ip-addresses-supported-by-ip-groups"></a>IP grupları tarafından desteklenen IP adresi sayısı için sınırlamalar var mı?

Evet. Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-firewall-limits)

## <a name="can-i-move-an-ip-group-to-another-resource-group"></a>Bir IP grubunu başka bir kaynak grubuna taşıyabilir miyim?

Hayır, bir IP grubunu başka bir kaynak grubuna taşımak Şu anda desteklenmiyor.

## <a name="what-is-the-tcp-idle-timeout-for-azure-firewall"></a>Azure Güvenlik Duvarı için TCP boşta kalma zaman aşımı nedir?

Ağ güvenlik duvarının standart bir davranışı, TCP bağlantılarının etkin tutulmasını ve etkinlik olmaması durumunda bunları hemen kapatmalarını sağlamaktır. Azure Güvenlik Duvarı TCP boşta kalma zaman aşımı dört dakikadır. Bu ayar yapılandırılabilir değildir. İşlem yapılmayan bir süre, zaman aşımı değerinden uzunsa, TCP veya HTTP oturumunun korunduğundan emin olmaz. Ortak bir uygulama, TCP etkin tutma özelliğini kullanmaktır. Bu uygulama, bağlantının daha uzun bir süre için etkin kalmasını önler. Daha fazla bilgi için bkz. [.NET örnekleri](https://docs.microsoft.com/dotnet/api/system.net.servicepoint.settcpkeepalive?redirectedfrom=MSDN&view=netcore-3.1#System_Net_ServicePoint_SetTcpKeepAlive_System_Boolean_System_Int32_System_Int32_).

## <a name="can-i-deploy-azure-firewall-without-a-public-ip-address"></a>Azure Güvenlik duvarını genel bir IP adresi olmadan dağıtabilir miyim?

Hayır, şu anda Azure Güvenlik duvarını genel bir IP adresiyle dağıtmanız gerekir.

## <a name="where-does-azure-firewall-store-customer-data"></a>Azure Güvenlik Duvarı müşteri verilerini nerede depolar?

Azure Güvenlik Duvarı, müşteri verilerini dağıtıldığı bölgenin dışına taşımaz veya depolamaz.