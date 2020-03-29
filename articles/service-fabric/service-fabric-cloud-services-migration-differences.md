---
title: Bulut Hizmetleri ile Hizmet Kumaşı arasındaki farklar
description: Bulut Hizmetlerinden Hizmet Kumaşına geçiş uygulamaları için kavramsal bir genel bakış.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 283ad2c63bb59771dab7881522e737f773ab1705
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75463373"
---
# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Uygulamaları geçirmeden önce Bulut Hizmetleri ve Hizmet Kumaşı arasındaki farklar hakkında bilgi edinin.
Microsoft Azure Hizmet Kumaşı, yüksek ölçeklenebilir ve son derece güvenilir dağıtılmış uygulamalar için yeni nesil bulut uygulama platformudur. Dağıtılmış bulut uygulamalarını paketleme, dağıtma, yükseltme ve yönetme için birçok yeni özellik sunar. 

Bu, Bulut Hizmetleri'nden Hizmet Kumaşı'na geçiş uygulamaları için bir giriş kılavuzudur. Öncelikle Bulut Hizmetleri ve Hizmet Kumaşı arasındaki mimari ve tasarım farklılıklarına odaklanır.

## <a name="applications-and-infrastructure"></a>Uygulamalar ve altyapı
Bulut Hizmetleri ve Hizmet Kumaşı arasındaki temel fark, VM'ler, iş yükleri ve uygulamalar arasındaki ilişkidir. Burada iş yükü, belirli bir görevi gerçekleştirmek veya bir hizmet sağlamak için yazdığınız kod olarak tanımlanır.

* **Bulut Hizmetleri, uygulamaları VM olarak dağıtmakla ilgilidir.** Yazdığınız kod, Web veya İşçi Rolü gibi bir VM örneğine sıkıca birleştiğinde dir. Bulut Hizmetlerinde iş yükünü dağıtmak, iş yükünü çalıştıran bir veya daha fazla VM örneğini dağıtmaktır. Uygulamaların ve VM'lerin ayrılması yoktur ve bu nedenle uygulamanın resmi bir tanımı yoktur. Bir uygulama, Bulut Hizmetleri dağıtımında Web veya İşçi Rolü örnekleri kümesi veya bulut hizmetleri dağıtımının tamamı olarak düşünülebilir. Bu örnekte, bir uygulama rol örnekleri kümesi olarak gösterilir.

![Bulut Hizmetleri uygulamaları ve topoloji][1]

* **Service Fabric, Windows veya Linux'ta Service Fabric çalıştıran mevcut VM'lere veya makinelere uygulama dağıtmakla ilgilidir.** Yazdığınız hizmetler, Hizmet Kumaşı uygulama platformu tarafından soyutlanan temel altyapıdan tamamen ayrılmıştır, böylece bir uygulama birden çok ortama dağıtılabilir. Hizmet Kumaşı'ndaki iş yüküne "hizmet" denir ve bir veya daha fazla hizmet, Service Fabric uygulama platformunda çalışan resmi olarak tanımlanmış bir uygulamada gruplandırılır. Tek bir Service Fabric kümesine birden çok uygulama dağıtılabilir.

![Servis Kumaş uygulamaları ve topoloji][2]

Service Fabric,Windows veya Linux'ta çalışan bir uygulama platformu katmanıdır, bulut hizmetleri ise iş yükü ekli Azure tarafından yönetilen VM'leri dağıtmak için kullanılan bir sistemdir.
Service Fabric uygulama modelinin bir takım avantajları vardır:

* Hızlı dağıtım süreleri. VM örnekleri oluşturmak zaman alabilir. Hizmet Kumaşı'nda, VM'ler Service Fabric uygulama platformlarını barındıran bir küme oluşturmak için yalnızca bir kez dağıtılır. Bu noktadan itibaren, uygulama paketleri kümeye çok hızlı bir şekilde dağıtılabilir.
* Yüksek yoğunluklu barındırma. Bulut Hizmetlerinde, Bir İşçi Rolü VM bir iş yükü barındırar. Hizmet Kumaşı'nda uygulamalar, bunları çalıştıran VM'lerden ayrıdır, bu da çok sayıda uygulamayı daha büyük dağıtımlar için genel maliyeti düşürebilecek az sayıda VM'ye dağıtabileceğiniz anlamına gelir.
* Service Fabric platformu, Ister Azure ister şirket içinde olsun, Windows Server veya Linux makineleri olan her yerde çalıştırılabilir. Platform, uygulamanızın farklı ortamlarda çalıştırabilmesi için temel altyapıüzerinde bir soyutlama katmanı sağlar. 
* Dağıtılmış uygulama yönetimi. Service Fabric, yalnızca dağıtılmış uygulamalara ev sahipliği yapan değil, aynı zamanda barındırma VM veya makine yaşam döngüsünden bağımsız olarak yaşam döngülerini yönetmeye yardımcı olan bir platformdur.

## <a name="application-architecture"></a>Uygulama mimarisi
Bulut Hizmetleri uygulamasının mimarisi genellikle, Web ile Web arasındaki bir uygulamanın durumunu ve verilerini yönetmek için Service Bus, Azure Table ve Blob Depolama, SQL, Redis ve diğerleri gibi çok sayıda dış hizmet bağımlılığı içerir Bulut Hizmetleri dağıtımında Çalışan Rolleri. Bulut Hizmetleri'nin tamambir uygulamasına örnek olarak şu şekilde görünebilir:  

![Bulut Hizmetleri mimarisi][9]

Service Fabric uygulamaları da tam bir uygulamada aynı dış hizmetleri kullanmayı seçebilirsiniz. Bu örnek Bulut Hizmetleri mimarisini kullanarak, Bulut Hizmetlerinden Hizmet Kumaşına en basit geçiş yolu, yalnızca Bulut Hizmetleri dağıtımını bir Hizmet Kumaşı uygulamasıyla değiştirmek ve genel mimariyi aynı tutmaktır. Web ve Çalışan Rolleri, en az kod değişikliğiyle Service Fabric stateless hizmetlerine taşınabilir.

![Basit geçişten sonra Hizmet Kumaş mimarisi][10]

Bu aşamada, sistem eskisi gibi çalışmaya devam etmelidir. Service Fabric'in özel özelliklerinden yararlanan harici devlet mağazaları, mümkün olduğu durumlarda devlet hizmeti olarak içselleştirilebilir. Bu, dış hizmetlerin daha önce yaptığı gibi uygulamanız için eşdeğer işlevsellik sağlayan özel hizmetler yazmayı gerektirdiğinden, Web ve İşçi Rollerinin Hizmet Kumaşı'na basit bir geçişinden daha fazla ilgilidir. Bunu yapmanın yararları şunlardır: 

* Dışa bağımlılıkları kaldırma 
* Dağıtım, yönetim ve yükseltme modellerini birleştirme. 

Bu hizmetlerin içselleştirilmesine örnek bir mimari şu şekilde görünebilir:

![Tam geçiş sonrası Hizmet Kumaş mimarisi][11]

## <a name="communication-and-workflow"></a>İletişim ve iş akışı
Bulut Hizmeti uygulamalarının çoğu birden fazla katmandan oluşur. Benzer şekilde, Bir Hizmet Kumaş uygulaması birden fazla hizmetten (genellikle birçok hizmetten) oluşur. İki yaygın iletişim modeli doğrudan iletişim ve harici dayanıklı depolama yoluyla vardır.

### <a name="direct-communication"></a>Doğrudan iletişim
Doğrudan iletişim sayesinde, katmanlar her katman tarafından açığa çıkarılan uç nokta üzerinden doğrudan iletişim kurabilir. Bulut Hizmetleri gibi durum dışı ortamlarda bu, yükü dengelemek için rasgele veya yuvarlak robin bir VM rolü örneği seçmek ve bitiş noktasına doğrudan bağlanmak anlamına gelir.

![Bulut Hizmetleri doğrudan iletişim][5]

 Doğrudan iletişim, Service Fabric'te ortak bir iletişim modelidir. Service Fabric ve Cloud Services arasındaki temel fark, Bulut Hizmetleri'nde bir VM'ye, Hizmet Kumaşı'nda ise bir hizmete bağlanmanızdır. Bu birkaç nedenden dolayı önemli bir ayrım:

* Hizmet Kumaşı'ndaki hizmetler, barındıran VM'lere bağlı değildir; hizmetler kümede hareket edebilir ve aslında çeşitli nedenlerle hareket etmesi beklenir: kaynak dengeleme, başarısızolma, uygulama ve altyapı yükseltmeleri ve yerleşim veya yük kısıtlamaları. Bu, bir hizmet örneğinin adresinin istediği zaman değişebileceği anlamına gelir. 
* Hizmet Kumaşındaki bir VM, her biri benzersiz uç noktaları olan birden çok hizmete ev sahipliği yapabilir.

Hizmet Dokusu, hizmetlerin uç nokta adreslerini çözmek için kullanılabilecek Adlandırma Hizmeti adı verilen bir hizmet bulma mekanizması sağlar. 

![Hizmet Kumaş doğrudan iletişim][6]

### <a name="queues"></a>Kuyruklar
Bulut Hizmetleri gibi durum dışı ortamlardakatmanlar arasında ortak bir iletişim mekanizması, iş görevlerini bir katmandan diğerine uzun bir şekilde depolamak için harici bir depolama kuyruğu kullanmaktır. Sık karşılaşılan bir senaryo, iş lerini, İşçi Rolü örneklerinin sırayı kesebileceği ve işleri işleyebilir şekilde bir Azure Kuyruğu veya Hizmet Veri Sitesi'ne gönderen bir web katmanıdır.

![Bulut Hizmetleri sıra iletişimi][7]

Aynı iletişim modeli Service Fabric'de de kullanılabilir. Bu, varolan bir Bulut Hizmetleri uygulamasını Hizmet Kumaşı'na geçirdiğinizde yararlı olabilir. 

![Hizmet Kumaş doğrudan iletişim][8]

## <a name="parity"></a>Parity
[Bulut Hizmetleri, kullanım kolaylığına karşı kontrol derecesi olarak Service Fabric'e benzer, ancak artık eski bir hizmettir ve Hizmet Kumaşı yeni geliştirme için önerilir;](https://docs.microsoft.com/azure/app-service/overview-compare) bir API karşılaştırması aşağıdaki gibidir:


| **Bulut Hizmeti API'si** | **Servis Kumaş API** | **Notlar** |
| --- | --- | --- |
| RoleInstance.GetID | FabricRuntime.GetNodeContext.NodeId veya . DüğümAdı | Kimlik, NodeName'nin bir özelliğidir |
| RoleInstance.GetFaultDomain | FabricClient.QueryManager.GetNodeList | DüğümAdı'na filtre uygulayın ve FD Özelliğini kullanın |
| RoleInstance.GetUpgradeDomain | FabricClient.QueryManager.GetNodeList | Düğüm Name'e filtre uygulayın ve Yükseltme özelliğini kullanın |
| RoleInstance.GetInstanceEndpoints | FabricRuntime.GetActivationContext veya Adlandırma (Çözümlü Hizmet) | CodePackageActivationContext hem FabricRuntime.GetActivationContext tarafından sağlanan ve serviceInitializationParameters.CodePackageActivationContext ile çoğaltmalar içinde sırasında sağlanan . Başlatmak |
| RoleEnvironment.GetRoles | FabricClient.QueryManager.GetNodeList | Türüne göre aynı tür filtreleme yapmak istiyorsanız FabricClient.ClusterManager.GetClusterManifest üzerinden küme manifestosundan düğüm türlerinin listesini alabilir ve rol/düğüm türlerini oradan alabilirsiniz. |
| RoleEnvironment.GetisAvailable | Connect-WindowsFabricCluster veya belirli bir düğümişaret bir FabricRuntime oluşturmak | * |
| RoleEnvironment.GetLocalResource | CodePackageActivationContext.Log/Temp/Work | * |
| RoleEnvironment.GetCurrentRoleInstance | CodePackageActivationContext.Log/Temp/Work | * |
| LocalResource.GetRootPath | CodePackageActivationContext.Log/Temp/Work | * |
| Role.GetInstances | FabricClient.QueryManager.GetNodeList veya Çözümlü Hizmet | * |
| RoleInstanceEndpoint.GetIPEndpoint | FabricRuntime.GetActivationContext veya Adlandırma (Çözümlü Hizmet) | * |

## <a name="next-steps"></a>Sonraki Adımlar
Bulut Hizmetlerinden Hizmet Kumaşına en basit geçiş yolu, uygulamanızın genel mimarisini kabaca aynı tutarak yalnızca Bulut Hizmetleri dağıtımını bir Hizmet Kumaşı uygulamasıyla değiştirmektir. Aşağıdaki makale, Bir Web veya İşçi Rolünü Hizmet Kumaşı devletsiz bir hizmete dönüştürmeye yardımcı olacak bir kılavuz sağlar.

* [Basit geçiş: Web veya İşçi Rolünü Hizmet Kumaşı devletsiz bir hizmete dönüştürme](service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
