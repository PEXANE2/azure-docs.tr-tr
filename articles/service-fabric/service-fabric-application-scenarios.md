---
title: Uygulama senaryoları ve tasarımı
description: Service Fabric'teki bulut uygulama kategorilerine genel bakış. Durum lu ve durumsuz hizmetleri kullanan uygulama tasarımını tartışır.
ms.topic: conceptual
ms.date: 01/08/2020
ms.custom: sfrev
ms.openlocfilehash: 0aeb8ab2923915befdd11f96025687be3b3c4ff9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76024747"
---
# <a name="service-fabric-application-scenarios"></a>Service Fabric uygulama senaryoları

Azure Hizmet Kumaşı, birçok iş uygulaması ve hizmeti yazabileceğiniz ve çalıştırabileceğiniz güvenilir ve esnek bir platform sunar. Bu uygulamalar ve mikro hizmetler durum suz veya durumlu olabilir ve verimliliği en üst düzeye çıkarmak için sanal makineler arasında kaynak dengelidir.

Service Fabric'in benzersiz mimarisi, uygulamalarınızda neredeyse gerçek zamanlı veri analizi, bellek içi hesaplama, paralel işlemler ve olay işleme işlemlerini gerçekleştirmenize olanak tanır. Değişen kaynak gereksinimlerinize bağlı olarak uygulamalarınızı kolayca ölçeklendirebilir veya dışarıda yapabilirsiniz.

Bina uygulamaları yla ilgili tasarım kılavuzu için, [Azure Hizmet Kumaşı'nda Microservices mimarisini](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) ve [Hizmet Kumaşı'nı kullanarak uygulama tasarımı için en iyi uygulamaları](service-fabric-best-practices-applications.md)okuyun.

Aşağıdaki uygulama türleri için Service Fabric platform'u kullanmayı düşünün:

* **Veri toplama, işleme ve IoT**: Service Fabric büyük ölçekli işler ve devlet hizmetleri ile düşük gecikme vardır. Aygıt ve hesaplama verilerinin bir araya geldiği milyonlarca cihazdaki verilerin işlenmesine yardımcı olabilir.

    Servis Kumaş ı kullanarak IoT hizmetleri inşa etmiş müşteriler [Honeywell](https://customers.microsoft.com/story/honeywell-builds-microservices-based-thermostats-on-azure)dahil , [PCL İnşaat](https://customers.microsoft.com/story/pcl-construction-professional-services-azure), [Crestron](https://customers.microsoft.com/story/crestron-partner-professional-services-azure), [BMW](https://customers.microsoft.com/story/bmw-enables-driver-mobility-via-azure-service-fabric/), [Schneider Electric](https://customers.microsoft.com/story/schneider-electric-powers-engergy-solutions-on-azure-service-fabric), ve [Örgü Sistemleri](https://customers.microsoft.com/story/mesh-systems-lights-up-the-market-with-iot-based-azure-solutions).

* **Oyun ve oturum tabanlı etkileşimli uygulamalar**: Uygulamanız çevrimiçi oyun veya anlık mesajlaşma gibi düşük gecikme süresi okuma ve yazma gerektiriyorsa Service Fabric kullanışlıdır. Service Fabric, ayrı bir mağaza veya önbellek oluşturmak zorunda kalmadan bu etkileşimli ve durumlu uygulamaları oluşturmanıza olanak tanır. Hizmet Kumaşı'nı [oyun hizmetlerinde kullanma](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf)yla ilgili tasarım kılavuzu için Azure oyun [çözümlerini](https://azure.microsoft.com/solutions/gaming/) ziyaret edin.

    Oyun hizmetleri inşa etmiş müşteriler [Next Games](https://customers.microsoft.com/story/next-games-media-telecommunications-azure) ve [Digamore](https://customers.microsoft.com/story/digamore-entertainment-scores-with-a-new-gaming-platform-based-on-azure-service-fabric/)içerir. Etkileşimli oturumlar inşa etmiş müşteriler [hololens ile Honeywell](https://customers.microsoft.com/story/honeywell-manufacturing-hololens)içerir.

* **Veri analizi ve iş akışı işleme**: Olayları veya veri akışlarını güvenilir bir şekilde işlemesi gereken uygulamalar, Service Fabric'te en iyi duruma getirilmiş okuma ve yazmalardan yararlanır. Service Fabric ayrıca, sonuçların güvenilir olması ve herhangi bir kayıp olmadan bir sonraki işlem aşamasına geçirilmesi gereken uygulama işleme boru hatlarını da destekler. Bu boru hatları, veri tutarlılığı ve hesaplama garantilerinin gerekli olduğu işlemsel ve finansal sistemleri içerir.

    İş akışı hizmetleri kuran müşteriler arasında [Zeiss Group,](https://customers.microsoft.com/story/zeiss-group-focuses-on-azure-service-fabric-for-key-integration-platform) [Quorum Business Solutions](https://customers.microsoft.com/en-us/story/quorum-business-solutions-expand-energy-managemant-solutions-using-azure-service-fabric)ve [Société General](https://customers.microsoft.com/en-us/story/societe-generale-speeds-real-time-market-quotes-using-azure-service-fabric)sayılabilir.

* **Veriler üzerinde hesaplama**: Service Fabric, yoğun veri hesaplaması yapan durumlu uygulamalar oluşturmanıza olanak tanır. Service Fabric, işlemlerin (hesaplama) ve verilerin uygulamalarda bir araya taşınmasına olanak tanır. 

   Normalde, uygulamanız verilere erişim gerektirdiğinde, harici bir veri önbelleği veya depolama katmanıyla ilişkili ağ gecikmesi hesaplama süresini sınırlar. Stateful Service Fabric hizmetleri, daha optimize edilmiş okuma ve yazma sağlayarak bu gecikmeyi ortadan kaldırır.

   Örneğin, 100 milisaniyeden daha az bir gidiş-dönüş süresi gereksinimi olan, müşteriler için gerçek zamanlı öneri seçimlerinin yakınında performans gösteren bir uygulama düşünün. Service Fabric hizmetlerinin gecikme ve performans özellikleri, gerekli verileri uzak depolamadan almak zorunda olan standart uygulama modeliyle karşılaştırıldığında kullanıcıya duyarlı bir deneyim sağlar. Öneri seçiminin hesaplanması veriler ve kurallarla birlikte bulunduğundan sistem daha duyarlıdır.

    Hesaplama hizmetleri oluşturmuş müşteriler [solidsoft yanıt](https://customers.microsoft.com/story/solidsoft-reply-platform-powers-e-verification-of-pharmaceuticals) ve [Infosupport](https://customers.microsoft.com/story/service-fabric-customer-profile-info-support-and-fudura)içerir.

* **Yüksek kullanılabilir hizmetler**: Service Fabric, birden çok ikincil hizmet yinelemesi oluşturarak hızlı bir şekilde başarısız olur. Donanım veya başka bir hata nedeniyle bir düğüm, işlem veya tek tek hizmet düşerse, ikincil yinelemelerden biri en az hizmet kaybıyla birincil yinelemeye yükseltilir.

* **Ölçeklenebilir hizmetler**: Tek tek hizmetler, durum küme boyunca ölçeklendirilebilir izin bölümlenebilir. Bireysel hizmetler de oluşturulabilir ve anında kaldırılabilir. Hizmetleri birkaç düğümdeki birkaç örnekten, birçok düğümdeki binlerce örneğe ölçeklendirebilir ve gerektiğinde yeniden ölçeklendirebilirsiniz. Service Fabric'i bu hizmetleri oluşturmak ve tüm yaşam döngülerini yönetmek için kullanabilirsiniz.

## <a name="application-design-case-studies"></a>Uygulama tasarımı vaka çalışmaları

Hizmet Kumaşı'nın uygulamaları tasarlamak için nasıl kullanıldığını gösteren örnek çalışmalar, Azure sitelerindeki [Müşteri hikayelerinde](https://customers.microsoft.com/search?sq=%22Azure%20Service%20Fabric%22&ff=&p=2&so=story_publish_date%20desc) ve [Mikro hizmetlerde](https://azure.microsoft.com/solutions/microservice-applications/) yayınlanır.

## <a name="designing-applications-composed-of-stateless-and-stateful-microservices"></a>Stateless ve stateful microservices oluşan uygulamalar tasarımı

Azure Bulut Hizmetleri çalışanı rolleriyle uygulama oluşturma, devletsiz bir hizmetörneğidir. Buna karşılık, devlet eki mikro hizmetler, yetkili durumlarını istek ve yanıtın ötesinde korurlar. Bu işlevsellik, çoğaltma tarafından desteklenen işlem garantileri sağlayan basit API'ler aracılığıyla yüksek kullanılabilirlik ve durum tutarlılığı sağlar.

Service Fabric'teki devlet hizmetleri, yalnızca veritabanları ve diğer veri depolarına değil, tüm uygulama türlerine yüksek kullanılabilirlik sağlar. Bu doğal bir ilerlemedir. Uygulamalar zaten NoSQL veritabanları için yüksek kullanılabilirlik için tamamen ilişkisel veritabanları kullanarak taşınmış. Artık uygulamaların kendileri güvenilirlik, tutarlılık veya kullanılabilirlikten ödün vermeden ek performans kazançları için kendi "sıcak" durumlarını ve verilerini içlerinde yönetilebilir.

Mikro hizmetlerden oluşan uygulamalar oluştururken, genellikle durum dışı web uygulamalarının (ASP.NET ve Node.js gibi) durum dışı ve stateful iş orta katman hizmetlerini çağıran bir birleşimine sahip olursunuz. Uygulamalar ve hizmetler, Service Fabric dağıtım komutları aracılığıyla aynı Hizmet Kumaşı kümesinde dağıtılır. Bu hizmetlerin her biri ölçek, güvenilirlik ve kaynak kullanımı açısından bağımsızdır. Bu bağımsızlık, gelişim ve yaşam döngüsü yönetiminde çevikliği ve esnekliği artırır.

Devlete uygun mikro hizmetler, tamamen durum suz uygulamaların kullanılabilirlik ve gecikme gereksinimlerini gidermek için geleneksel olarak gerekli olan ek kuyruklar ve önbelleklere olan gereksinimi ortadan kaldırdığından uygulama tasarımlarını basitleştirir. Devlet hizmetlerinin yüksek kullanılabilirlik ve düşük gecikme süresi olduğundan, uygulamanızda yönetilen daha az ayrıntı vardır.

Aşağıdaki diyagramlar, durum dilsiz bir uygulama tasarlamak ile durum dolu bir uygulama tasarlamak arasındaki farkları göstermektedir. [Güvenilir Hizmetler](service-fabric-reliable-services-introduction.md) ve Güvenilir [Aktörler](service-fabric-reliable-actors-introduction.md) programlama modellerinden yararlanarak, devlet hizmetleri yüksek iş ortası ve düşük gecikme süresi elde ederken uygulama karmaşıklığını azaltır.

Aşağıda, devletsiz hizmetleri kullanan örnek ![bir uygulama verilmiştir: Devletsiz hizmetleri kullanan uygulama][Image1]

Aşağıda, devlet tarafından sunulan hizmetleri kullanan ![bir örnek uygulama verilmiştir: Devlet tarafından sunulan hizmetleri kullanan uygulama][Image2]

## <a name="next-steps"></a>Sonraki adımlar

* Service Fabric [Reliable Services](service-fabric-reliable-services-quick-start.md) ve [Reliable Actors](service-fabric-reliable-actors-get-started.md) programlama modelleri ile stateless ve stateful hizmetler oluşturmaya başlayın.
* Azure'da mikro hizmetler oluşturma konusunda rehberlik etmek için Azure Mimari [Merkezi'ni ziyaret edin.](https://docs.microsoft.com/azure/architecture/microservices/)
* Azure [Hizmet Kumaşı uygulamasına](service-fabric-best-practices-overview.md) gidin ve uygulama tasarımı kılavuzu için en iyi uygulamaları kümeleyin.

* Ayrıca bkz:
  * [Mikro hizmetleri anlama](service-fabric-overview-microservices.md)
  * [Hizmet durumunu tanımlama ve yönetme](service-fabric-concepts-state.md)
  * [Hizmetlerin kullanılabilirliği](service-fabric-availability-services.md)
  * [Ölçek hizmetleri](service-fabric-concepts-scalability.md)
  * [Bölümhizmetleri](service-fabric-concepts-partitioning.md)

[Image1]: media/service-fabric-application-scenarios/AppwithStatelessServices.png
[Image2]: media/service-fabric-application-scenarios/AppwithStatefulServices.png
