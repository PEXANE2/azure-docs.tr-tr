---
title: Azure İşlevleri ağ seçenekleri
description: Azure İşlevler'de bulunan tüm ağ seçeneklerine genel bakış.
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: d8c3357325eadefec7bb97faba5d600e9c6793a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276718"
---
# <a name="azure-functions-networking-options"></a>Azure İşlevleri ağ seçenekleri

Bu makalede, Azure İşlevler için barındırma seçenekleri arasında kullanılabilir ağ özellikleri açıklanmaktadır. Aşağıdaki tüm ağ seçenekleri, internet-routable adreslerini kullanmadan kaynaklara erişmek için bazı yeteneği vermek, ya da bir işlev uygulaması na internet erişimini kısıtlamak için.

Barındırma modelleri, farklı ağ yalıtım düzeylerine sahiptir. Doğru olanı seçmek, ağ yalıtım gereksinimlerinizi karşılamanıza yardımcı olur.

İşlev uygulamalarını birkaç şekilde barındırabilirsiniz:

* Çok kiracılı bir altyapıda çalışan ve çeşitli sanal ağ bağlantısı ve ölçekleme seçenekleri içeren bir dizi plan seçeneği vardır:
    * Yüke yanıt olarak dinamik olarak ölçeklendirilen ve minimum ağ yalıtımı seçeneği sunan [Tüketim planı.](functions-scale.md#consumption-plan)
    * Daha kapsamlı ağ yalıtımı sunarken dinamik olarak ölçeklendirilen [Premium planı.](functions-scale.md#premium-plan)
    * Sabit ölçekte çalışan ve Premium plana benzer ağ yalıtımı sunan Azure [Uygulama Hizmeti planı.](functions-scale.md#app-service-plan)
* Bir Uygulama Hizmet [Ortamında](../app-service/environment/intro.md)işlevleri çalıştırabilirsiniz. Bu yöntem, işlevinizi sanal ağınıza dağır ve tam ağ denetimi ve yalıtımı sunar.

## <a name="matrix-of-networking-features"></a>Ağ özelliklerinin matrisi

|                |[Tüketim planı](functions-scale.md#consumption-plan)|[Premium planı](functions-scale.md#premium-plan)|[Uygulama Hizmeti planı](functions-scale.md#app-service-plan)|[App Service Ortamı](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Özel site erişimi & gelen IP kısıtlamaları](#inbound-ip-restrictions)|✅Evet|✅Evet|✅Evet|✅Evet|
|[Sanal ağ tümleştirmesi](#virtual-network-integration)|❌№|✅Evet (Bölgesel)|✅Evet (Bölgesel ve Ağ Geçidi)|✅Evet|
|[Sanal ağ tetikleyicileri (HTTP dışı)](#virtual-network-triggers-non-http)|❌№| ✅Evet |✅Evet|✅Evet|
|[Karma bağlantılar](#hybrid-connections) (yalnızca Windows)|❌№|✅Evet|✅Evet|✅Evet|
|[Giden IP kısıtlamaları](#outbound-ip-restrictions)|❌№| ✅Evet|✅Evet|✅Evet|

## <a name="inbound-ip-restrictions"></a>Gelen IP kısıtlamaları

UYGULAMANIZA erişime izin verilen veya reddedilen IP adreslerinin öncelikli olarak sıralanmış bir listesini tanımlamak için IP kısıtlamalarını kullanabilirsiniz. Liste IPv4 ve IPv6 adreslerini içerebilir. Bir veya daha fazla giriş olduğunda, listenin sonunda örtülü bir "tümini reddet" bulunur. IP kısıtlamaları tüm işlev barındırma seçenekleriyle çalışır.

> [!NOTE]
> Ağ kısıtlamaları yerindeyken, portal düzenleyicisini yalnızca sanal ağınızın içinden veya Güvenli Alıcılar listesindeki Azure portalına erişmek için kullandığınız makinenin IP adresini koyduğunuzda kullanabilirsiniz. Ancak, **platform özellikleri** sekmesindeki tüm özelliklere herhangi bir makineden erişebilirsiniz.

Daha fazla bilgi için [Azure Uygulama Hizmeti statik erişim kısıtlamalarına](../app-service/app-service-ip-restrictions.md)bakın.

## <a name="private-site-access"></a>Özel site erişimi

Özel site erişimi, uygulamanızı yalnızca Azure sanal ağı gibi özel bir ağdan erişilebilir hale getirmek anlamına gelir.

* Hizmet bitiş noktaları yapılandırıldığında [Premium,](./functions-premium-plan.md) [Tüketim](functions-scale.md#consumption-plan)ve [Uygulama Hizmeti](functions-scale.md#app-service-plan) planlarında özel site erişimi mevcuttur.
    * Hizmet bitiş **noktaları, Platform özellikleri** > altında uygulama başına bazda yapılandırılabilir**Ağ Yapılandırma** > **Erişim Kısıtlamaları** > **Ekle Kuralı.** Sanal ağlar artık kural türü olarak seçilebilir.
    * Daha fazla bilgi için [sanal ağ hizmeti bitiş noktalarına](../virtual-network/virtual-network-service-endpoints-overview.md)bakın.
    * Hizmet bitiş noktalarıyla, sanal ağ tümleştirmesi yapılandırıldığında bile işlevinizin internete tam çıkış erişimiolduğunu unutmayın.
* Özel site erişimi, dahili yük dengeleyicisi (ILB) ile yapılandırılan bir Uygulama Hizmet Ortamı içinde de kullanılabilir. Daha fazla bilgi için [bkz.](../app-service/environment/create-ilb-ase.md)

Özel site erişimini nasıl ayarlayatılabildiğini öğrenmek için Azure [İşlerini Oluştur özel site erişimine](functions-create-private-site-access.md)bakın.

## <a name="virtual-network-integration"></a>Sanal ağ tümleştirmesi

Sanal ağ tümleştirmesi, işlev uygulamanızın sanal ağ içindeki kaynaklara erişmesine olanak tanır. Azure İşlevler iki tür sanal ağ tümleştirmesini destekler:

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

Azure İşlevler'deki sanal ağ tümleştirmesi, App Service web uygulamalarıyla paylaşılan altyapıyı kullanır. İki sanal ağ tümleştirme türü hakkında daha fazla bilgi edinmek için bkz:

* [Bölgesel sanal ağ Entegrasyonu](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [Ağ geçidi gerekli sanal ağ entegrasyonu](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Sanal ağ tümleştirmesini nasıl ayarlayacağınızı öğrenmek için [bkz.](functions-create-vnet.md)

## <a name="regional-virtual-network-integration"></a>Bölgesel Sanal Ağ Entegrasyonu

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

## <a name="connecting-to-service-endpoint-secured-resources"></a>Hizmet bitiş noktası güvenli kaynaklara bağlanma

Daha yüksek bir güvenlik düzeyi sağlamak için, hizmet bitiş noktalarını kullanarak bir dizi Azure hizmetini sanal ağla sınırlandırabilirsiniz. Daha sonra kaynağa erişmek için işlev uygulamanızı bu sanal ağla tümleştirmeniz gerekir. Bu yapılandırma, sanal ağ tümleştirmedestekleyen tüm planlarda desteklenir.

[Sanal ağ hizmeti bitiş noktaları hakkında daha fazla bilgi edinin.](../virtual-network/virtual-network-service-endpoints-overview.md)

## <a name="restricting-your-storage-account-to-a-virtual-network"></a>Depolama hesabınızı sanal ağla sınırlama

Bir işlev uygulaması oluşturduğunuzda, Blob, Queue ve Tablo depolama alanını destekleyen genel amaçlı bir Azure Depolama hesabı oluşturmanız veya bağlantı kurmanız gerekir. Şu anda bu hesapta sanal ağ kısıtlaması kullanamazsınız. İşlev uygulamanız için kullandığınız depolama hesabında bir sanal ağ hizmeti bitiş noktası yapılandırırsanız, bu uygulamanızı bozar.

[Depolama hesabı gereksinimleri hakkında daha fazla bilgi edinin.](./functions-create-function-app-portal.md#storage-account-requirements)

## <a name="using-key-vault-references"></a>Key Vault referanslarını kullanma 

Anahtar Vault referansları, azure işlevleri uygulamanızda azure anahtar kasası ile ilgili sırları herhangi bir kod değişikliği gerektirmeden kullanmanıza olanak tanır. Azure Key Vault, erişim ilkeleri ve denetim geçmişi üzerinde tam denetime sahip merkezi sırlar yönetimi sağlayan bir hizmettir.

Key [Vault'unuzun](../app-service/app-service-key-vault-references.md) hizmet bitiş noktalarıyla güvenli olması durumunda Şu anda Key Vault başvuruları çalışmaz. Sanal ağ tümleştirmesini kullanarak Key Vault'a bağlanmak için uygulama kodunuzdaki anahtar kasasını aramanız gerekir.

## <a name="virtual-network-triggers-non-http"></a>Sanal ağ tetikleyicileri (HTTP dışı)

Şu anda, sanal ağ içinden http tetikleyici olmayan işlevleri iki şekilde kullanabilirsiniz: 
+ İşlev uygulamanızı premium planda çalıştırın ve Sanal ağ tetikleyici desteğini etkinleştirin.
+ İşlev uygulamanızı bir Uygulama Hizmeti planında veya Uygulama Hizmeti Ortamında çalıştırın.

### <a name="premium-plan-with-virtual-network-triggers"></a>Sanal ağ tetikleyicileri ile Premium Plan

Premium planda çalışırken, HTTP tetikleyici olmayan işlevleri sanal ağ içinde çalışan hizmetlere bağlayabilirsiniz. Bunu yapmak için, işlev uygulamanız için sanal ağ tetikleyici desteğini etkinleştirmeniz gerekir. **Sanal ağ tetikleyici destek** ayarı, Azure [portalında](https://portal.azure.com) **İşlev uygulaması ayarları**altında bulunur.

![VNETToggle](media/functions-networking-options/virtual-network-trigger-toggle.png)

Aşağıdaki Azure CLI komutunu kullanarak sanal ağ tetikleyicilerini de etkinleştirebilirsiniz:

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.functionsRuntimeScaleMonitoringEnabled=1 --resource-type Microsoft.Web/sites
```

Sanal ağ tetikleyicileri, Fonksiyonlar çalışma zamanının 2.x sürümünde ve üstünde desteklenir. Aşağıdaki HTTP olmayan tetikleyici türleri desteklenir.

| Dahili numara | En Düşük Sürüm |
|-----------|---------| 
|[Microsoft.Azure.Webİşler.Extensions.Depolama](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/) | 3.0.10 veya üzeri |
|[Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs)| 4.1.0 veya üzeri|
|[Microsoft.Azure.WebJobs.Extensions.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus)| 3.2.0 veya üzeri|
|[Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB)| 3.0.5 veya üzeri|
|[Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)| 2.0.0 veya üzeri|

> [!IMPORTANT]
> Sanal ağ tetikleyici desteğini etkinleştirirken, yalnızca yukarıdaki tetikleyici türleri uygulamanızla dinamik olarak ölçeklendirilir. Yukarıda listelenmemiş tetikleyicileri kullanmaya devam edebilirsiniz, ancak önceden ısıtılmış örnek sayısının ötesine ölçeklendirilmezler. Tetikleyicilerin tam listesi için [tetikleyicilere ve bağlamalara](./functions-triggers-bindings.md#supported-bindings) bakın.

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>Sanal ağ tetikleyicileri ile Uygulama Hizmet Planı ve Uygulama Hizmet Ortamı

İşlev uygulamanız bir Uygulama Hizmet Planı veya Uygulama Hizmeti Ortamında çalıştığında, HTTP tetikleyici olmayan işlevleri kullanabilirsiniz. İşlevlerinizin doğru şekilde tetiklenabilmesi için tetikleyici bağlantıda tanımlanan kaynağa erişimi olan bir sanal ağa bağlı olmalısınız. 

Örneğin, Azure Cosmos DB'yi yalnızca sanal ağdan gelen trafiği kabul etmek üzere yapılandırmak istediğinizi varsayalım. Bu durumda, işlev uygulamanızı, söz konusu sanal ağla sanal ağ tümleştirmesi sağlayan bir Uygulama Hizmeti planında dağıtmanız gerekir. Bu, bir işlevin bu Azure Cosmos DB kaynağı tarafından tetiklenmesini sağlar. 

## <a name="hybrid-connections"></a>Karma Bağlantılar

[Karma Bağlantılar,](../service-bus-relay/relay-hybrid-connections-protocol.md) Azure Röle'nin diğer ağlardaki uygulama kaynaklarına erişmek için kullanabileceğiniz bir özelliğidir. Uygulamanızdan uygulama bitiş noktasına erişim sağlar. Uygulamanıza erişmek için kullanamazsınız. Karma Bağlantılar, Tüketim planı hariç tüm Windows'ta çalışan işlevler için kullanılabilir.

Azure İşlevlerinde kullanıldığı gibi, her karma bağlantı tek bir TCP ana bilgisayar ve bağlantı noktası birleşimi ile ilişkilidir. Bu, karma bağlantının bitiş noktasının, bir TCP dinleme bağlantı noktasına erişirken herhangi bir işletim sisteminde ve herhangi bir uygulamada olabileceği anlamına gelir. Karma Bağlantılar özelliği, uygulama protokolünün ne olduğunu veya neye eriştediğinizi bilmiyor veya önemsemiyor. Sadece ağ erişimi sağlar.

Daha fazla bilgi edinmek [için Karma Bağlantılar için Uygulama Hizmeti belgelerine](../app-service/app-service-hybrid-connections.md)bakın. Aynı yapılandırma adımları Azure İşlevlerini destekler.

>[!IMPORTANT]
> Karma Bağlantılar yalnızca Windows planlarında desteklenir. Linux desteklenmiyor

## <a name="outbound-ip-restrictions"></a>Giden IP kısıtlamaları

Giden IP kısıtlamaları Premium planda, Uygulama Hizmeti planında veya Uygulama Hizmeti Ortamında kullanılabilir. Uygulama Hizmet Ortamınızın dağıtıldığı sanal ağ için giden kısıtlamaları yapılandırabilirsiniz.

Bir işlev uygulamasını Premium plana veya Uygulama Hizmeti planına sanal ağla entegre ettiğinizde, uygulama varsayılan olarak internete giden aramalar yapabilir. Bir uygulama ayarı `WEBSITE_VNET_ROUTE_ALL=1`ekleyerek, ağ güvenlik grubu kurallarının trafiği kısıtlamak için kullanılabildiği tüm giden trafiği sanal ağınıza gönderilmeye zorlarsınız.

## <a name="troubleshooting"></a>Sorun giderme 

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ ve Azure İşlevleri hakkında daha fazla bilgi edinmek için:

* [Sanal ağ entegrasyonu ile başlamak hakkında öğretici izleyin](./functions-create-vnet.md)
* [İşlevler ağ sss okuyun](./functions-networking-faq.md)
* [App Service/Functions ile sanal ağ entegrasyonu hakkında daha fazla bilgi edinin](../app-service/web-sites-integrate-with-vnet.md)
* [Azure'da sanal ağlar hakkında daha fazla bilgi edinin](../virtual-network/virtual-networks-overview.md)
* [App Service Ortamları ile daha fazla ağ özelliği ve denetim etkinleştirin](../app-service/environment/intro.md)
* [Karma Bağlantılar kullanarak güvenlik duvarı değişiklikleri olmadan şirket içi tek kaynaklara bağlanma](../app-service/app-service-hybrid-connections.md)
