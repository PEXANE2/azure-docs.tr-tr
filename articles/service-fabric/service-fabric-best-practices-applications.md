---
title: Azure Hizmet Kumaşı uygulama tasarımı en iyi uygulamalar
description: Azure Hizmet Kumaşı'nı kullanarak uygulama ve hizmet geliştirmek için en iyi uygulamalar ve tasarım konuları.
author: markfussell
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: mfussell
ms.openlocfilehash: 876980bd6a59bace9ab4e490358964d19fa52c7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586096"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Azure Hizmet Kumaşı uygulama tasarımı en iyi uygulamalar

Bu makalede, Azure Hizmet Kumaşı'nda uygulama ve hizmet oluşturmak için en iyi uygulama kılavuzu sağlanmaktadır.
 
## <a name="get-familiar-with-service-fabric"></a>Servis Kumaşı'nı yakından tanısın
* Servis [Kumaşı hakkında bilgi edinmek istediğiniz So'yu okuyun?](service-fabric-content-roadmap.md)
* Service [Fabric uygulama senaryoları](service-fabric-application-scenarios.md)hakkında bilgi edinin.
* [Service Fabric programlama modeline genel bakışı](service-fabric-choose-framework.md)okuyarak programlama modeli seçeneklerini anlayın.



## <a name="application-design-guidance"></a>Uygulama tasarımı kılavuzu
Service Fabric uygulamalarının [genel mimarisine](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) ve [tasarım konularına](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations)aşina olun.

### <a name="choose-an-api-gateway"></a>Bir API ağ geçidi seçin
Daha sonra ölçeklendirilebilir arka uç hizmetleri ile iletişim kuran bir API ağ geçidi hizmeti kullanın. Kullanılan en yaygın API ağ geçidi hizmetleri şunlardır:

- [Hizmet Kumaşı ile entegre](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-api-management)olan Azure [API Yönetimi.](https://docs.microsoft.com/azure/service-fabric/service-fabric-api-management-overview)
- [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) veya [Azure Etkinlik Hub'ları](https://docs.microsoft.com/azure/event-hubs/), Event Hub bölümlerinden okumak için [ServiceFabricProcessor'i](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Microsoft.Azure.EventHubs.ServiceFabricProcessor) kullanarak.
- [Træfik ters proxy](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/), [Azure Servis Kumaş sağlayıcısı](https://docs.traefik.io/v1.6/configuration/backends/servicefabric/)nı kullanarak .
- [Azure Uygulama Ağ Geçidi](https://docs.microsoft.com/azure/application-gateway/).

   > [!NOTE] 
   > Azure Application Gateway, Service Fabric ile doğrudan tümleşik değildir. Azure API Yönetimi genellikle tercih edilen seçimdir.
- Kendi özel ASP.NET [Core](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore) web uygulama ağ geçidi.

### <a name="stateless-services"></a>Devletsiz hizmetler
[Güvenilir Hizmetler'i](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) kullanarak ve durumu bir Azure veritabanında, Azure Cosmos DB'de veya Azure Depolama'da depolayarak her zaman devletsiz hizmetler oluşturmaya başlamanızı tavsiye ettik. Dışsallaştırılmış durum çoğu geliştirici için daha tanıdık bir yaklaşımdır. Bu yaklaşım, mağazadaki sorgu özelliklerinden yararlanmanızı da sağlar.  

### <a name="when-to-use-stateful-services"></a>Devlet hizmetlerinin ne zaman kullanılacağı
Düşük gecikme sonu için bir senaryonuz olduğunda ve verileri bilgi işlemle yakın tutmanız gerektiğinde, devlet tarafından sunulan hizmetleri göz önünde bulundurun. Bazı örnek senaryolar arasında IoT dijital ikiz aygıtları, oyun durumu, oturum durumu, veritabanından veri önbelleğe alma ve diğer hizmetlere yapılan çağrıları izlemek için uzun süren iş akışları yer almaktadır.

Veri saklama süresine karar verin:

- **Önbelleğe alınmış veriler.** Dış mağazalara gecikme bir sorun olduğunda önbelleğe alma kullanın. Kendi veri önbelleği olarak özel bir hizmet kullanın veya [açık kaynak SoCreate Service Fabric Distributed Önbelleği](https://github.com/SoCreate/service-fabric-distributed-cache)kullanmayı düşünün. Bu senaryoda, önbellekteki tüm verileri kaybederseniz endişelenmenize gerek yoktur.
- **Zamana bağlı veriler.** Bu senaryoda, gecikme süresi için verileri bilgi işlem için yakın tutmanız gerekir, ancak bir *felakette*verileri kaybetmeyi göze alabilirsiniz. Örneğin, birçok IoT çözümünde, son birkaç gündeki ortalama sıcaklığın hesaplandığı, ancak bu verilerin kaybolması durumunda kaydedilen belirli veri noktalarının önemli olmadığı gibi, verilerin hesaplamaya yakın olması gerekir. Ayrıca, bu senaryoda genellikle tek tek veri noktalarını yedeklemeyi umursamazsınız. Yalnızca dış depolamaya periyodik olarak yazılmış hesaplanmış ortalama değerleri yedeklersiniz.  
- **Uzun vadeli veriler**. Güvenilir koleksiyonlar verilerinizi kalıcı olarak depolayabilir. Ancak bu durumda, kümeleriniz için periyodik [yedekleme ilkelerini yapılandırma](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-configure-periodic-backup) da dahil olmak üzere [olağanüstü durum kurtarma için hazırlanmanız](https://docs.microsoft.com/azure/service-fabric/service-fabric-disaster-recovery)gerekir. Sonuç olarak, kümeniz bir felakette yok edilirse, yeni bir küme oluşturmanız gereken bir yerde ne olacağını ve yeni uygulama örneklerini nasıl dağıtacağınıve en son yedeklemeden nasıl kurtarAcağınız konusunda yapılandırAbilirsiniz.

Maliyetlerden tasarruf edin ve kullanılabilirliği artırın:
- Uzak mağazadan veri erişimi ve işlem maliyetleri ne tabi değildir ve Redis için Azure Önbelleği gibi başka bir hizmet kullanmanız gerekmedığından, durum bilgisi olan hizmetleri kullanarak maliyetleri azaltabilirsiniz.
- İşlem için değil, öncelikle depolama için değil, devlet hizmetlerinin kullanılması pahalıdır ve bunu önermiyoruz. Ucuz yerel depolama ile bilgi işlem olarak devlet hizmetleri düşünün.
- Diğer hizmetlere olan bağımlılıkları kaldırarak, hizmet kullanılabilirliğinizi artırabilirsiniz. Kümedeki HA ile durumu yönetmek sizi diğer hizmet düşüş saatlerinden veya gecikme sorunlarından yalıtır.

## <a name="how-to-work-with-reliable-services"></a>Güvenilir Hizmetlerle Nasıl ÇalışUlur?
Service Fabric Reliable Services, kolayca vatansız ve stateful hizmetler oluşturmanıza olanak sağlar. Daha fazla bilgi [için Güvenilir Hizmetler'e giriş](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction)e bakın.
- Her zaman stateless ve `RunAsync()` stateful hizmetler ve stateful `ChangeRole()` hizmetler için yöntem de [iptal belirteci](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-lifecycle#stateful-service-primary-swaps) onur. Bunu yapmazsanız, Service Fabric servisinizin kapatıp kapamadığını bilmez. Örneğin, iptal jetonuna uymazsanız, çok daha uzun uygulama yükseltme süreleri oluşabilir.
-   [İletişim dinleyicilerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication) zamanında açın ve kapatın ve iptal jetonları onurlandırın.
-   Eşitleme kodunu asla async koduyla karıştırmayın. Örneğin, async aramalarınızda kullanmayın. `.GetAwaiter().GetResult()` Arama yığını *boyunca async* kullanın.

## <a name="how-to-work-with-reliable-actors"></a>Güvenilir Aktörlerle Nasıl ÇalışUlur?
Hizmet Kumaş Güvenilir Aktörler kolayca stateful, sanal aktörler oluşturmanıza olanak sağlar. Daha fazla bilgi [için, Güvenilir Aktörler giriş](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction)bakın.

- Cidden uygulamanızı ölçeklendirmek için aktörler arasında pub / alt mesajlaşma kullanmayı düşünün. Bu hizmeti sağlayan araçlar arasında [açık kaynak kodlu SoCreate Service Fabric Pub/Sub](https://service-fabric-pub-sub.socreate.it/) ve [Azure Hizmet Veri Servisi](https://docs.microsoft.com/azure/service-bus/)bulunmaktadır.
- Aktör durumunu [olabildiğince ayrıntılı](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices)yapın.
- [Aktörün yaşam döngüsünü](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices)yönetin. Bir daha kullanmayacaksanız aktörleri silin. Tüm durum bellekte depolandığı [için, geçici durum sağlayıcısını](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#state-persistence-and-replication)kullanırken gereksiz aktörleri silmek özellikle önemlidir.
- [Sıra tabanlı eşzamanlılıkları](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction#concurrency)nedeniyle, aktörler en iyi bağımsız nesneler olarak kullanılır. Çok aktörlü, eşzamanlı yöntem çağrılarının (her biri büyük olasılıkla ayrı bir ağ çağrısına dönüşen) grafiklerini oluşturmayın veya dairesel aktör istekleri oluşturun. Bunlar performansı ve ölçeği önemli ölçüde etkileyecektir.
- Eşitleme kodunu async koduyla karıştırmayın. Performans sorunlarını önlemek için tutarlı bir şekilde async kullanın.
- Aktörlerle uzun süreli aramalar yapma. Uzun süreli aramalar, sıra tabanlı eşzamanlılık nedeniyle aynı aktöre yapılan diğer çağrıları engelleyecektir.
- [Service Fabric remoting](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-remoting) kullanarak diğer hizmetlerle iletişim kuruyorsanız ve `ServiceProxyFactory`bir , aktör düzeyinde *değil,* [aktör-hizmet](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-using) düzeyinde fabrika oluşturun.


## <a name="application-diagnostics"></a>Uygulama tanılama
Hizmet çağrılarına [uygulama günlüğe kaydetme](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) konusunda ayrıntılı bilgi olun. Hizmetlerin birbirini aradığı senaryoları tanılamanıza yardımcı olur. Örneğin, B çağrıları C'yi aradığında, arama her yerde başarısız olabilir. Yeterli günlüğe kaydetmeniz yoksa, hataları tanılamak zordur. Hizmetler arama birimleri nedeniyle çok fazla günlüğe kaydediliyorsa, en azından hataları ve uyarıları günlüğe kaydedin.

## <a name="iot-and-messaging-applications"></a>IoT ve mesajlaşma uygulamaları
[Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) veya Azure Etkinlik [Hub'larından](https://docs.microsoft.com/azure/event-hubs/)gelen iletileri okurken [ServiceFabricProcessor'i](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor)kullanın. ServiceFabricProcessor, etkinlik merkezi bölümlerinden okuma durumunu korumak için Service Fabric Reliable Services ile bütünleşir `IEventProcessor::ProcessEventsAsync()` ve yöntem le hizmetlerinize yeni iletiler getirir.


## <a name="design-guidance-on-azure"></a>Azure'da tasarım kılavuzu
* Azure'da mikro hizmetler oluşturma yla ilgili tasarım kılavuzu için [Azure mimari merkezini](https://docs.microsoft.com/azure/architecture/microservices/) ziyaret [edin.](https://docs.microsoft.com/azure/architecture/microservices/)

* [Oyun hizmetlerinde Hizmet Kumaşı'nı kullanma](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf)yla ilgili tasarım kılavuzu için [Azure for Gaming ile başlayın'ı](https://docs.microsoft.com/gaming/azure/) ziyaret edin.
