---
title: Azure Service Fabric uygulama tasarımı en iyi uygulamaları
description: Azure Service Fabric kullanarak uygulama ve hizmet geliştirmeye yönelik en iyi yöntemler ve tasarım konuları.
author: markfussell
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: mfussell
ms.openlocfilehash: 742cd9b1e7480fcc510b61d8987e42b499a1ff20
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86261198"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Azure Service Fabric uygulama tasarımı en iyi uygulamaları

Bu makalede, Azure Service Fabric üzerinde uygulama ve hizmet oluşturmak için en iyi Yöntem Kılavuzu sunulmaktadır.
 
## <a name="get-familiar-with-service-fabric"></a>Service Fabric hakkında bilgi edinin
* [Service Fabric hakkında bilgi edinmek için](service-fabric-content-roadmap.md) bu makaleyi okuyun.
* [Service Fabric uygulama senaryoları](service-fabric-application-scenarios.md)hakkında bilgi edinin.
* [Service Fabric programlama modeline genel bakış ' ı](service-fabric-choose-framework.md)okuyarak programlama modeli seçimlerini anlayın.



## <a name="application-design-guidance"></a>Uygulama tasarım kılavuzu
Service Fabric uygulamalarının [genel mimarisi](/azure/architecture/reference-architectures/microservices/service-fabric) ve [tasarım konuları](/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations)hakkında bilgi sahibi olun.

### <a name="choose-an-api-gateway"></a>Bir API ağ geçidi seçin
Daha sonra ölçeklenebilen arka uç hizmetleriyle iletişim kuran bir API ağ geçidi hizmeti kullanın. Kullanılan en yaygın API ağ geçidi hizmetleri şunlardır:

- [Service Fabric ile tümleştirilmiş](./service-fabric-tutorial-deploy-api-management.md) [Azure API Management](./service-fabric-api-management-overview.md).
- Olay Hub 'ı bölümlerinden okumak için [Servicefabricprocessor](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Microsoft.Azure.EventHubs.ServiceFabricProcessor) kullanarak [Azure IoT Hub](../iot-hub/index.yml) veya [Azure Event Hubs](../event-hubs/index.yml).
- [Azure Service Fabric sağlayıcısını](https://docs.traefik.io/v1.6/configuration/backends/servicefabric/)kullanarak [ters proxy Træfik](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric).
- [Azure Application Gateway](../application-gateway/index.yml).

   > [!NOTE] 
   > Azure Application Gateway doğrudan Service Fabric tümleştirilmiştir. Azure API Management genellikle tercih edilen seçenektir.
- Kendi özel oluşturulmuş [ASP.NET Core](./service-fabric-reliable-services-communication-aspnetcore.md) Web uygulaması ağ geçidiniz.

### <a name="stateless-services"></a>Durum bilgisi olmayan hizmetler
[Reliable Services](./service-fabric-reliable-services-introduction.md) kullanarak ve bir Azure veritabanı, Azure Cosmos DB veya Azure depolama alanında durum depolayarak durum bilgisi olmayan hizmetler oluşturarak her zaman başlayabilmeniz önerilir. Externalized durumu çoğu geliştirici için daha tanıdık bir yaklaşımdır. Bu yaklaşım ayrıca depodaki sorgu özelliğinden yararlanmanızı sağlar.  

### <a name="when-to-use-stateful-services"></a>Durum bilgisi olan hizmetler ne zaman kullanılır?
Düşük gecikme süresi için bir senaryonuz varsa ve verilerin işlem için kapatılmasını sağlamak için durum bilgisi olan hizmetleri göz önünde bulundurun. Bazı örnek senaryolar IoT Digital ikizi cihazlarını, oyun durumunu, oturum durumunu, bir veritabanından önbelleğe alma verilerini ve diğer hizmetlere yapılan çağrıları izlemek için uzun süreli iş akışlarını içerir.

Veri saklama zaman dilimine karar verin:

- **Önbelleğe alınmış veriler**. Dış depolara gecikme bir sorun olduğunda önbelleğe alma özelliğini kullanın. Kendi veri önbelleğiniz olarak durum bilgisi olan bir hizmet kullanın veya [Açık kaynaklı SoCreate Service Fabric dağıtılmış önbelleğini](https://github.com/SoCreate/service-fabric-distributed-cache)kullanmayı deneyin. Bu senaryoda, önbellekteki tüm verileri kaybederseniz endişelenmeniz gerekmez.
- **Zamana sınırlı veriler**. Bu senaryoda, gecikme süresi boyunca verileri daha sonra işlem yapmak için yakın tutmanız gerekir, ancak verileri bir *olağanüstü*durumda kaybetmemek için kullanabilirsiniz. Örneğin, birçok IoT çözümlerinde, son birkaç günün ortalama sıcaklığının hesaplanmakta olduğu, ancak bu veriler kaybediliyorsa, kaydedilen belirli veri noktaları önemli değildir. Ayrıca, bu senaryoda, genellikle bireysel veri noktalarını yedeklemeyi önemli bir şekilde dikkate almanız gerekmez. Yalnızca, düzenli aralıklarla dış depolamaya yazılan hesaplanmış ortalama değerleri yedeklemeniz gerekir.  
- **Uzun süreli veriler**. Güvenilir koleksiyonlar, verilerinizi kalıcı olarak saklayabilir. Ancak bu durumda, kümeleriniz için [düzenli yedekleme ilkeleri yapılandırma](./service-fabric-backuprestoreservice-configure-periodic-backup.md) dahil olmak üzere [olağanüstü durum kurtarma için hazırlanmanız](./service-fabric-disaster-recovery.md)gerekir. Aslında, kümeniz bir olağanüstü durumda yok edildiğinde, yeni bir küme oluşturmanız ve yeni uygulama örneklerinin dağıtılması ve en son yedeklemeden kurtarılması için ne olacağını yapılandırırsınız.

Maliyetleri kaydedin ve kullanılabilirliği geliştirebilirsiniz:
- Uzak depodan veri erişimi ve işlem maliyetlerinden sorumlu olmadığınızdan ve redin için Azure önbelleği gibi başka bir hizmet kullanmanız gerekmediği için, durum bilgisi olan hizmetleri kullanarak maliyetleri azaltabilirsiniz.
- Genellikle depolama için durum bilgisi olan hizmetlerin kullanılması, işlem için değildir ve bunu önermiyoruz. Durum bilgisi olan Hizmetleri, ucuz yerel depolama ile işlem olarak düşünün.
- Diğer hizmetlerde bağımlılıkları kaldırarak hizmet kullanılabilirliğini geliştirebilirsiniz. Kümede HA ile durum yönetimi, diğer hizmet saatleri veya gecikme sorunları olduğunu yalıtır.

## <a name="how-to-work-with-reliable-services"></a>Reliable Services ile çalışma
Service Fabric Reliable Services, durum bilgisiz ve durum bilgisi olan hizmetleri kolayca oluşturmanızı sağlar. Daha fazla bilgi için [Reliable Services giriş](./service-fabric-reliable-services-introduction.md)bölümüne bakın.
- Durum bilgisiz ve durum bilgisi olan hizmetlere yönelik yöntemde [iptal belirtecini](./service-fabric-reliable-services-lifecycle.md#stateful-service-primary-swaps) her zaman kabul edin `RunAsync()` ve `ChangeRole()` durum bilgisi olan hizmetler için yöntemi. Bunu yapmazsanız, hizmetinizin kapatılıp kapatılmadığı Service Fabric bilmez. Örneğin, iptal belirtecini dikkate mazsanız, daha uzun bir uygulama yükseltme süresi oluşabilir.
-    [İletişim dinleyicilerini](./service-fabric-reliable-services-communication.md) zamanında açın ve kapatın ve iptal belirteçlerini kabul edin.
-    Eşitleme kodunu zaman uyumsuz kodla karıştırmayın. Örneğin, `.GetAwaiter().GetResult()` zaman uyumsuz aramalarınız içinde kullanmayın. Çağrı yığınında zaman uyumsuz olarak *tüm yolu* kullanın.

## <a name="how-to-work-with-reliable-actors"></a>Reliable Actors ile çalışma
Service Fabric Reliable Actors, durum bilgisi olan sanal aktörlerin kolayca oluşturulmasını sağlar. Daha fazla bilgi için [Reliable Actors giriş](./service-fabric-reliable-actors-introduction.md)bölümüne bakın.

- Uygulamanızı ölçeklendirmeye yönelik aktörler arasında yayın/alt ileti kullanımını önemli ölçüde düşünün. Bu hizmeti sağlayan araçlar, [Açık kaynaklı SoCreate Service Fabric pub/Sub](https://service-fabric-pub-sub.socreate.it/) ve [Azure Service Bus](/azure/service-bus/)içerir.
- Aktör durumunu [mümkün olduğunca ayrıntılı](./service-fabric-reliable-actors-state-management.md#best-practices)hale getirin.
- [Aktörün yaşam döngüsünü](./service-fabric-reliable-actors-state-management.md#best-practices)yönetin. Onları tekrar kullanacaksanız aktörleri silin. Gereksiz aktörlerin silinmesi özellikle [geçici durum sağlayıcısını](./service-fabric-reliable-actors-state-management.md#state-persistence-and-replication)kullandığınızda önemlidir, çünkü tüm durum bellekte depolanır.
- Doğru [tabanlı eşzamanlılık](./service-fabric-reliable-actors-introduction.md#concurrency)nedeniyle aktör, bağımsız nesneler olarak en iyi şekilde kullanılır. Birden çok aktör, zaman uyumlu yöntem çağrısı (her biri büyük olasılıkla ayrı bir ağ çağrısı haline gelir) veya dairesel aktör istekleri oluşturma gibi grafikler oluşturmayın. Bunlar, performansı ve ölçeği önemli ölçüde etkiler.
- Zaman uyumsuz kodla eşitleme kodunu karıştırmayın. Performans sorunlarını engellemek için zaman uyumsuz olarak sürekli kullanın.
- Aktörlerin uzun süre çalışan çağrıları yapmayın. Uzun süre çalışan çağrılar, çift tabanlı eşzamanlılık nedeniyle aynı aktöre yapılan diğer çağrıları engeller.
- [Service Fabric uzaktan iletişimi](./service-fabric-reliable-services-communication-remoting.md) kullanarak diğer hizmetlerle iletişim kursanız ve oluştururken `ServiceProxyFactory` , aktör düzeyinde *değil* , [aktör hizmeti](./service-fabric-reliable-actors-using.md) düzeyinde fabrika oluşturun.


## <a name="application-diagnostics"></a>Uygulama Tanılama
Hizmet çağrılarında [uygulama günlüğü](./service-fabric-diagnostics-event-generation-app.md) ekleme hakkında ayrıntılı bilgi. Hizmetlerin birbirini çağırabileceği senaryoları tanılamanıza yardımcı olur. Örneğin, bir A çağrısı B C çağrısını çağırdığında, çağrı her yerde başarısız olabilir. Yeterli günlük kaydı yoksa, hataların tanılanabilmesi zordur. Çağrı birimleri nedeniyle hizmetler çok fazla günlüğe kaydedildiğine göre, en azından hataların ve uyarıların günlüğe kaydedildiğine dikkat edin.

## <a name="iot-and-messaging-applications"></a>IoT ve mesajlaşma uygulamaları
[Azure IoT Hub](../iot-hub/index.yml) veya [Azure Event Hubs](../event-hubs/index.yml)Iletileri okurken [servicefabricprocessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/ServiceFabricProcessor)kullanın. ServiceFabricProcessor, Olay Hub 'ı bölümlerinden okuma durumunu korumak ve yöntemi aracılığıyla hizmetlerinize yeni iletiler vermek için Service Fabric Reliable Services ile tümleşir `IEventProcessor::ProcessEventsAsync()` .


## <a name="design-guidance-on-azure"></a>Azure 'da Tasarım Kılavuzu
* [Azure 'da mikro hizmetler oluşturma](/azure/architecture/microservices/)hakkında tasarım kılavuzu için [Azure mimari merkezini](/azure/architecture/microservices/) ziyaret edin.

* [Oyun hizmetlerinde Service Fabric kullanma](/gaming/azure/reference-architectures/multiplayer-synchronous-sf)hakkında tasarım kılavuzu için [Azure ile çalışmaya başlama](/gaming/azure/) makalesini ziyaret edin.
