---
title: Azure Etkinlik Hub'ları için ağ güvenliği
description: Bu makalede, özel uç noktalardan erişim yapılandırmak için nasıl açıklanır
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: spelluru
ms.openlocfilehash: ab85cdb2854de5c147c68afd8e4fe5e17ac2899b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477947"
---
# <a name="network-security-for-azure-event-hubs"></a>Azure Etkinlik Hub'ları için ağ güvenliği 
Bu makalede, Azure Olay Hub'ları ile aşağıdaki güvenlik özelliklerinin nasıl kullanılacağı açıklanmaktadır: 

- Hizmet etiketleri
- IP Güvenlik Duvarı kuralları
- Ağ hizmeti uç noktaları
- Özel uç noktalar (önizleme)


## <a name="service-tags"></a>Hizmet etiketleri
Hizmet etiketi, belirli bir Azure hizmetinin IP adresi önekleri grubunu temsil eder. Microsoft, hizmet etiketinin kapsadığı adres önekleri yönetir ve adresler değiştikçe servis etiketini otomatik olarak günceller ve sık sık ağ güvenliği kurallarına yönelik güncelleştirmelerin karmaşıklığını en aza indirir. Hizmet etiketleri hakkında daha fazla bilgi için [Hizmet etiketleriyle ilgili genel bakış](../virtual-network/service-tags-overview.md)adabakın.

[Ağ güvenlik gruplarında](../virtual-network/security-overview.md#security-rules) veya [Azure Güvenlik Duvarı'nda](../firewall/service-tags.md)ağ erişim denetimlerini tanımlamak için hizmet etiketlerini kullanabilirsiniz. Güvenlik kuralları oluştururken belirli IP adresleri yerine hizmet etiketlerini kullanın. Bir kuralın uygun *kaynak* veya *hedef* alanında hizmet etiketi adını (örneğin **EventHub)** belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz.

| Hizmet etiketi | Amaç | Gelen veya giden kullanabilir miyim? | Bölgesel olabilir mi? | Azure Güvenlik Duvarı ile kullanılabilir misiniz? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **EventHub** | Azure Etkinlik Hub'ları. | Giden | Evet | Evet |


## <a name="ip-firewall"></a>IP güvenlik duvarı 
Varsayılan olarak, İstek geçerli kimlik doğrulama ve yetkilendirmeyle birlikte geldiği sürece, Olay Hub'ları ad alanlarına internetten erişilebilir. IP güvenlik duvarı ile, [cidr (Classless Etki Alanı Yönlendirmesi)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir iPv4 adresi kümesi veya IPv4 adres aralıklarıyla daha da kısıtlayabilirsiniz.

Bu özellik, Azure Etkinlik Hub'larına yalnızca belirli tanınmış sitelerden erişilmesi gereken senaryolarda yararlıdır. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, [Azure Express Route][express-route] ile Olay Hub'ları kullanıyorsanız, yalnızca şirket içi altyapı IP adreslerinizden gelen trafiğe izin vermek için bir **güvenlik duvarı kuralı** oluşturabilirsiniz. 

IP güvenlik duvarı kuralları Olay Hub'ları ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokol kullanarak istemcilerden gelen tüm bağlantılar için geçerlidir. Olay Hub'ları ad alanında izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimi yetkisiz olarak reddedilir. Yanıtta IP kuralından bahsedilmez. Sırayla IP filtresi kuralları uygulanır ve IP adresiyle eşleşen ilk kural kabul veya reddetme eylemini belirler.

Daha fazla bilgi için, [bir olay hub'ı için IP güvenlik duvarını nasıl yapılandırabilirsiniz](event-hubs-ip-filtering.md)

## <a name="network-service-endpoints"></a>Ağ hizmeti uç noktaları
Olay Hub'larının [Virtual Network (VNet) Service Endpoints][vnet-sep] ile entegrasyonu, sanal ağlara bağlı sanal makineler gibi iş yüklerinden mesajlaşma yeteneklerine güvenli erişim sağlar ve ağ trafiği yolu güvenli hale gelir her iki uçta da.

En az bir sanal ağ alt ağ hizmeti bitiş noktasına bağlı olarak yapılandırıldıktan sonra, ilgili Olay Hub'ları ad alanı artık sanal ağlarda her yerden gelen trafiği kabul etmez. Sanal ağ açısından bakıldığında, Olay Hub'larının ad alanını hizmet bitiş noktasına bağlama, sanal ağ alt ağından ileti hizmetine yalıtılmış bir ağ tünelini yapılandırır. 

Sonuç, ileti hizmeti bitiş noktasının genel BIR IP aralığında olmasına rağmen, alt ağa ve ilgili Olay Hub'larına bağlı iş yükleri arasındaki özel ve yalıtılmış bir ilişkidir. Bu davranışın bir istisnası vardır. Varsayılan olarak bir hizmet bitiş noktasıetkinleştirmek, sanal ağile ilişkili [IP güvenlik duvarındaki](event-hubs-ip-filtering.md) `denyall` kuralı etkinleştirir. Olay Hub ortak bitiş noktasına erişimi etkinleştirmek için IP güvenlik duvarına belirli IP adresleri ekleyebilirsiniz. 

> [!IMPORTANT]
> Sanal ağlar, Etkinlik Hub'larının **standart** ve **özel** katmanlarında desteklenir. **Temel** katmanda desteklenmiyor.

### <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi tarafından etkinleştirilen gelişmiş güvenlik senaryoları 

Sıkı ve bölümlere ayrılmış güvenlik gerektiren ve sanal ağ alt ağlarının bölümlere ayrılmış hizmetler arasındaki segmentasyonu sağladığı çözümler, yine de bu bölmelerde bulunan hizmetler arasında iletişim yolları gerektirir.

TCP/IP üzerinden HTTPS taşıyanlar da dahil olmak üzere bölmeler arasındaki herhangi bir anlık IP rotası, ağ katmanındaki güvenlik açıklarından yararlanma riski taşır. Mesajlaşma hizmetleri, iletilerin taraflar arasında geçiş sırasında diske bile yazıldığı yalıtımlı iletişim yolları sağlar. Her ikisi de aynı Olay Hub'ları örneğine bağlı olan iki farklı sanal ağdaki iş yükleri, iletiler aracılığıyla verimli ve güvenilir bir şekilde iletişim kurabilir ve ilgili ağ yalıtım sınır bütünlüğü korunur.
 
Bu, güvenliğe duyarlı bulut çözümlerinizin yalnızca Azure endüstri lideri güvenilir ve ölçeklenebilir eşzamanlı mesajlaşma özelliklerine erişmedikleri, aynı zamanda artık mesajlaşmayı güvenli çözüm bölmeleri arasında iletişim yolları oluşturmak için kullanabileceği anlamına gelir. HTTPS ve diğer TLS güvenlikli soket protokolleri de dahil olmak üzere eşler arası iletişim moduyla ulaşılabileceklerden daha güvenlidir.

### <a name="bind-event-hubs-to-virtual-networks"></a>Olay hub'larını sanal ağlara bağlama

**Sanal ağ kuralları,** Azure Etkinlik Hub'larınızın belirli bir sanal ağ alt ağındaki bağlantıları kabul edip etmediğini kontrol eden güvenlik duvarı güvenlik özelliğidir.

Olay Hub'larının ad alanını sanal ağa bağlamaiki adımlı bir işlemdir. Öncelikle bir sanal ağın alt ağında sanal bir **Ağ hizmeti bitiş noktası** oluşturmanız ve [hizmet bitiş noktası genel bakışı][vnet-sep] makalesinde açıklandığı gibi **Microsoft.EventHub** için etkinleştirmeniz gerekir. Hizmet bitiş noktasını ekledikten sonra, Olay Hub'ları ad alanını **sanal ağ kuralıyla**ona bağlarsınız.

Sanal ağ kuralı, Olay Hub'larının ad alanının sanal ağ alt ağıyla ilişkisidir. Kural mevcut olsa da, alt ağa bağlı tüm iş yüklerine Olay Hub'ları ad alanına erişim hakkı verilir. Olay Hub'larının kendisi hiçbir zaman giden bağlantılar kurmaz, erişim sağlaması gerekmez ve bu nedenle bu kuralı etkinleştirerek alt ağınıza erişim izni verilmez.

Daha fazla bilgi için, [bir olay hub'ı için sanal ağ hizmeti uç noktalarını nasıl yapılandırabilirsiniz](event-hubs-service-endpoints.md)

## <a name="private-endpoints"></a>Özel uç noktalar

[Azure Özel Bağlantı hizmeti,](../private-link/private-link-overview.md) Azure Hizmetlerine (örneğin, Azure Etkinlik Hub'ları, Azure Azure Depolama ve Azure Cosmos DB) ve Azure barındırılan müşteri/iş ortağı hizmetlerine sanal ağınızdaki özel bir **bitiş noktası** üzerinden erişmenizi sağlar.

Özel bitiş noktası, sizi Azure Özel Bağlantısı ile çalışan bir hizmete özel ve güvenli bir şekilde bağlayan bir ağ arabirimidir. Özel bitiş noktası, VNet'inizden gelen özel bir IP adresini kullanır ve hizmeti VNet'inize etkin bir şekilde getirir. Hizmete giden tüm trafik özel bitiş noktasından yönlendirilebilir, bu nedenle ağ geçidi, NAT aygıtları, ExpressRoute veya VPN bağlantıları veya genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağıörneğine bağlanarak erişim denetiminde en yüksek parçalılık düzeyini sağlayabilirsiniz.

> [!NOTE]
> Bu özellik yalnızca **özel** katmanla desteklenir. İlgili katman hakkında daha fazla bilgi için, [Özel Etkinlik Hub'larına Genel Bakış'a](event-hubs-dedicated-overview.md)bakın. 
>
> Bu özellik şu anda **önizlemede.** 


Daha fazla bilgi için, [bir olay hub'ı için özel uç noktaları nasıl yapılandırılabilirsiniz](private-link-service.md)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir olay hub'ı için IP güvenlik duvarı nasıl yapılandırılabilen](event-hubs-ip-filtering.md)
- [Bir olay hub'ı için sanal ağ hizmeti uç noktaları nasıl yapılandırılabilen](event-hubs-service-endpoints.md)
- [Olay hub'ı için özel uç noktaları yapılandırma](private-link-service.md)