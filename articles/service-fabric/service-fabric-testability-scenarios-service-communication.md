---
title: 'Test edilebilirlik: Hizmet iletişimi'
description: Servise hizmet iletişimi, Bir Hizmet Kumaşı uygulamasının kritik bir entegrasyon noktasıdır. Bu makalede, tasarım konuları ve test teknikleri anlatılmaktadır.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 87b922cb9655588a22c739d26c9ce9e49d35781a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75465555"
---
# <a name="service-fabric-testability-scenarios-service-communication"></a>Hizmet Kumaş test edilebilirlik senaryoları: Hizmet iletişimi
Azure Hizmet Kumaşı'nda mikro hizmetler ve hizmet odaklı mimari stiller doğal olarak ortaya çıkar. Bu tür dağıtılmış mimarilerde, bileşenleştirilmiş mikrohizmet uygulamaları genellikle birbiriyle konuşması gereken birden çok hizmetlerden oluşur. En basit durumlarda bile, genellikle en az bir stateless web hizmeti ve iletişim kurmak için gereken bir stateful veri depolama hizmeti var.

Hizmete ileti iletişim, her hizmet uzak bir API'yi diğer hizmetlere maruz kattığından, bir uygulamanın kritik bir tümleştirme noktasıdır. G/Ç içeren bir dizi API sınırıyla çalışmak genellikle iyi miktarda test ve doğrulama yla birlikte biraz bakım gerektirir.

Bu hizmet sınırları dağıtılmış bir sistemde birbirine bağlandığında dikkat edilmesi gereken çok sayıda husus vardır:

* *Ulaşım protokolü.* Artırılmış birlikte çalışabilirlik için HTTP'yi mi yoksa maksimum iş verimi için özel bir ikili protokol mü kullanacaksınız?
* *Hata işleme*. Kalıcı ve geçici hatalar nasıl işlenir? Bir hizmet farklı bir düğüme geçtiğinde ne olur?
* *Zaman zaman dilimleri ve gecikme süresi.* Çok katmanlı uygulamalarda, her hizmet katmanı yığın aracılığıyla ve kullanıcıya gecikmeyi nasıl işler?

Service Fabric tarafından sağlanan yerleşik hizmet iletişim bileşenlerinden birini kullanın veya kendi aracınızı oluşturun, hizmetleriniz arasındaki etkileşimleri test etmek, uygulamanızda esnekliği sağlamak için çok önemlidir.

## <a name="prepare-for-services-to-move"></a>Taşıma hizmetleri için hazırlanın
Hizmet örnekleri zaman içinde hareket edebilir. Bu, özellikle özel olarak uyarlanmış en uygun kaynak dengelemeiçin yük ölçümleri ile yapılandırıldığında geçerlidir. Service Fabric, dağıtılmış bir sistemin ömrü boyunca gerçekleşen yükseltmeler, arızalar, ölçeklendirme ler ve diğer durumlarda bile bunların kullanılabilirliğini en üst düzeye çıkarmak için hizmet örneklerinizi taşır.

Hizmetler kümede hareket ettikçe, müşterileriniz ve diğer hizmetleriniz bir hizmetle konuştuklarında iki senaryoyu işlemek üzere hazır olmalıdır:

* Hizmet örneği veya bölüm yinelemesi, son konuştuğunuz dan beri taşındı. Bu, bir hizmet yaşam döngüsünün normal bir parçasıdır ve uygulamanızın ömrü boyunca gerçekleşmesi beklenmelidir.
* Hizmet örneği veya bölüm yinelemesi taşıma işlemindedir. Bir hizmetin bir düğümden diğerine geçememesi Hizmet Kumaşı'nda çok hızlı bir şekilde gerçekleşse de, hizmetin iletişim bileşeninin yavaş başlaması durumunda kullanılabilirlikte bir gecikme olabilir.

Bu senaryoları incelikle işlemek, düzgün çalışan bir sistem için önemlidir. Bunu yapmak için, unutmayın:

* Bağlanabilen her hizmetin dinlediği bir *adres* vardır (örneğin, HTTP veya WebSockets). Bir hizmet örneği veya bölüm hareket ettiğinde, adresi bitiş noktası değişir. (Farklı bir IP adresi ile farklı bir düğüme taşınır.) Yerleşik iletişim bileşenlerini kullanıyorsanız, bunlar sizin için hizmet adreslerini yeniden çözme işlemlerini işler.
* Hizmet örneği dinleyicisini yeniden başlattın. Bu, hizmet örneği taşındıktan sonra hizmetin dinleyiciyi ne kadar hızlı açtığına bağlıdır.
* Varolan bağlantıların, hizmet yeni bir düğümde açıldıktan sonra kapatılması ve yeniden açılması gerekir. Zarif bir düğüm kapatma veya yeniden başlatma, varolan bağlantıların düzgün bir şekilde kapatılması için zaman sağlar.

### <a name="test-it-move-service-instances"></a>Test edin: Hizmet örneklerini taşıma
Service Fabric'in test edilebilirlik araçlarını kullanarak, bu durumları farklı şekillerde sınamak için bir test senaryosu yazabilirsiniz:

1. Devlet hizmetinin birincil yinelemesi taşıyın.
   
    Durumlu hizmet bölümü birincil yinelemesi herhangi bir sayıda nedenlerle taşıntılabilir. Hizmetlerinizin taşımaya çok kontrollü bir şekilde nasıl tepki verdiğini görmek için belirli bir bölümün birincil kopyasını hedeflemek için bunu kullanın.
   
    ```powershell
   
    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService
   
    ```
2. Düğümü durdur.
   
    Bir düğüm durdurulduğunda, Hizmet Dokusu bu düğümdeki tüm hizmet örneklerini veya bölümleri kümedeki diğer kullanılabilir düğümlerden birine taşır. Kümenizden bir düğümün kaybolduğu ve düğümüzerindeki tüm hizmet örneklerinin ve yinelemelerinin taşınması gereken bir durumu sınamak için bunu kullanın.
   
    PowerShell **Stop-ServiceFabricNode** cmdlet kullanarak düğümü durdurabilirsiniz:
   
    ```powershell
   
    PS > Stop-ServiceFabricNode -NodeName Node_1
   
    ```

## <a name="maintain-service-availability"></a>Hizmet kullanılabilirliğini koruma
Bir platform olarak Service Fabric, hizmetlerinizin yüksek kullanılabilirliğini sağlamak üzere tasarlanmıştır. Ancak aşırı durumlarda, altta yatan altyapı sorunları yine de kullanılamamasına neden olabilir. Bu senaryoları test etmek de önemlidir.

Devlet hizmetleri, yüksek kullanılabilirlik için durumu çoğaltmak için çoğunluk tabanlı bir sistem kullanır. Bu, yazma işlemleri gerçekleştirmek için bir çoğaltma çoğunluğu kullanılabilir olması gerektiği anlamına gelir. Yaygın bir donanım hatası gibi nadir durumlarda, bir çok çoğaltma kullanılabilir olmayabilir. Bu gibi durumlarda, yazma işlemleri gerçekleştirmek mümkün olmayacaktır, ancak yine de okuma işlemleri gerçekleştirmek mümkün olacak.

### <a name="test-it-write-operation-unavailability"></a>Test edin: İşlem inavailability yazma
Service Fabric'teki test edilebilirlik araçlarını kullanarak, bir test olarak çoğunluk kaybına neden olan bir hata enjekte edebilirsiniz. Böyle bir senaryo nadir olsa da, devlet hizmetine bağlı olan istemci ve hizmetlerin, bu tür bir hizmete yazma isteğinde bulunamayacakları durumları işlemeye hazır olmaları önemlidir. Ayrıca, devlet hizmetinin kendisinin bu olasılığın farkında olması ve arayanlara incelikle iletebildiği de önemlidir.

PowerShell **Invoke-ServiceFabricPartitionQuorumLoss** cmdlet kullanarak çoğunluk kaybına neden olabilirsiniz:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

Bu örnekte, `QuorumLossMode` tüm `QuorumReplicas` kopyaları çökertmeden çoğunluk kaybına neden olmak istediğimizi belirtmek için ayarlayın. Bu şekilde, okuma işlemleri hala mümkündür. Tüm bölümün kullanılamadığı bir senaryoyu sınamak için bu `AllReplicas`anahtarı da 'ye ayarlayabilirsiniz

## <a name="next-steps"></a>Sonraki adımlar
[Test edilebilirlik eylemleri hakkında daha fazla bilgi edinin](service-fabric-testability-actions.md)

[Test edilebilirlik senaryoları hakkında daha fazla bilgi edinin](service-fabric-testability-scenarios.md)

