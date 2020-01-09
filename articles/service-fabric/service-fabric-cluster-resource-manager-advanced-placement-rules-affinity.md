---
title: Service Fabric kümesi Kaynak Yöneticisi-benzeşim
description: Azure Service Fabric Hizmetleri için hizmet benzeşimine genel bakış ve hizmet benzeşimi yapılandırması Kılavuzu.
services: service-fabric
documentationcenter: .net
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7bfd261802fbf891b8f45079255783cb1e8ac7d4
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75551752"
---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Service Fabric 'de hizmet benzeşimini yapılandırma ve kullanma
Benzeşim, genellikle daha büyük tek parçalı uygulamaların bulut ve mikro hizmet dünyasına geçişini kolaylaştırmak için sağlanan bir denetimdir. Ayrıca, hizmetin performansını iyileştirmek için bir iyileştirme olarak da kullanılır, ancak bunu yapmak yan etkilere sahip olabilir.

Daha büyük bir uygulama ya da mikro hizmetlerle tasarlanmayan bir Service Fabric (ya da herhangi bir dağıtılmış ortam) gibi bir uygulama olduğunuzu varsayalım. Bu tür bir geçiş yaygındır. Uygulamanın tamamını ortama kaldırarak, paketleyerek ve sorunsuz çalıştığından emin olarak çalışmaya başlayabilirsiniz. Daha sonra, hepsi birbirleriyle konuşacak farklı bir daha küçük hizmetlere kadar parçalara başlayabilirsiniz.

Sonuç olarak, uygulamanın bazı sorunlar yaşadığını fark edebilirsiniz. Sorunlar genellikle bu kategorilerden birine girer:

1. Tek parçalı uygulamalarda bulunan X bileşeni, bileşen Y üzerinde belgelenmiş bir bağımlılığa sahipti ve bu bileşenleri yalnızca ayrı hizmetlere açtınız. Bu hizmetler artık kümedeki farklı düğümlerde çalıştığından, bunlar bozulur.
2. Bu bileşenler (Yerel adlandırılmış kanallar | paylaşılan bellek | diskte bulunan dosyalar) üzerinden iletişim kurar ve şu anda performans nedenleriyle paylaşılan bir yerel kaynağa yazamayacak olmaları gerekir. Bu sabit bağımlılık daha sonra, belki de kaldırılır.
3. Her şey iyidir, ancak bu iki bileşenin gerçekten geveze/performans duyarlı olduğunu döndürür. Bunları ayrı hizmetlere taşıdıklarında, genel uygulama performansı veya gecikme süresi artar. Sonuç olarak, genel uygulama beklentileri karşılamıyor.

Bu durumlarda, yeniden düzenleme çalışmalarımızı kaybetmek istemiyorum ve tek bir duruma dönmek istemiyorum. Son koşul, düz iyileştirme olarak bile istenebilir. Bununla birlikte, bileşenleri doğal olarak hizmet olarak çalışacak şekilde yeniden tasarlayabilmemiz için (veya performans beklentilerini başka bir şekilde çözebilmemiz için), bazı yere bir anlamlı ihtiyacımız olacak.

Ne yapmanız gerekir? Ayrıca, benzeşimi açmayı deneyebilirsiniz.

## <a name="how-to-configure-affinity"></a>Benzeşimi yapılandırma
Benzeşimi ayarlamak için iki farklı hizmet arasında bir benzeşim ilişkisi tanımlarsınız. Benzeşim ' i "bir hizmeti başka bir yerde" işaret ederek, "Bu hizmet yalnızca hizmetin çalıştığı yeri çalıştırılabilir" olarak düşünebilirsiniz. Bazen bir üst/alt ilişki olarak benzeşimine başvurduk (alt öğeyi üst öğede işaret edersiniz). Benzeşim, bir hizmetin çoğaltmalarının veya örneklerinin başka bir hizmetin aynı düğümlere yerleştirilmesini sağlar.

```csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

> [!NOTE]
> Bir alt hizmet yalnızca tek bir benzeşim ilişkisinde yer alabilir. Her seferinde iki üst hizmete karşı alt öğe olmasını istediyseniz, birkaç seçeneğiniz vardır:
> - İlişkileri ters çevirin (geçerli alt hizmette parentService1 ve parentService2 noktası vardır) veya
> - Üst öğelerinden birini, kurala göre hub olarak belirleyin ve bu hizmette tüm hizmetlere sahip yapın. 
>
> Kümedeki sonuç yerleştirme davranışı aynı olmalıdır.
>

## <a name="different-affinity-options"></a>Farklı benzeşim seçenekleri
Benzeşim, çeşitli bağıntı düzenlerinden biri ile temsil edilir ve iki farklı moda sahiptir. En yaygın benzeşim modu, Alignedadınity ' i çağırdığımız şeydir. Yazılı olmayan bir yerde, farklı hizmetlerin çoğaltmaları veya örnekleri aynı düğümlere yerleştirilir. Diğer mod Alignedadınity ' dir. Hizalanmış benzeşim yalnızca durum bilgisi olan hizmetlerle yararlıdır. Hizalı benzeşim sağlamak için durum bilgisi olan iki hizmetin yapılandırılması, bu hizmetlerin baş altındaki nesnelerin birbirleriyle aynı düğümlere yerleştirilmesini sağlar. Ayrıca, bu hizmetler için her bir ikincil çiftin aynı düğümlere yerleştirilmesine neden olur. Durum bilgisi olmayan hizmetler için Alignedadsız yapılandırma için de mümkündür (daha az yaygın). Yazılı olmayan, iki durumlu hizmetin farklı çoğaltmaları aynı düğümlerde çalışır, ancak bunların en önemli bir kısmı farklı düğümlere bitemiyor.

<center>

![benzeşim modlarını ve bunların etkilerini][Image1]
</center>

### <a name="best-effort-desired-state"></a>En iyi çaba istenen durumu
Benzeşim ilişkisi en iyi çabadır. Aynı yürütülebilir işlemde çalışan aynı birlikte bulundurma garantisi veya güvenilirliği sağlamaz. Benzeşim ilişkisindeki hizmetler, başarısız olabilecek ve bağımsız olarak taşınabilecek temel olarak farklı varlıklardır. Benzeşim ilişkisi de kesilebilir, ancak bu kesmeler geçicidir. Örneğin, kapasite sınırlamaları, benzeşim ilişkisindeki bazı hizmet nesnelerinden belirli bir düğüme sığabilecek anlamına gelebilir. Bu durumlarda, aynı yerde bir benzeşim ilişkisi olsa da, diğer kısıtlamalar nedeniyle zorlanamamış olabilir. Bunu yapmak mümkünse, ihlalin daha sonra otomatik olarak düzeltilir.

### <a name="chains-vs-stars"></a>Zincirler ve yıldızlar
Küme Kaynak Yöneticisi, benzeşim ilişkilerinin zincirlerini modelleyebilir. Bunun anlamı, bir benzeşim ilişkisinde alt öğe olan bir hizmetin başka bir benzeşim ilişkisinde üst öğe olamayacağı anlamına gelir. Bu tür bir ilişki modellemek istiyorsanız, onu bir zincir yerine bir yıldız olarak modelleyebilirsiniz. Bir zincirden bir yıldıza taşımak için en alttaki alt öğe onun yerine ilk alt öğenin üst öğesi olur. Hizmetlerinizin düzenlemesine bağlı olarak, bunu birden çok kez yapmanız gerekebilir. Doğal üst hizmet yoksa, bir yer tutucu görevi gören bir tane oluşturmanız gerekebilir. Gereksinimlerinize bağlı olarak, [uygulama gruplarına](service-fabric-cluster-resource-manager-application-groups.md)da bakmak isteyebilirsiniz.

<center>

![zincirler ve][Image2]
benzeşim Ilişkileri bağlamında </center>

Günümüzde benzeşim ilişkileri hakkında daha fazla şey, varsayılan olarak yönlerdir. Bu, benzeşim kuralının yalnızca alt öğenin üst öğeyle birlikte yerleştirilmesini zorladığı anlamına gelir. Üst öğenin alt öğesiyle birlikte bulunduğundan emin değildir. Bu nedenle, bir benzeşim ihlali varsa ve ihlalin bir nedenden dolayı sorunu gidermek için, alt öğenin düğümün düğümüne taşınması uygun değildir; bu durumda, üst öğenin alt öğesine taşınmasının ihlalin düzelmesine neden olur. e alt öğenin düğümü. Config [Moveparenttofixaffinityihlato](service-fabric-cluster-fabric-settings.md) true olarak ayarlamak, yönlülüğü kaldırır. Farklı hizmetler farklı yaşam döngülerine sahip olduğundan ve bağımsız olarak başarısız olduğundan benzeşim ilişkisinin kusursuz veya anında zorlanabildiğine dikkat edin. Örneğin, çöktüğü için üst öğe aniden başka bir düğüme devredildiğini varsayalım. Küme Kaynak Yöneticisi ve Yük Devretme Yöneticisi önce yük devretme işlemini işley, tutarlı ve kullanılabilir durumda tutulması önceliktir. Yük devretme işlemi tamamlandıktan sonra benzeşim ilişkisi bozulur, ancak kümenin üst öğeyle ilgili olmadığını fark edene kadar her şeyi Kaynak Yöneticisi. Bu denetim sıralamaları düzenli aralıklarla gerçekleştirilir. Bu [makalede](service-fabric-cluster-resource-manager-management-integration.md#constraint-types)küme kaynak yöneticisi kısıtlamaların nasıl değerlendirileceği hakkında daha fazla bilgi bulabilirsiniz ve [Bu](service-fabric-cluster-resource-manager-balancing.md) , bu kısıtlamaların değerlendirildiği temposunda nasıl yapılandırılacağı hakkında daha fazla bilgi sağlar.   


### <a name="partitioning-support"></a>Bölümleme desteği
Benzeşim hakkında dikkat edilecek son şey, üst öğenin bölümlendiğine ilişkin benzeşim ilişkilerinin desteklenmediğini unutmayın. Bölümlenmiş üst hizmetler sonunda desteklenebilir, ancak bugün buna izin verilmez.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [Hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Hizmetleri küçük bir makine kümesiyle sınırlandırmak veya hizmetlerin yükünü toplamak için [uygulama gruplarını](service-fabric-cluster-resource-manager-application-groups.md) kullanın

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png