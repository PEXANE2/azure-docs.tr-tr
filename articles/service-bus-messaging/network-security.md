---
title: Azure Servis Veri Servisi için ağ güvenliği
description: Bu makalede, hizmet etiketleri, IP güvenlik duvarı kuralları, hizmet bitiş noktaları ve özel uç noktaları gibi ağ güvenliği özellikleri açıklanmaktadır.
services: service-bus-messaging
documentationcenter: .net
author: axisc
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: aschhab
ms.openlocfilehash: 95f8c2a3b47b59bab7df909be43dacdb1f9c58f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79479286"
---
# <a name="network-security-for-azure-service-bus"></a>Azure Servis Veri Servisi için ağ güvenliği 
Bu makalede, Azure Hizmet Veri Servisi ile aşağıdaki güvenlik özelliklerinin nasıl kullanılacağı açıklanmaktadır: 

- Hizmet etiketleri
- IP Güvenlik Duvarı kuralları
- Ağ hizmeti uç noktaları
- Özel uç noktalar (önizleme)


## <a name="service-tags"></a>Hizmet etiketleri
Hizmet etiketi, belirli bir Azure hizmetinin IP adresi önekleri grubunu temsil eder. Microsoft, hizmet etiketinin kapsadığı adres önekleri yönetir ve adresler değiştikçe servis etiketini otomatik olarak günceller ve sık sık ağ güvenliği kurallarına yönelik güncelleştirmelerin karmaşıklığını en aza indirir. Hizmet etiketleri hakkında daha fazla bilgi için [Hizmet etiketleriyle ilgili genel bakış](../virtual-network/service-tags-overview.md)adabakın.

[Ağ güvenlik gruplarında](../virtual-network/security-overview.md#security-rules) veya [Azure Güvenlik Duvarı'nda](../firewall/service-tags.md)ağ erişim denetimlerini tanımlamak için hizmet etiketlerini kullanabilirsiniz. Güvenlik kuralları oluştururken belirli IP adresleri yerine hizmet etiketlerini kullanın. Bir kuralın uygun *kaynak* veya *hedef* alanında servis etiketi adını (örneğin **ServiceBus)** belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz.

| Hizmet etiketi | Amaç | Gelen veya giden kullanabilir miyim? | Bölgesel olabilir mi? | Azure Güvenlik Duvarı ile kullanılabilir misiniz? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ServiceBus** | Premium hizmet katmanını kullanan Azure Servis Veri Servisi trafiği. | Giden | Evet | Evet |


## <a name="ip-firewall"></a>IP güvenlik duvarı 
Varsayılan olarak, Hizmet Veri Servisi ad alanlarına, istek geçerli kimlik doğrulama ve yetkilendirmeyle birlikte geldiği sürece internetten erişilebilir. IP güvenlik duvarı ile, [cidr (Classless Etki Alanı Yönlendirmesi)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gösteriminde yalnızca bir iPv4 adresi kümesi veya IPv4 adres aralıklarıyla daha da kısıtlayabilirsiniz.

Bu özellik, Azure Servis Veri Tos'unyalnızca belirli tanınmış sitelerden erişilmesi gereken senaryolarda yararlıdır. Güvenlik duvarı kuralları, belirli IPv4 adreslerinden kaynaklanan trafiği kabul etmek için kuralları yapılandırmanızı sağlar. Örneğin, Hizmet Veri Yolu'nun [Azure Ekspres Rotası][ekspres rotası] ile kullanılması durumunda, yalnızca şirket içi altyapı IP adreslerinizden veya kurumsal bir NAT ağ geçidinin adreslerinden gelen trafiğe izin vermek için bir **güvenlik duvarı kuralı** oluşturabilirsiniz. 

IP güvenlik duvarı kuralları Hizmet Veri Servisi ad alanı düzeyinde uygulanır. Bu nedenle, kurallar desteklenen herhangi bir protokol kullanarak istemcilerden gelen tüm bağlantılar için geçerlidir. Hizmet Veri Gönderisi ad alanında izin verilen bir IP kuralıyla eşleşmeyen bir IP adresinden gelen bağlantı girişimi yetkisiz olarak reddedilir. Yanıtta IP kuralından bahsedilmez. Sırayla IP filtresi kuralları uygulanır ve IP adresiyle eşleşen ilk kural kabul veya reddetme eylemini belirler.

Daha fazla bilgi için, [Hizmet Veri Servisi ad alanı için IP güvenlik duvarını nasıl yapılandırabilirsiniz](service-bus-ip-filtering.md)

## <a name="network-service-endpoints"></a>Ağ hizmeti uç noktaları
Servis Veri Yolu'nun [Sanal Ağ (VNet) hizmet bitiş noktalarıyla](service-bus-service-endpoints.md) tümleştirilmesi, sanal ağlara bağlı sanal makineler gibi iş yüklerinden mesajlaşma yeteneklerine güvenli erişim sağlar ve ağ trafik yolu her iki uçta da güvenli hale gelir.

En az bir sanal ağ altağ hizmeti bitiş noktasına bağlanacak şekilde yapılandırıldıktan sonra, ilgili Servis Veri Aracı ad alanı artık herhangi bir yerden değil, yetkili sanal ağ(lar) trafiğini kabul eder. Sanal ağ açısından bakıldığında, Hizmet Veri Sitesi ad alanını hizmet bitiş noktasına bağlama, sanal ağ alt ağından mesajlaşma hizmetine yalıtılmış bir ağ tünelini yapılandırır.

Sonuç, ileti hizmeti bitiş noktasının genel BIR IP aralığında olmasına rağmen, alt ağa bağlı iş yükleri ile ilgili Service Bus ad alanı arasındaki özel ve yalıtılmış bir ilişkidir.

> [!IMPORTANT]
> Sanal Ağlar yalnızca [Premium katman](service-bus-premium-messaging.md) Hizmet Veri Günü ad alanlarında desteklenir.
> 
> Servis Veri Servisi ile VNet hizmet bitiş noktalarını kullanırken, Standart ve Premium katman Hizmet Veri Mes'ü ad alanlarını karıştıran uygulamalarda bu uç noktaları etkinleştirmemelisiniz. Standart katman VNet'leri desteklemediği için. Bitiş noktası yalnızca Premium katman ad alanlarıyla sınırlıdır.

### <a name="advanced-security-scenarios-enabled-by-vnet-integration"></a>VNet tümleştirmesi tarafından etkinleştirilen gelişmiş güvenlik senaryoları 

Sıkı ve bölümlere ayrılmış güvenlik gerektiren ve sanal ağ alt ağlarının bölümlere ayrılmış hizmetler arasındaki segmentasyonu sağladığı çözümler, genellikle bu bölmelerde bulunan hizmetler arasında genellikle iletişim yolları gerektirir.

TCP/IP üzerinden HTTPS taşıyanlar da dahil olmak üzere bölmeler arasındaki herhangi bir anlık IP rotası, ağ katmanındaki güvenlik açıklarından yararlanma riski taşır. Mesajlaşma hizmetleri, iletilerin taraflar arasında geçiş sırasında diske bile yazıldığı tamamen yalıtımlı iletişim yolları sağlar. Her ikisi de aynı Hizmet Veri Yolu örneğine bağlı olan iki farklı sanal ağdaki iş yükleri, iletiler aracılığıyla verimli ve güvenilir bir şekilde iletişim kurabilir ve ilgili ağ yalıtımsınır bütünlüğü korunur.
 
Bu, güvenliğe duyarlı bulut çözümlerinizin yalnızca Azure endüstri lideri güvenilir ve ölçeklenebilir eşzamanlı mesajlaşma özelliklerine erişmedikleri, aynı zamanda artık mesajlaşmayı güvenli çözüm bölmeleri arasında iletişim yolları oluşturmak için kullanabileceği anlamına gelir. HTTPS ve diğer TLS güvenlikli soket protokolleri de dahil olmak üzere eşler arası iletişim moduyla ulaşılabileceklerden daha güvenlidir.

### <a name="bind-service-bus-to-virtual-networks"></a>Servis Veri Toplarını Sanal Ağlara Bağlama

*Sanal ağ kuralları,* Azure Hizmet Veri Servisi sunucunuzun belirli bir sanal ağ alt ağından bağlantıları kabul edip etmediğini kontrol eden güvenlik duvarı güvenlik özelliğidir.

Hizmet Veri Sitesi ad alanını sanal ağa bağlamaiki aşamalı bir işlemdir. Öncelikle bir Sanal **Ağ** alt ağında bir Sanal Ağ hizmet bitiş noktası oluşturmanız ve hizmet bitiş noktasına genel [bakışta](service-bus-service-endpoints.md)açıklandığı gibi **Microsoft.ServiceBus** için etkinleştirmeniz gerekir. Hizmet bitiş noktasını ekledikten sonra, Servis Veri Sitesi ad alanını **sanal ağ kuralıyla**ona bağlarsınız.

Sanal ağ kuralı, Hizmet Veri Mesle'nin ad alanının sanal ağ alt ağıyla ilişkisidir. Kural olsa da, alt ağa bağlı tüm iş yüklerine Hizmet Veri Yolundan ad alanına erişim izni verilir. Service Bus'un kendisi hiçbir zaman giden bağlantılar kurmaz, erişim sağlaması gerekmez ve bu nedenle bu kuralı etkinleştirerek alt ağınıza erişim izni verilmez.

Daha fazla bilgi için, [Hizmet Veri Sitesi ad alanı için sanal ağ hizmeti bitiş noktalarını nasıl yapılandırabilirsiniz](service-bus-service-endpoints.md)

## <a name="private-endpoints"></a>Özel uç noktalar

Azure Özel Bağlantı Hizmeti, Azure hizmetlerine (örneğin, Azure Servis Veri Mes'i, Azure Cosmos Depolama ve Azure Cosmos DB) ve Azure barındırılan müşteri/iş ortağı hizmetlerine sanal **ağınızdaki özel** bir bitiş noktası üzerinden erişmenizi sağlar.

Özel bitiş noktası, sizi Azure Özel Bağlantısı ile çalışan bir hizmete özel ve güvenli bir şekilde bağlayan bir ağ arabirimidir. Özel bitiş noktası, VNet'inizden gelen özel bir IP adresini kullanır ve hizmeti VNet'inize etkin bir şekilde getirir. Hizmete giden tüm trafik özel bitiş noktasından yönlendirilebilir, bu nedenle ağ geçidi, NAT aygıtları, ExpressRoute veya VPN bağlantıları veya genel IP adresleri gerekmez. Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağı üzerinden geçer ve genel İnternet’ten etkilenme olasılığı ortadan kaldırılır. Bir Azure kaynağıörneğine bağlanarak erişim denetiminde en yüksek parçalılık düzeyini sağlayabilirsiniz.

Daha fazla bilgi için Azure [Özel Bağlantı nedir?](../private-link/private-link-overview.md)

> [!NOTE]
> Bu özellik, Azure Hizmet Veri Servisi'nin **birinci sınıf** katmanıyla desteklenir. Premium katman hakkında daha fazla bilgi için [Service Bus Premium ve Standart mesajlaşma katmanları](service-bus-premium-messaging.md) makalesine bakın.
>
> Bu özellik şu anda **önizlemede.** 


Daha fazla bilgi için, [Hizmet Veri Aracı ad alanı için özel uç noktaları nasıl yapılandırılabilirsiniz](private-link-service.md)


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Hizmet Veri Sesi ad alanı için IP güvenlik duvarı nasıl yapılandırılabilen](service-bus-ip-filtering.md)
- [Hizmet Veri Sitesi ad alanı için sanal ağ hizmeti bitiş noktaları nasıl yapılandırılır?](service-bus-service-endpoints.md)
- [Hizmet Veri Servisi ad alanı için özel uç noktaları yapılandırma](private-link-service.md)
