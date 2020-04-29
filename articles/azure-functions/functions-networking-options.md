---
title: Azure İşlevleri ağ seçenekleri
description: Azure Işlevlerinde kullanılabilen tüm ağ seçeneklerine genel bakış.
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: 6637627d48df8f9b6126debc215aac9bceb76f6b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80419573"
---
# <a name="azure-functions-networking-options"></a>Azure İşlevleri ağ seçenekleri

Bu makalede, Azure Işlevleri için barındırma seçenekleri genelinde kullanılabilen ağ özellikleri açıklanmaktadır. Aşağıdaki tüm ağ seçenekleri, internet 'e yönlendirilebilir adresler kullanmadan veya bir işlev uygulamasına internet erişimini kısıtlamak için bazı bir özellik sunar.

Barındırma modellerinin farklı düzeylerde ağ yalıtımı vardır. Doğru olanı seçtiğinizde, ağ yalıtımı gereksinimlerinizi karşılamanıza yardımcı olur.

İşlev uygulamalarını birkaç yolla barındırabilirsiniz:

* Farklı düzeylerde sanal ağ bağlantısı ve ölçeklendirme seçenekleri sayesinde çok kiracılı bir altyapıda çalışan plan seçenekleri arasından seçim yapabilirsiniz:
    * [Tüketim planı](functions-scale.md#consumption-plan) , yükleme yanıt olarak dinamik olarak ölçeklendirilir ve en düşük ağ yalıtımı seçeneklerini sunar.
    * [Premium plan](functions-scale.md#premium-plan) ayrıca dinamik olarak ölçeklendiriyor ve daha kapsamlı ağ yalıtımı sunmaktadır.
    * Azure [App Service planı](functions-scale.md#app-service-plan) sabit bir ölçekte çalışır ve Premium plana benzer ağ yalıtımı sağlar.
* İşlevleri bir [App Service ortamı](../app-service/environment/intro.md)çalıştırabilirsiniz. Bu yöntem, işlevinizi sanal ağınıza dağıtır ve tam ağ denetimi ve yalıtımı sağlar.

## <a name="matrix-of-networking-features"></a>Ağ özellikleri matrisi

|                |[Tüketim planı](functions-scale.md#consumption-plan)|[Premium planı](functions-scale.md#premium-plan)|[App Service planı](functions-scale.md#app-service-plan)|[App Service Ortamı](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[Gelen IP kısıtlamaları ve özel site erişimi](#inbound-ip-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|
|[Sanal Ağ tümleştirmesi](#virtual-network-integration)|❌Hayır|✅Evet (bölgesel)|✅Evet (bölgesel ve ağ geçidi)|✅Yes|
|[Sanal ağ Tetikleyicileri (HTTP olmayan)](#virtual-network-triggers-non-http)|❌Hayır| ✅Yes |✅Yes|✅Yes|
|[Karma bağlantılar](#hybrid-connections) (yalnızca Windows)|❌Hayır|✅Yes|✅Yes|✅Yes|
|[Giden IP kısıtlamaları](#outbound-ip-restrictions)|❌Hayır| ✅Yes|✅Yes|✅Yes|

## <a name="inbound-ip-restrictions"></a>Gelen IP kısıtlamaları

Uygulamanıza erişim izni verilen veya reddedilen IP adreslerinin öncelik sırasına sahip bir listesini tanımlamak için IP kısıtlamalarını kullanabilirsiniz. Liste, IPv4 ve IPv6 adreslerini içerebilir. Bir veya daha fazla giriş olduğunda, listenin sonunda örtülü bir "Tümünü Reddet" bulunur. IP kısıtlamaları tüm işlev barındırma seçenekleriyle çalışır.

> [!NOTE]
> Ağ kısıtlamalarına sahip olmak üzere, Portal düzenleyicisini yalnızca sanal ağınızdan veya Güvenli Alıcılar listesindeki Azure portal erişmek için kullandığınız makinenin IP adresini girdiğinizde kullanabilirsiniz. Ancak, herhangi bir makineden **platform özellikleri** sekmesindeki herhangi bir özelliğe erişmeye devam edebilirsiniz.

Daha fazla bilgi için bkz. [Azure App Service statik erişim kısıtlamaları](../app-service/app-service-ip-restrictions.md).

## <a name="private-site-access"></a>Özel site erişimi

Özel site erişimi, uygulamanızı yalnızca bir Azure sanal ağı gibi özel bir ağdan erişilebilir hale getirme anlamına gelir.

* Özel site erişimi, hizmet uç noktaları yapılandırıldığında [Premium](./functions-premium-plan.md), [Tüketim](functions-scale.md#consumption-plan)ve [App Service](functions-scale.md#app-service-plan) planlarında kullanılabilir.
    * Hizmet uç noktaları, **platform özellikleri** > **ağ oluşturma** > **erişim kısıtlamalarını** > yapılandırma**Kural Ekle**' nin altında uygulama temelinde yapılandırılabilir. Sanal ağlar artık bir kural türü olarak seçilebilir.
    * Daha fazla bilgi için bkz. [sanal ağ hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md).
    * Hizmet uç noktalarında, işlevinizin hala internet 'e yönelik sanal ağ tümleştirmesiyle birlikte tam giden erişimi olduğunu aklınızda bulundurun.
* Özel site erişimi, iç yük dengeleyici (ıLB) ile yapılandırılmış bir App Service Ortamı içinde de kullanılabilir. Daha fazla bilgi için bkz. [bir App Service ortamı iç yük dengeleyici oluşturma ve kullanma](../app-service/environment/create-ilb-ase.md).

Özel site erişimini ayarlamayı öğrenmek için bkz. [Azure işlevleri özel site erişimi oluşturma](functions-create-private-site-access.md).

## <a name="virtual-network-integration"></a>Sanal ağ tümleştirmesi

Sanal Ağ tümleştirmesi, işlev uygulamanızın bir sanal ağ içindeki kaynaklara erişmesine olanak sağlar.
Azure Işlevleri iki tür sanal ağ tümleştirmesini destekler:

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

Azure Işlevlerinde sanal ağ tümleştirmesi App Service Web Apps ile paylaşılan altyapıyı kullanır. İki tür sanal ağ tümleştirmesi hakkında daha fazla bilgi edinmek için bkz.:

* [Bölgesel sanal ağ tümleştirmesi](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [Ağ Geçidi-gerekli sanal ağ tümleştirmesi](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Sanal ağ tümleştirmesini ayarlamayı öğrenmek için bkz. [bir işlev uygulamasını bir Azure sanal ağı Ile tümleştirme](functions-create-vnet.md).

## <a name="regional-virtual-network-integration"></a>Bölgesel sanal ağ tümleştirmesi

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-regional.md)]

## <a name="connect-to-service-endpoint-secured-resources"></a>Hizmet uç noktası güvenliği sağlanmış kaynaklara bağlanma

Daha yüksek bir güvenlik düzeyi sağlamak için, hizmet uç noktalarını kullanarak bir dizi Azure hizmetini bir sanal ağ ile kısıtlayabilirsiniz. Daha sonra, kaynak erişimi için işlev uygulamanızı bu sanal ağla tümleştirmeniz gerekir. Bu yapılandırma, sanal ağ tümleştirmesini destekleyen tüm planlarda desteklenir.

Daha fazla bilgi için bkz. [sanal ağ hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md).

## <a name="restrict-your-storage-account-to-a-virtual-network"></a>Depolama hesabınızı bir sanal ağla sınırlayın

Bir işlev uygulaması oluşturduğunuzda, blob, kuyruk ve tablo depolamayı destekleyen genel amaçlı bir Azure depolama hesabı oluşturmanız veya bağlamanız gerekir. Şu anda bu hesapta herhangi bir sanal ağ kısıtlaması kullanamazsınız. İşlev uygulamanız için kullanmakta olduğunuz depolama hesabında bir sanal ağ hizmeti uç noktası yapılandırırsanız, bu yapılandırma uygulamanızı bozacaktır.

Daha fazla bilgi için bkz. [depolama hesabı gereksinimleri](./functions-create-function-app-portal.md#storage-account-requirements).

## <a name="use-key-vault-references"></a>Key Vault başvurularını kullan

Kod değişikliği gerektirmeden Azure Işlevleri uygulamanızda Azure Key Vault gizli dizileri kullanmak için Azure Key Vault başvurularını kullanabilirsiniz. Azure Key Vault, erişim ilkeleri ve denetim geçmişi üzerinde tam denetim ile merkezi gizli dizi yönetimi sağlayan bir hizmettir.

Şu anda, anahtar kasanızın hizmet uç noktalarıyla güvenliği varsa [Key Vault başvuruları](../app-service/app-service-key-vault-references.md) çalışmaz. Sanal ağ tümleştirmesini kullanarak bir anahtar kasasına bağlanmak için, uygulama kodunuzda Key Vault çağırmanız gerekir.

## <a name="virtual-network-triggers-non-http"></a>Sanal ağ Tetikleyicileri (HTTP olmayan)

Şu anda, bir sanal ağ içinden HTTP olmayan tetikleyici işlevlerini iki şekilde kullanabilirsiniz:

+ İşlev uygulamanızı Premium bir planda çalıştırın ve sanal ağ tetikleme desteğini etkinleştirin.
+ İşlev uygulamanızı bir App Service planında veya App Service Ortamı çalıştırın.

### <a name="premium-plan-with-virtual-network-triggers"></a>Sanal ağ tetikleyicilerine sahip Premium plan

Bir Premium planı çalıştırdığınızda, HTTP olmayan tetikleyici işlevlerini bir sanal ağ içinde çalışan hizmetlere bağlayabilirsiniz. Bunu yapmak için, işlev uygulamanız için sanal ağ tetikleme desteğini etkinleştirmeniz gerekir. **Sanal ağ tetikleme desteği** ayarı, **işlev uygulaması ayarları**altındaki [Azure Portal](https://portal.azure.com) bulunur.

![Sanal ağ geçişi](media/functions-networking-options/virtual-network-trigger-toggle.png)

Ayrıca, aşağıdaki Azure CLı komutunu kullanarak sanal ağ tetikleyicilerini etkinleştirebilirsiniz:

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.functionsRuntimeScaleMonitoringEnabled=1 --resource-type Microsoft.Web/sites
```

Sanal ağ Tetikleyicileri, Işlevler çalışma zamanının 2. x ve üzerinde desteklenir. Aşağıdaki HTTP olmayan tetikleyici türleri desteklenir.

| Dahili numara | En düşük sürüm |
|-----------|---------| 
|[Microsoft. Azure. WebJobs. Extensions. Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/) | 3.0.10 veya üzeri |
|[Microsoft. Azure. WebJobs. Extensions. EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs)| 4.1.0 veya üzeri|
|[Microsoft. Azure. WebJobs. Extensions. ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus)| 3.2.0 veya üzeri|
|[Microsoft. Azure. WebJobs. Extensions. CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB)| 3.0.5 veya üzeri|
|[Microsoft. Azure. WebJobs. Extensions. DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)| 2.0.0 veya üzeri|

> [!IMPORTANT]
> Sanal ağ tetikleme desteğini etkinleştirdiğinizde, yalnızca önceki tabloda gösterilen tetikleyici türleri uygulamanızla dinamik olarak ölçeklendirilir. Tabloda olmayan Tetikleyicileri kullanmaya devam edebilirsiniz, ancak bunlar önceden çarpımış örnek sayısının ötesinde ölçeklendirilmez. Tetikleyicilerin tüm listesi için bkz. [Tetikleyiciler ve bağlamalar](./functions-triggers-bindings.md#supported-bindings).

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>Sanal ağ tetikleyicilerle plan ve App Service Ortamı App Service

İşlev uygulamanız App Service planınızdan veya bir App Service Ortamı çalıştırıldığında HTTP olmayan tetikleyici işlevleri kullanabilirsiniz. İşlevlerinizin doğru tetiklenmesi için, tetikleyici bağlantısında tanımlanan kaynağa erişimi olan bir sanal ağa bağlı olmanız gerekir.

Örneğin, yalnızca bir sanal ağdan gelen trafiği kabul etmek için Azure Cosmos DB yapılandırmak istediğinizi varsayalım. Bu durumda, işlev uygulamanızı sanal ağ ile sanal ağ tümleştirmesi sağlayan bir App Service planına dağıtmanız gerekir. Tümleştirme, bir işlevin bu Azure Cosmos DB kaynak tarafından tetiklenmesi için izin sağlar.

## <a name="hybrid-connections"></a>Karma Bağlantılar

[Karma bağlantılar](../service-bus-relay/relay-hybrid-connections-protocol.md) , diğer ağlardaki uygulama kaynaklarına erişmek için kullanabileceğiniz bir Azure Relay özelliğidir. Uygulamadan bir uygulama uç noktasına erişim sağlar. Uygulamanıza erişmek için kullanamazsınız. Karma Bağlantılar, tüketim planı hariç Windows üzerinde çalışan işlevler için kullanılabilir.

Azure Işlevlerinde kullanıldığında, her karma bağlantı tek bir TCP ana bilgisayarı ve bağlantı noktası bileşimiyle söz konusu. Bu, karma bağlantının uç noktasının, TCP dinleme bağlantı noktasına eriştiğiniz sürece herhangi bir işletim sisteminde ve herhangi bir uygulamada olabileceği anlamına gelir. Karma Bağlantılar özelliği, uygulama protokolünün ne olduğunu veya ne erişmekte olduğunu bilmez veya ilgilenmez. Yalnızca ağ erişimi sağlar.

Daha fazla bilgi edinmek için [Karma Bağlantılar App Service belgelerine](../app-service/app-service-hybrid-connections.md)bakın. Aynı yapılandırma adımları Azure Işlevlerini destekler.

>[!IMPORTANT]
> Karma Bağlantılar yalnızca Windows planlarında desteklenir. Linux desteklenmez.

## <a name="outbound-ip-restrictions"></a>Giden IP kısıtlamaları

Giden IP kısıtlamaları bir Premium planda, App Service planında veya App Service Ortamı kullanılabilir. App Service Ortamı dağıtıldığı sanal ağın giden kısıtlamalarını yapılandırabilirsiniz.

Bir Premium planda veya bir sanal ağla App Service bir planda bir işlev uygulamasını tümleştirdiğinizde, uygulama varsayılan olarak internet 'e giden çağrılar yapmaya devam edebilir. Uygulama ayarını `WEBSITE_VNET_ROUTE_ALL=1`ekleyerek, trafiği kısıtlamak için ağ güvenlik grubu kurallarının kullanılabileceği sanal ağınıza giden tüm trafiği gönderilmesini zorlarsınız.

## <a name="troubleshooting"></a>Sorun giderme

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ağ ve Azure Işlevleri hakkında daha fazla bilgi edinmek için:

* [Sanal ağ tümleştirmesiyle çalışmaya başlama öğreticisini izleyin](./functions-create-vnet.md)
* [Ağ iletişimi SSS Işlevlerini okuyun](./functions-networking-faq.md)
* [App Service/Işlevlerle sanal ağ tümleştirmesi hakkında daha fazla bilgi edinin](../app-service/web-sites-integrate-with-vnet.md)
* [Azure 'da sanal ağlar hakkında daha fazla bilgi edinin](../virtual-network/virtual-networks-overview.md)
* [App Service ortamlarla daha fazla ağ özelliği ve denetimi etkinleştirin](../app-service/environment/intro.md)
* [Karma Bağlantılar kullanarak, güvenlik duvarı değişikliği yapmadan tek tek şirket içi kaynaklara bağlanma](../app-service/app-service-hybrid-connections.md)
