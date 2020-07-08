---
title: Azure Service Bus için ağ güvenliği
description: Bu makalede hizmet etiketleri, IP güvenlik duvarı kuralları, hizmet uç noktaları ve özel uç noktalar gibi ağ güvenlik özellikleri açıklanmaktadır.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 731300179ce9a0ff72169cdad5c7c039749b20f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341132"
---
# <a name="network-security-for-azure-service-bus"></a>Azure Service Bus için ağ güvenliği 
Bu makalede, Azure Service Bus ile aşağıdaki güvenlik özelliklerinin nasıl kullanılacağı açıklanmaktadır: 

- Hizmet etiketleri
- IP güvenlik duvarı kuralları
- Ağ Hizmeti uç noktaları
- Özel uç noktalar (Önizleme)


## <a name="service-tags"></a>Hizmet etiketleri
Hizmet etiketi, belirli bir Azure hizmetinden bir IP adresi önekleri grubunu temsil eder. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir ve ağ güvenlik kuralları için sık sık güncelleştirmelerin karmaşıklığını en aza indirir. Hizmet etiketleri hakkında daha fazla bilgi için bkz. [hizmet etiketlerine genel bakış](../virtual-network/service-tags-overview.md).

[Ağ güvenlik gruplarında](../virtual-network/security-overview.md#security-rules) veya [Azure Güvenlik duvarında](../firewall/service-tags.md)ağ erişim denetimleri tanımlamak için hizmet etiketlerini kullanabilirsiniz. Güvenlik kuralları oluştururken belirli IP adreslerinin yerine hizmet etiketleri kullanın. Bir kuralın uygun *kaynak* veya *hedef* alanındaki hizmet etiketi adını (örneğin, **ServiceBus**) belirterek, karşılık gelen hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz.

| Hizmet etiketi | Amaç | Gelen veya giden trafiği kullanabilir miyim? | Bölgesel olabilir mi? | Azure Güvenlik Duvarı ile kullanılabilir mi? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ServiceBus** | Premium hizmet katmanını kullanan trafiği Azure Service Bus. | Giden | Evet | Evet |


> [!NOTE]
> Yalnızca **Premium** ad alanları için hizmet etiketlerini kullanabilirsiniz. **Standart** bir ad alanı kullanıyorsanız, aşağıdaki komutu ÇALıŞTıRDıĞıNıZDA gördüğünüz IP adresini kullanın: `nslookup <host name for the namespace>` . Örneğin: `nslookup contosons.servicebus.windows.net`. 

## <a name="ip-firewall"></a>IP güvenlik duvarı 
Varsayılan olarak, istek geçerli kimlik doğrulaması ve yetkilendirmeyle geldiği sürece, Service Bus ad alanlarına internet 'ten erişilebilir. IP güvenlik duvarı ile, [CIDR (sınıfsız etki alanları arası yönlendirme)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir dizi IPv4 adresi veya IPv4 adres aralığı ile sınırlayabilirsiniz.

Bu özellik, Azure Service Bus yalnızca belirli bilinen sitelerden erişilebilir olması gereken senaryolarda yararlıdır. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, [Azure Express Route] [Express-Route] ile Service Bus kullanırsanız, yalnızca şirket içi altyapı IP adreslerinizin veya bir kurumsal NAT ağ geçidinin adreslerinden gelen trafiğe izin vermek için bir **güvenlik duvarı kuralı** oluşturabilirsiniz. 

IP güvenlik duvarı kuralları Service Bus ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokolü kullanarak istemcilerden gelen tüm bağlantılara uygulanır. Service Bus ad alanındaki izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimleri yetkisiz olarak reddedilir. Yanıt, IP kuralından bahsetmiyor. IP filtresi kuralları sırasıyla uygulanır ve IP adresiyle eşleşen ilk kural kabul etme veya reddetme eylemini belirler.

Daha fazla bilgi için bkz. [Service Bus ad alanı IÇIN IP güvenlik duvarını yapılandırma](service-bus-ip-filtering.md)

## <a name="network-service-endpoints"></a>Ağ Hizmeti uç noktaları
[Sanal ağ (VNet) hizmet uç noktaları](service-bus-service-endpoints.md) ile Service Bus tümleştirmesi, sanal ağlara bağlı sanal makineler gibi iş yüklerinden, her iki uçta da güvenli hale getirilen ağ trafiği yolu ile güvenli erişim sağlar.

En az bir sanal ağ alt ağ hizmeti uç noktasına bağlanacak şekilde yapılandırıldıktan sonra, ilgili Service Bus ad alanı artık herhangi bir yerden trafiği kabul etmez, ancak yetkilendirilmiş sanal ağ (ler) i. Sanal ağ perspektifinden bir Service Bus ad alanını bir hizmet uç noktasına bağlamak, sanal ağ alt ağından mesajlaşma hizmetine yalıtılmış bir ağ tüneli yapılandırır.

Sonuç olarak, alt ağa ve ilgili Service Bus ad alanıyla ilişkili olan iş yükleri arasında özel ve yalıtılmış bir ilişki vardır. Bu, bir genel IP aralığında yer alan mesajlaşma hizmeti uç noktasının observable ağ adresi artma.

> [!IMPORTANT]
> Sanal ağlar yalnızca [Premium katmanda](service-bus-premium-messaging.md) Service Bus ad alanlarında desteklenir.
> 
> VNet hizmet uç noktalarını Service Bus kullanılırken, standart ve Premium katman Service Bus ad alanlarını karıştıran uygulamalarda bu uç noktaları etkinleştirmemelisiniz. Standart katman VNET 'leri desteklemediğinden. Uç nokta yalnızca Premium katman ad alanları ile kısıtlıdır.

### <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi tarafından etkinleştirilen gelişmiş güvenlik senaryoları 

Sıkı ve compartmenbir güvenlik gerektiren ve sanal ağ alt ağlarının, compartmenhizmeti arasında segmentleme sağladığı çözümler, genellikle bu bölmeleri bulunan hizmetler arasında iletişim yollarına gerek duyar.

TCP/IP üzerinden HTTPS 'yi yürüten bu dahil olmak üzere, bölmeleri arasındaki tüm anında IP rotası, üzerindeki ağ katmanından güvenlik açıklarına karşı yararlanma riskini taşır. Mesajlaşma Hizmetleri, iletiler, taraflar arasında geçiş yaparken iletilerin diske yazıldığı, tamamen yalıtılmış iletişim yolları sağlar. Aynı Service Bus örneğine bağlanan iki ayrı sanal ağdaki iş yükleri iletiler aracılığıyla verimli ve güvenilir bir şekilde iletişim kurabilir, ancak ilgili ağ yalıtımı sınır bütünlüğü korunur.
 
Bu, güvenlik duyarlı bulut çözümlerinizin yalnızca Azure sektör lideri güvenilir ve ölçeklenebilir zaman uyumsuz mesajlaşma özelliklerine erişim elde edemeyeceği anlamına gelir, ancak artık HTTPS ve diğer TLS güvenlikli yuva protokolleri de dahil olmak üzere herhangi bir eşler arası iletişim moduyla ulaşılabilir 'dan daha güvenli olan güvenli çözüm bölmeleri arasında iletişim yolları oluşturmak için mesajlaşma kullanabilir.

### <a name="bind-service-bus-to-virtual-networks"></a>Service Bus sanal ağlara bağlama

*Sanal ağ kuralları* , Azure Service Bus sunucunuzun belirli bir sanal ağ alt ağından gelen bağlantıları kabul edip etmediğini denetleyen güvenlik duvarı güvenlik özelliğidir.

Bir Service Bus ad alanını bir sanal ağa bağlamak iki adımlı bir işlemdir. Önce bir sanal ağ alt ağında bir **sanal ağ hizmeti uç noktası** oluşturmanız ve [hizmet uç noktasına genel bakış](service-bus-service-endpoints.md)bölümünde açıklandığı gibi **Microsoft. ServiceBus** için etkinleştirmeniz gerekir. Hizmet uç noktasını ekledikten sonra, Service Bus ad alanını bir **sanal ağ kuralıyla**bağlayın.

Sanal ağ kuralı, bir sanal ağ alt ağıyla Service Bus ad alanının bir ilişkidir. Kural var olsa da, alt ağa erişen tüm iş yükleri Service Bus ad alanına erişim izni verilir. Service Bus kendisi hiçbir şekilde giden bağlantı oluşturmaz, erişim elde etmek zorunda değildir ve bu nedenle bu kuralı etkinleştirerek alt ağınız için hiçbir şekilde erişim izni verilmez.

Daha fazla bilgi için bkz. [Service Bus ad alanı için sanal ağ hizmet uç noktalarını yapılandırma](service-bus-service-endpoints.md)

## <a name="private-endpoints"></a>Özel uç noktalar

Azure özel bağlantı hizmeti, Azure hizmetlerine (örneğin, Azure Service Bus, Azure depolama ve Azure Cosmos DB) ve Azure 'da barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki **özel bir uç nokta** üzerinden erişmenizi sağlar.

Özel uç nokta, Azure özel bağlantısı tarafından desteklenen bir hizmete özel ve güvenli bir şekilde bağlanan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor. Hizmete giden tüm trafik özel uç nokta aracılığıyla yönlendirilebilir, bu nedenle ağ geçitleri, NAT cihazları, ExpressRoute veya VPN bağlantıları ya da genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağı örneğine bağlanarak, erişim denetimi için en yüksek düzeyde ayrıntı düzeyi sağlayabilirsiniz.

Daha fazla bilgi için bkz. [Azure özel bağlantısı nedir?](../private-link/private-link-overview.md)

> [!NOTE]
> Bu özellik, Azure Service Bus **Premium** katmanıyla desteklenir. Premium katmanı hakkında daha fazla bilgi için, [Service Bus Premium ve standart mesajlaşma katmanları](service-bus-premium-messaging.md) makalesine bakın.
>
> Bu özellik şu anda **Önizleme**aşamasındadır. 


Daha fazla bilgi için bkz. [Service Bus ad alanı için özel uç noktaları yapılandırma](private-link-service.md)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Service Bus ad alanı için IP güvenlik duvarını yapılandırma](service-bus-ip-filtering.md)
- [Service Bus ad alanı için sanal ağ hizmet uç noktalarını yapılandırma](service-bus-service-endpoints.md)
- [Service Bus ad alanı için özel uç noktaları yapılandırma](private-link-service.md)
