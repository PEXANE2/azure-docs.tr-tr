---
title: Azure Işlevleri ağ seçenekleri
description: Azure Işlevlerinde kullanılabilen tüm ağ seçeneklerine genel bakış
author: alexkarcher-msft
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: bf5ce8da2ce62a5da821588c8f635bbab04dd3c1
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72881565"
---
# <a name="azure-functions-networking-options"></a>Azure Işlevleri ağ seçenekleri

Bu makalede, Azure Işlevleri için barındırma seçenekleri genelinde kullanılabilen ağ özellikleri açıklanmaktadır. Aşağıdaki ağ seçeneklerinin tümü, internet yönlendirilebilir adreslerini kullanmadan kaynaklara erişim olanağı sağlar veya bir işlev uygulamasına internet erişimini kısıtlayabilir.

Barındırma modellerinin farklı düzeylerde ağ yalıtımı vardır. Doğru olanı seçtiğinizde, ağ yalıtımı gereksinimlerinizi karşılamanıza yardımcı olur.

İşlev uygulamalarını birkaç yolla barındırabilirsiniz:

* Çeşitli düzeylerde sanal ağ bağlantısı ve ölçeklendirme seçenekleri içeren çok kiracılı bir altyapıda çalışan bir plan seçenekleri kümesi vardır:
    * Yükleme için dinamik olarak ölçeklendirilen ve en düşük ağ yalıtımı seçeneklerini sunan [Tüketim planı](functions-scale.md#consumption-plan).
    * Ayrıca, dinamik olarak ölçeklendirilirken, daha kapsamlı ağ yalıtımı sunan [Premium planı](functions-scale.md#premium-plan).
    * Sabit ölçekte çalışan ve Premium plana benzer ağ yalıtımı sunan Azure [App Service planı](functions-scale.md#app-service-plan).
* İşlevleri bir [App Service ortamı](../app-service/environment/intro.md)çalıştırabilirsiniz. Bu yöntem, işlevinizi sanal ağınıza dağıtır ve tam ağ denetimi ve yalıtımı sağlar.

## <a name="matrix-of-networking-features"></a>Ağ özellikleri matrisi

|                |[Tüketim planı](functions-scale.md#consumption-plan)|[Premium plan (Önizleme)](functions-scale.md#premium-plan)|[App Service planı](functions-scale.md#app-service-plan)|[App Service Ortamı](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Özel site erişimini & gelen IP kısıtlamaları](#inbound-ip-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|
|[Sanal ağ tümleştirmesi](#virtual-network-integration)|❌No|✅Yes (bölgesel)|✅Yes (bölgesel ve ağ geçidi)|✅Yes|
|[Sanal ağ Tetikleyicileri (HTTP olmayan)](#virtual-network-triggers-non-http)|❌No| ❌No|✅Yes|✅Yes|
|[Karma Bağlantılar](#hybrid-connections)|❌No|✅Yes|✅Yes|✅Yes|
|[Giden IP kısıtlamaları](#outbound-ip-restrictions)|❌No| ❌No|❌No|✅Yes|

## <a name="inbound-ip-restrictions"></a>Gelen IP kısıtlamaları

Uygulamanıza erişimine izin verilen/reddedilen IP adreslerinin öncelik sırasına sahip bir listesini tanımlamak için IP kısıtlamalarını kullanabilirsiniz. Liste, IPv4 ve IPv6 adreslerini içerebilir. Bir veya daha fazla giriş olduğunda, listenin sonunda örtülü bir "Tümünü Reddet" bulunur. IP kısıtlamaları tüm işlev barındırma seçenekleriyle çalışır.

> [!NOTE]
> Ağ kısıtlamaları olduğunda, yalnızca sanal ağınızdaki Portal düzenleyicisini kullanabilir veya Azure portal erişmek için kullandığınız makinenin IP 'sini daha sonra listesiniz demektir. Ancak, herhangi bir makineden **platform özellikleri** sekmesindeki herhangi bir özelliğe erişmeye devam edebilirsiniz.

Daha fazla bilgi için bkz. [Azure App Service statik erişim kısıtlamaları](../app-service/app-service-ip-restrictions.md).

## <a name="private-site-access"></a>Özel site erişimi

Özel site erişimi, uygulamanızı Azure sanal ağı içindeki gibi özel bir ağdan erişilebilir hale getirme anlamına gelir.

* Özel site erişimi, **hizmet uç noktaları** yapılandırıldığında [Premium](./functions-premium-plan.md), [Tüketim](functions-scale.md#consumption-plan) ve [App Service planınızdan](functions-scale.md#app-service-plan) kullanılabilir.
    * Hizmet uç noktaları, platform özellikleri > Ağ Iletişimi > erişim kısıtlamalarını Yapılandırma > kural ekle ' nin altında uygulama temelinde yapılandırılabilir. Sanal ağlar artık kuralın "türü" olarak seçilebilir.
    * Daha fazla bilgi için bkz. [sanal ağ hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)
        * Hizmet uç noktalarında, işlevinizin hala internet 'e yönelik sanal ağ tümleştirmesiyle birlikte tam giden erişimi olduğunu aklınızda bulundurun.
* Özel site erişimi, iç yük dengeleyici (ıLB) ile yapılandırılmış bir App Service Ortamı de mevcuttur. Daha fazla bilgi için bkz. [bir App Service ortamı iç yük dengeleyici oluşturma ve kullanma](../app-service/environment/create-ilb-ase.md).

## <a name="virtual-network-integration"></a>Sanal ağ tümleştirmesi

Sanal Ağ tümleştirmesi, işlev uygulamanızın bir sanal ağ içindeki kaynaklara erişmesine olanak sağlar. Bu özellik hem Premium planda hem de App Service planında kullanılabilir. Uygulamanız bir App Service Ortamı ise, zaten bir sanal ağda bulunur ve aynı sanal ağdaki kaynaklara ulaşmak için sanal ağ tümleştirmesinin kullanılmasını gerektirmez.

Sanal ağ tümleştirmesini, uygulamalardan sanal ağınızda çalışan veritabanlarına ve Web hizmetlerine erişimi etkinleştirmek için kullanabilirsiniz. Sanal ağ tümleştirmesiyle, VM 'inizdeki uygulamalar için genel bir uç nokta kullanıma almanız gerekmez. Bunun yerine özel, internet dışı yönlendirilebilir adreslerini kullanabilirsiniz.

Sanal ağ tümleştirmesinin iki biçimi vardır:

+ **Bölgesel sanal ağ tümleştirmesi (Önizleme)** : aynı bölgedeki sanal ağlarla tümleştirmeyi mümkün bir şekilde sunar. Bu tür bir tümleştirme, aynı bölgedeki bir sanal ağ için bir alt ağ gerektirir. Bu özellik hala önizlemededir, ancak aşağıda belirtilen uyarılar ile Windows üzerinde çalışan işlev uygulamaları için desteklenir.
+ **Ağ geçidi gereken sanal ağ tümleştirmesi**: uzak bölgelerdeki sanal ağlarla veya klasik sanal ağlarla tümleştirmeyi mümkün. Bu tür bir tümleştirme, sanal ağ geçidinin VNet 'iniz üzerinde dağıtılmasını gerektirir. Bu, yalnızca Windows üzerinde çalışan işlev uygulamaları için desteklenen Noktadan siteye VPN tabanlı bir özelliktir.

Bir uygulama tek seferde yalnızca bir VNet tümleştirme özelliğinin türünü kullanabilir. Her ikisi de birçok senaryo için yararlı olmakla birlikte, aşağıdaki tablo her birinin nerede kullanılması gerektiğini gösterir:

| Sorun  | Çözüm |
|----------|----------|
| Aynı bölgedeki bir RFC 1918 adresine (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) ulaşmak ister | bölgesel VNet tümleştirmesi |
| Klasik VNet veya başka bir bölgedeki VNet 'teki kaynaklara ulaşmak ister | Ağ Geçidi gerekli VNet tümleştirmesi |
| ExpressRoute üzerinde RFC 1918 uç noktalarına ulaşmak ister | bölgesel VNet tümleştirmesi |
| Hizmet uç noktaları genelinde kaynaklara ulaşmak ister misiniz? | bölgesel VNet tümleştirmesi |

Her iki özellik de ExpressRoute üzerinde RFC olmayan 1918 adreslerine ulaşmanıza imkan tanır. Bunu yapmak için, şimdilik bir AO kullanmanız gerekir.

Bölgesel VNet tümleştirmesinin kullanılması, VNet 'iniz şirket içi sunucuya bağlanamaz veya hizmet uç noktalarını yapılandırmaz. Bu, ayrı ağ yapılandırması. Bölgesel VNet tümleştirmesi, uygulamanızın bu bağlantı türleri arasında çağrı yapmasına olanak sağlar.

VNet tümleştirmesi, kullanılan sürümden bağımsız olarak, işlev uygulamanızın sanal ağınızdaki kaynaklara erişmesini sağlar, ancak sanal ağdan işlev uygulamanıza özel site erişimi vermez. Özel site erişimi, uygulamanızı yalnızca bir Azure sanal ağı içindeki gibi özel bir ağdan erişilebilir hale getirme anlamına gelir. VNet tümleştirmesi yalnızca uygulamanızdan VNet 'e giden çağrılar yapmak içindir. 

VNet tümleştirme özelliği:

* Standart, Premium veya PremiumV2 App Service planı gerektirir
* TCP ve UDP 'yi destekler
* App Service uygulamalarla ve Işlev uygulamalarıyla çalışır

VNet tümleştirmesinin şunları dahil desteklemediği bazı şeyler vardır:

* Sürücü takma
* AD tümleştirmesi
* NetBios

Işlevlerde sanal ağ tümleştirmesi App Service Web Apps ile paylaşılan altyapıyı kullanır. İki tür sanal ağ tümleştirmesi hakkında daha fazla bilgi edinmek için bkz.:

* [Bölgesel Sanal Ağ Tümleştirmesi](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [Ağ Geçidi gerekli VNet tümleştirmesi](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Sanal ağ tümleştirmesini kullanma hakkında daha fazla bilgi için bkz. [bir işlev uygulamasını bir Azure sanal ağı Ile tümleştirme](functions-create-vnet.md).

## <a name="connecting-to-service-endpoint-secured-resources"></a>Hizmet uç noktası güvenliği sağlanmış kaynaklara bağlanılıyor

> [!NOTE]
> Geçici olarak, aşağı akış kaynağında erişim kısıtlamalarını yapılandırdıktan sonra, yeni hizmet uç noktalarının işlev uygulamanız için kullanılabilir olması 12 saate kadar sürebilir. Bu süre boyunca, kaynak uygulamanız için tamamen kullanılamaz olur.

Daha yüksek bir güvenlik düzeyi sağlamak için, hizmet uç noktalarını kullanarak bir dizi Azure hizmetini bir sanal ağ ile kısıtlayabilirsiniz. Daha sonra, kaynak erişimi için işlev uygulamanızı bu sanal ağla tümleştirmeniz gerekir. Bu yapılandırma, sanal ağ tümleştirmesini destekleyen tüm planlarda desteklenir.

[Sanal ağ hizmeti uç noktaları hakkında buradan daha fazla bilgi edinin.](../virtual-network/virtual-network-service-endpoints-overview.md)

### <a name="restricting-your-storage-account-to-a-virtual-network"></a>Depolama hesabınızı bir sanal ağla kısıtlama

Bir işlev uygulaması oluştururken blob, kuyruk ve tablo depolamayı destekleyen genel amaçlı bir Azure depolama hesabı oluşturmanız veya bağlamanız gerekir. Şu anda bu hesapta herhangi bir sanal ağ kısıtlaması kullanılması mümkün değildir. İşlev uygulamanız için kullandığınız depolama hesabında bir sanal ağ hizmet uç noktası yapılandırırsanız, uygulamanız kesilir.

[Depolama hesabı gereksinimleri hakkında buradan daha fazla bilgi edinin.](./functions-create-function-app-portal.md#storage-account-requirements
)

## <a name="virtual-network-triggers-non-http"></a>Sanal ağ Tetikleyicileri (HTTP olmayan)

Şu anda, bir sanal ağ içinden HTTP dışında Işlev tetiklerinin kullanabilmesi için, işlev uygulamanızı bir App Service planında veya bir App Service Ortamı çalıştırmanız gerekir.

Örneğin, bir sanal ağdan gelen trafiği kabul etmek için Azure Cosmos DB yapılandırmak istiyorsanız, Azure Cosmos DB tetikleyicilerini yapılandırmak için bu sanal ağla sanal ağ tümleştirmesiyle birlikte işlev uygulamanızı dağıtmanız gerekir Bu kaynaktan. Önizleme aşamasında, VNET tümleştirmesini yapılandırmak Premium planın bu Azure Cosmos DB kaynakta tetiklemesine izin vermez.

Nelerin desteklendiğini iki kez kontrol etmek için [http olmayan tüm tetikleyiciler için bu listeyi](./functions-triggers-bindings.md#supported-bindings) işaretleyin.

## <a name="hybrid-connections"></a>Karma Bağlantılar

[Karma bağlantılar](../service-bus-relay/relay-hybrid-connections-protocol.md) , diğer ağlardaki uygulama kaynaklarına erişmek için kullanabileceğiniz bir Azure Relay özelliğidir. Uygulamadan bir uygulama uç noktasına erişim sağlar. Uygulamanıza erişmek için kullanamazsınız. Karma Bağlantılar, tüketim planı hariç tüm işlevler için kullanılabilir.

Azure Işlevlerinde kullanıldığında, her karma bağlantı tek bir TCP ana bilgisayarı ve bağlantı noktası bileşimiyle söz konusu. Bu, bir TCP dinleme bağlantı noktasına eriştiğiniz sürece karma bağlantının uç noktasının herhangi bir işletim sisteminde ve herhangi bir uygulamada olabileceği anlamına gelir. Karma Bağlantılar özelliği, uygulama protokolünün ne olduğunu veya ne erişeceğimizi bilmez veya ilgilenmez. Yalnızca ağ erişimi sağlar.

Daha fazla bilgi edinmek için, aynı yapılandırma adımları aracılığıyla Işlevleri destekleyen [karma bağlantılar için App Service belgelerine](../app-service/app-service-hybrid-connections.md)bakın.

## <a name="outbound-ip-restrictions"></a>Giden IP kısıtlamaları

Giden IP kısıtlamaları yalnızca bir App Service Ortamı dağıtılan işlevler için kullanılabilir. App Service Ortamı dağıtıldığı sanal ağın giden kısıtlamalarını yapılandırabilirsiniz.

Bir Işlev uygulamasını Premium planda tümleştirdiğinizde veya bir sanal ağla plan App Service, uygulama yine de internet 'e giden çağrılar yapabilir.

## <a name="next-steps"></a>Sonraki adımlar

Ağ ve Azure Işlevleri hakkında daha fazla bilgi edinmek için: 

* [Sanal ağ tümleştirmesiyle çalışmaya başlama öğreticisini izleyin](./functions-create-vnet.md)
* [Ağ iletişimi SSS Işlevlerini okuyun](./functions-networking-faq.md)
* [App Service/Işlevlerle sanal ağ tümleştirmesi hakkında daha fazla bilgi edinin](../app-service/web-sites-integrate-with-vnet.md)
* [Azure 'da sanal ağlar hakkında daha fazla bilgi edinin](../virtual-network/virtual-networks-overview.md)
* [App Service ortamlarla daha fazla ağ özelliği ve denetimi etkinleştirin](../app-service/environment/intro.md)
* [Karma Bağlantılar kullanarak, güvenlik duvarı değişikliği yapmadan tek tek şirket içi kaynaklara bağlanma](../app-service/app-service-hybrid-connections.md)
