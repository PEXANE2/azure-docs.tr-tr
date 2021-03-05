---
title: Azure İşlevleri ağ seçenekleri
description: Azure Işlevlerinde kullanılabilen tüm ağ seçeneklerine genel bakış.
author: cachai2
ms.topic: conceptual
ms.date: 1/21/2021
ms.author: cachai
ms.openlocfilehash: f826c947b1e47c1c996a8e9102492e85adafa326
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102215162"
---
# <a name="azure-functions-networking-options"></a>Azure İşlevleri ağ seçenekleri

Bu makalede, Azure Işlevleri için barındırma seçenekleri genelinde kullanılabilen ağ özellikleri açıklanmaktadır. Aşağıdaki tüm ağ seçenekleri, internet 'e yönlendirilebilir adresler kullanmadan veya bir işlev uygulamasına internet erişimini kısıtlamak için bazı bir özellik sunar.

Barındırma modellerinin farklı düzeylerde ağ yalıtımı vardır. Doğru olanı seçtiğinizde, ağ yalıtımı gereksinimlerinizi karşılamanıza yardımcı olur.

İşlev uygulamalarını birkaç yolla barındırabilirsiniz:

* Farklı düzeylerde sanal ağ bağlantısı ve ölçeklendirme seçenekleri sayesinde çok kiracılı bir altyapıda çalışan plan seçenekleri arasından seçim yapabilirsiniz:
    * [Tüketim planı](consumption-plan.md) , yükleme yanıt olarak dinamik olarak ölçeklendirilir ve en düşük ağ yalıtımı seçeneklerini sunar.
    * [Premium plan](functions-premium-plan.md) ayrıca dinamik olarak ölçeklendiriyor ve daha kapsamlı ağ yalıtımı sunmaktadır.
    * Azure [App Service planı](dedicated-plan.md) sabit bir ölçekte çalışır ve Premium plana benzer ağ yalıtımı sağlar.
* İşlevleri bir [App Service ortamı](../app-service/environment/intro.md)çalıştırabilirsiniz. Bu yöntem, işlevinizi sanal ağınıza dağıtır ve tam ağ denetimi ve yalıtımı sağlar.

## <a name="matrix-of-networking-features"></a>Ağ özellikleri matrisi

[!INCLUDE [functions-networking-features](../../includes/functions-networking-features.md)]

## <a name="inbound-access-restrictions"></a>Gelen erişim kısıtlamaları

Uygulamanıza erişim izni verilen veya reddedilen IP adreslerinin öncelik sırasına sahip bir listesini tanımlamak için erişim kısıtlamalarını kullanabilirsiniz. Liste, IPv4 ve IPv6 adreslerini veya [hizmet uç noktaları](#use-service-endpoints)kullanan belirli sanal ağ alt ağlarını içerebilir. Bir veya daha fazla giriş olduğunda, listenin sonunda örtülü bir "Tümünü Reddet" bulunur. IP kısıtlamaları tüm işlev barındırma seçenekleriyle çalışır.

Erişim kısıtlamaları [Premium](functions-premium-plan.md), [Tüketim](consumption-plan.md)ve [App Service](dedicated-plan.md)kullanılabilir.

> [!NOTE]
> Ağ kısıtlamalarına sahip olmak için, yalnızca sanal ağınızdan veya kullandığınız makinenin IP adresini, Güvenli Alıcılar listesindeki Azure portal erişmek için girdiğinizde dağıtabilirsiniz. Ancak, portalı kullanarak işlevi yine de yönetebilirsiniz.

Daha fazla bilgi için bkz. [Azure App Service statik erişim kısıtlamaları](../app-service/app-service-ip-restrictions.md).

### <a name="use-service-endpoints"></a>Hizmet uç noktası kullanma

Hizmet uç noktalarını kullanarak, seçili Azure sanal ağ alt ağlarına erişimi kısıtlayabilirsiniz. Belirli bir alt ağa erişimi kısıtlamak için, **sanal ağ** türüyle bir kısıtlama kuralı oluşturun. Daha sonra izin vermek veya erişimi reddetmek istediğiniz abonelik, sanal ağ ve alt ağı seçebilirsiniz. 

Hizmet uç noktaları, seçtiğiniz alt ağ için Microsoft. Web ile zaten etkin değilse, **eksik Microsoft. Web hizmeti uç noktalarını yoksay** onay kutusunu seçmediğiniz takdirde bunlar otomatik olarak etkinleştirilir. Uygulama üzerinde hizmet uç noktalarını etkinleştirmek isteyebileceğiniz, ancak alt ağda olmayan bir senaryo, temel olarak bunları alt ağda etkinleştirme izinlerine sahip olmanıza bağlı olarak değişir. 

Alt ağda hizmet uç noktalarını etkinleştirmek için başka bir kişiye ihtiyacınız varsa, **yok sayma Microsoft. Web hizmeti uç noktaları** onay kutusunu seçin. Uygulamanız, olasılığına ' deki hizmet uç noktaları için, alt ağda daha sonra etkinleştirilmesini sağlayacak şekilde yapılandırılır. 

![Sanal ağ türü seçili olan "IP kısıtlaması Ekle" bölmesinin ekran görüntüsü.](../app-service/media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

App Service Ortamı çalıştıran uygulamalara erişimi kısıtlamak için hizmet uç noktalarını kullanamazsınız. Uygulamanız bir App Service Ortamı olduğunda, IP erişim kuralları uygulayarak bu erişime erişimi denetleyebilirsiniz. 

Hizmet uç noktalarını ayarlamayı öğrenmek için bkz. [Azure işlevleri özel site erişimi oluşturma](functions-create-private-site-access.md).

## <a name="private-endpoint-connections"></a>Özel uç nokta bağlantıları

[!INCLUDE [functions-private-site-access](../../includes/functions-private-site-access.md)]

Depolama veya hizmet veri yolu gibi özel bir uç nokta bağlantısına sahip diğer hizmetleri çağırmak için, uygulamanızı [Özel uç noktalara giden çağrılar](#private-endpoints)yapmak üzere yapılandırmayı unutmayın.

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

Bir işlev uygulaması oluşturduğunuzda, blob, kuyruk ve tablo depolamayı destekleyen genel amaçlı bir Azure depolama hesabı oluşturmanız veya bağlamanız gerekir. Bu depolama hesabını hizmet uç noktaları veya özel uç nokta ile güvenli bir şekilde değiştirebilirsiniz. Bu özellik şu anda, sanal ağların yalnızca Premium SKU için kullanılabildiği esnek damgalar haricinde standart ve Premium içeren tüm sanal ağ desteklenen SKU 'lar için geçerlidir. Özel bir ağla sınırlı bir depolama hesabı ile bir işlev ayarlamak için:

1. Hizmet uç noktaları etkin olmayan bir depolama hesabıyla bir işlev oluşturun.
1. İşlevini sanal ağınıza bağlanacak şekilde yapılandırın.
1. Farklı bir depolama hesabı oluşturun veya yapılandırın.  Bu, hizmet uç noktalarıyla güvenli hale yaptığımız depolama hesabıdır ve işlevimizi bağlayacağız.
1. Güvenli depolama hesabında [bir dosya paylaşma oluşturun](../storage/files/storage-how-to-create-file-share.md#create-file-share) .
1. Depolama hesabı için hizmet uç noktalarını veya özel uç noktayı etkinleştirin.  
    * Özel uç nokta bağlantıları kullanıyorsanız, depolama hesabının `file` ve alt kaynaklar için özel bir uç noktası olması gerekir `blob` .  Dayanıklı İşlevler gibi belirli yetenekler kullanılıyorsa, Ayrıca, `queue` `table` özel bir uç nokta bağlantısı aracılığıyla da ihtiyacınız ve erişilebilir olur.
    * Hizmet uç noktaları kullanılıyorsa, depolama hesapları için işlev uygulamalarınıza adanmış alt ağı etkinleştirin.
1. Dosya ve BLOB içeriğini işlev uygulama depolama hesabından güvenli depolama hesabına ve dosya paylaşımıyla kopyalayın.
1. Bu depolama hesabı için bağlantı dizesini kopyalayın.
1. İşlev uygulaması **yapılandırması** altındaki **uygulama ayarlarını** aşağıdakilere göre güncelleştirin:
    - `AzureWebJobsStorage` güvenli depolama hesabı için bağlantı dizesine.
    - `WEBSITE_CONTENTAZUREFILECONNECTIONSTRING` güvenli depolama hesabı için bağlantı dizesine.
    - `WEBSITE_CONTENTSHARE` güvenli depolama hesabında oluşturulan dosya paylaşımının adı.
    - Adı ve değeri olan yeni bir ayar oluşturun `WEBSITE_CONTENTOVERVNET` `1` .
    - Depolama hesabı özel uç nokta bağlantıları kullanıyorsa, aşağıdaki ayarları doğrulayın veya ekleyin
        - `WEBSITE_VNET_ROUTE_ALL` değeri ile `1` .
        - `WEBSITE_DNS_SERVER` değeri ile `168.63.129.16` 
1. Uygulama ayarlarını kaydedin.  

İşlev uygulaması yeniden başlatılır ve artık güvenli bir depolama hesabına bağlanacak.

## <a name="use-key-vault-references"></a>Key Vault başvurularını kullanma

Kod değişikliği gerektirmeden Azure Işlevleri uygulamanızda Azure Key Vault gizli dizileri kullanmak için Azure Key Vault başvurularını kullanabilirsiniz. Azure Key Vault, erişim ilkeleri ve denetim geçmişi üzerinde tam kontrol sahibi olarak merkezi gizli dizi yönetimi sağlar.

Şu anda, anahtar kasanızın hizmet uç noktalarıyla güvenliği varsa [Key Vault başvuruları](../app-service/app-service-key-vault-references.md) çalışmaz. Sanal ağ tümleştirmesini kullanarak bir anahtar kasasına bağlanmak için, uygulama kodunuzda Key Vault çağırmanız gerekir.

## <a name="virtual-network-triggers-non-http"></a>Sanal ağ Tetikleyicileri (HTTP olmayan)

Şu anda, bir sanal ağ içinden HTTP olmayan tetikleyici işlevlerini iki şekilde kullanabilirsiniz:

+ İşlev uygulamanızı Premium bir planda çalıştırın ve sanal ağ tetikleme desteğini etkinleştirin.
+ İşlev uygulamanızı bir App Service planında veya App Service Ortamı çalıştırın.

### <a name="premium-plan-with-virtual-network-triggers"></a>Sanal ağ tetikleyicilerine sahip Premium plan

Bir Premium planı çalıştırdığınızda, HTTP olmayan tetikleyici işlevlerini bir sanal ağ içinde çalışan hizmetlere bağlayabilirsiniz. Bunu yapmak için, işlev uygulamanız için sanal ağ tetikleme desteğini etkinleştirmeniz gerekir. **Çalışma zamanı ölçek izleme** ayarı, [Azure Portal](https://portal.azure.com) **yapılandırma**  >  **işlevi çalışma zamanı ayarları** altında bulunur.

:::image type="content" source="media/functions-networking-options/virtual-network-trigger-toggle.png" alt-text="VNETToggle":::

Ayrıca, aşağıdaki Azure CLı komutunu kullanarak sanal ağ tetikleyicilerini etkinleştirebilirsiniz:

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.functionsRuntimeScaleMonitoringEnabled=1 --resource-type Microsoft.Web/sites
```

> [!TIP]
> App Service plan örneklerinizin ne zaman ölçekleneceğini öğrenmek için tetikleyicilerlerinizi izlemesi gerekeceğinden, sanal ağ tetikleyicilerinin etkinleştirilmesi uygulamanızın performansını etkileyebilir. Bu etkinin çok küçük olması olasıdır.

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

[Karma bağlantılar](../azure-relay/relay-hybrid-connections-protocol.md) , diğer ağlardaki uygulama kaynaklarına erişmek için kullanabileceğiniz bir Azure Relay özelliğidir. Uygulamadan bir uygulama uç noktasına erişim sağlar. Uygulamanıza erişmek için kullanamazsınız. Karma Bağlantılar, tüketim planı hariç Windows üzerinde çalışan işlevler için kullanılabilir.

Azure Işlevlerinde kullanıldığında, her karma bağlantı tek bir TCP ana bilgisayarı ve bağlantı noktası bileşimiyle söz konusu. Bu, karma bağlantının uç noktasının, TCP dinleme bağlantı noktasına eriştiğiniz sürece herhangi bir işletim sisteminde ve herhangi bir uygulamada olabileceği anlamına gelir. Karma Bağlantılar özelliği, uygulama protokolünün ne olduğunu veya ne erişmekte olduğunu bilmez veya ilgilenmez. Yalnızca ağ erişimi sağlar.

Daha fazla bilgi edinmek için [Karma Bağlantılar App Service belgelerine](../app-service/app-service-hybrid-connections.md)bakın. Aynı yapılandırma adımları Azure Işlevlerini destekler.

>[!IMPORTANT]
> Karma Bağlantılar yalnızca Windows planlarında desteklenir. Linux desteklenmez.

## <a name="outbound-ip-restrictions"></a>Giden IP kısıtlamaları

Giden IP kısıtlamaları bir Premium planda, App Service planında veya App Service Ortamı kullanılabilir. App Service Ortamı dağıtıldığı sanal ağın giden kısıtlamalarını yapılandırabilirsiniz.

Bir Premium planda veya bir sanal ağla App Service bir planda bir işlev uygulamasını tümleştirdiğinizde, uygulama varsayılan olarak internet 'e giden çağrılar yapmaya devam edebilir. Uygulama ayarını ekleyerek `WEBSITE_VNET_ROUTE_ALL=1` , trafiği kısıtlamak için ağ güvenlik grubu kurallarının kullanılabileceği sanal ağınıza giden tüm trafiği gönderilmesini zorlarsınız.

## <a name="automation"></a>Otomasyon
Aşağıdaki API 'Ler, bölgesel sanal ağ tümleştirmelerini programlı bir şekilde yönetmenizi sağlar:

+ **Azure CLI**: [`az functionapp vnet-integration`](/cli/azure/functionapp/vnet-integration) Bölgesel bir sanal ağ tümleştirmesi eklemek, listelemek veya kaldırmak için komutları kullanın.  
+ **ARM şablonları**: bölgesel sanal ağ tümleştirmesi, bir Azure Resource Manager şablonu kullanılarak etkinleştirilebilir. Tam bir örnek için, [Bu işlevlere hızlı başlangıç şablonu](https://azure.microsoft.com/resources/templates/101-function-premium-vnet-integration/)' na bakın.

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
