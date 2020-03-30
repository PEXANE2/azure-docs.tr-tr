---
title: Düşük öncelikli düşük öncelikli VM'lerde iş yüklerini çalıştırma - Azure Toplu İş | Microsoft Dokümanlar
description: Azure Toplu Iş Yükünün maliyetini azaltmak için düşük öncelikli VM'leri nasıl sağatabildiğini öğrenin.
services: batch
author: mscurrell
manager: evansma
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.topic: article
ms.workload: na
ms.date: 03/19/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 9f4b9ed9254eaf950311dd27d5716c4681707614
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80053905"
---
# <a name="use-low-priority-vms-with-batch"></a>Batch ile düşük öncelikli VM’ler kullanma

Azure Toplu İşlem, Toplu Iş Yükünün maliyetini azaltmak için düşük öncelikli sanal makineler (VM'ler) sunar. Düşük öncelikli VM'ler, çok düşük bir maliyetle büyük miktarda bilgi işlem gücünün kullanılmasını sağlayarak yeni toplu iş yükü türlerini mümkün kılar.
 
Düşük öncelikli VM'ler Azure'daki fazla kapasiteden yararlanır. Havuzlarınızda düşük öncelikli VM'ler belirttiğinizde, Azure Toplu İşlem ilerve saolundığında bu fazlalığı kullanabilir.
 
Düşük öncelikli VM'lerin kullanılmasının amacı, bu VM'lerin kullanılabilir kapasiteye bağlı olarak herhangi bir zamanda tahsis edilemeyebilir veya önceden engellenemeyebilir. Bu nedenle, düşük öncelikli VM'ler belirli iş yükü türleri için en uygundur. İş tamamlama süresinin esnek olduğu ve çalışmanın birçok VM'ye dağıtıldığı toplu iş ve eşzamanlı işleme iş yükleri için düşük öncelikli VM'ler kullanın.
 
Düşük öncelikli VM'ler, özel VM'lerle karşılaştırıldığında önemli ölçüde daha düşük bir fiyata sunulur. Fiyatlandırma ayrıntıları için [Toplu Fiyatlandırma'ya](https://azure.microsoft.com/pricing/details/batch/)bakın.

> [!NOTE]
> [Spot VM'ler](https://azure.microsoft.com/pricing/spot/) artık [tek örnekvm'ler](https://docs.microsoft.com/azure/virtual-machines/linux/spot-vms) ve [VM ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot)için kullanılabilir. Spot VM'ler düşük öncelikli VM'lerin bir evrimidir, ancak fiyatlandırmanın farklılık gösterebileceği ve Spot VM'leri ayırırken isteğe bağlı maksimum fiyat belirlenebileceği farklıdır.
>
> Azure Toplu İşlem havuzları, [Toplu İş API'lerinin ve araçlarının](https://docs.microsoft.com/azure/batch/batch-apis-tools)yeni sürümleriyle, genel kullanıma sunulduktan sonraki birkaç ay içinde Spot VM'leri desteklemeye başlayacaktır. Spot VM desteği kullanıma sunulduktan sonra, düşük öncelikli VM'ler amortismana alınacaktır - Spot VM'lere geçiş için yeterli zaman sağlamak için en az 12 ay boyunca geçerli API'ler ve araç sürümleri kullanılarak desteklenmeye devam edeceklerdir. 
>
> Spot VM'ler Bulut [Hizmeti Yapılandırma](https://docs.microsoft.com/rest/api/batchservice/pool/add#cloudserviceconfiguration) havuzları için desteklenmez. Spot VM'leri kullanmak için Bulut Hizmeti havuzlarının [Sanal Makine Yapılandırma](https://docs.microsoft.com/rest/api/batchservice/pool/add#virtualmachineconfiguration) havuzlarına geçirilmesi gerekir.


## <a name="use-cases-for-low-priority-vms"></a>Düşük öncelikli VM'ler için kılıfları kullanma

Düşük öncelikli VM'lerin özellikleri göz önüne alındığında, hangi iş yükleri bunları kullanabilir ve kullanamaz? Genel olarak, toplu iş yükleri iyi bir uyum vardır, işler birçok paralel görevlere bölünür veya ölçeklendirilip birçok VM'ye dağıtılan birçok iş vardır.

-   Azure'da fazla kapasite kullanımını en üst düzeye çıkarmak için uygun işler ölçeklendirilebilir.

-   Bazen VM'ler kullanılamayabilir veya önsene olabilir, bu da işlerin kapasitesinin azalmasına neden olur ve görev kesintisine ve yeniden çalıştırmalara neden olabilir. Bu nedenle, işlerin çalıştırmak için alabilecekleri süre içinde esnek olması gerekir.

-   Daha uzun görevleri olan işler kesintiye uğrarsa daha fazla etkilenebilir. Uzun süren görevler yürütülürken ilerlemeyi kaydetmek için denetim altlığı uygularsa, kesintinin etkisi azalır. Daha kısa yürütme sürelerine sahip görevler, kesintinin etkisi çok daha az olduğundan, düşük öncelikli VM'lerle en iyi şekilde çalışma eğilimindedir.

-   Birden fazla VM kullanan uzun süreli MPI işleri düşük öncelikli VM'ler kullanmak için uygun değildir, çünkü önceden engellenmiş bir VM tüm işin yeniden çalışmasına neden olabilir.

Düşük öncelikli VM'ler kullanmak için uygun toplu iş işleme kullanım örneklerine bazı örnekler şunlardır:

-   **Geliştirme ve test**: Özellikle büyük ölçekli çözümler geliştiriliyorsa önemli tasarruflar sağlanabilir. Her türlü test yararlanabilir, ancak büyük ölçekli yük testi ve regresyon testi harika kullanımalanlarıdır.

-   **İsteğe bağlı kapasitenin desteklenmesi**: Düşük öncelikli VM'ler düzenli özel VM'leri tamamlamak için kullanılabilir - mevcut olduğunda, işler ölçeklenebilir ve bu nedenle daha düşük maliyetle daha hızlı tamamlanabilir; kullanılamadığında, özel VM'lerin taban çizgisi kullanılabilir durumda kalır.

-   **Esnek iş yürütme süresi**: İşlerin tamamlanması gereken süre içinde esneklik varsa, kapasitedeki potansiyel düşüşler tolere edilebilir; ancak, düşük öncelikli VM işleri eklenmesi ile sık sık daha hızlı ve daha düşük bir maliyetle çalıştırın.

Toplu iş havuzları, iş yürütme süresindeki esnekliğe bağlı olarak düşük öncelikli VM'leri birkaç şekilde kullanacak şekilde yapılandırılabilir:

-   Düşük öncelikli VM'ler yalnızca havuzda kullanılabilir. Bu durumda, Toplu Iş, kullanılabilir olduğunda önceden engellenen kapasiteyi kurtarır. Yalnızca düşük öncelikli VM'ler kullanıldığından, bu yapılandırma işleri yürütmenin en ucuz yoludur.

-   Düşük öncelikli VM'ler, özel VM'lerin sabit bir taban çizgisiyle birlikte kullanılabilir. Sabit sayıda özel VM, bir işin ilerlemesini sağlamak için her zaman bir kapasite olmasını sağlar.

-   Özel ve düşük öncelikli VM'lerin dinamik bir karışımı olabilir, böylece daha ucuz düşük öncelikli VM'ler yalnızca kullanılabilir olduğunda kullanılır, ancak tam fiyatlı özel VM'ler gerektiğinde ölçeklendirilir. Bu yapılandırma, işlerin ilerlemesini sağlamak için minimum kapasite tutar.

## <a name="batch-support-for-low-priority-vms"></a>Düşük öncelikli VM'ler için toplu destek

Azure Toplu İşlem, düşük öncelikli VM'lerin tüketilmesive bunlardan yararlanmayı kolaylaştıran çeşitli özellikler sağlar:

-   Toplu iş havuzları hem özel VM'ler hem de düşük öncelikli VM'ler içerebilir. Bir havuz oluşturulduğunda veya varolan bir havuz için herhangi bir zamanda değiştirildiğinde, açık yeniden boyutlandırma işlemini veya otomatik ölçek kullanılarak her vm türünün sayısı belirtilebilir. İş ve görev gönderme, havuzdaki VM türlerine bakılmaksızın değişmeden kalabilir. Ayrıca, işleri mümkün olduğunca ucuza çalıştırmak için düşük öncelikli VM'leri tamamen kullanacak şekilde bir havuz yapılandırabilirsiniz, ancak kapasite minimum eşiğin altına düşerse, işlerin çalışmasını sağlamak için özel VM'leri döndürebilirsiniz.

-   Toplu iş havuzları otomatik olarak düşük öncelikli VM'lerin hedef sayısını arar. VM'ler önceden önlenirse, Toplu İşlem kaybedilen kapasiteyi değiştirmeye ve hedefe geri dönmeye çalışır.

-   Görevler kesildiğinde, Toplu İşlem görevleri algılar ve yeniden çalıştırmak için görevleri otomatik olarak yeniden sıralar.

-   Düşük öncelikli VM'ler, özel VM'ler için kinden farklı ayrı bir VCPU kotaya sahiptir. 
    Düşük öncelikli VM'ler için kota, özel VM'ler için kotadan daha yüksektir, çünkü düşük öncelikli VM'ler daha az maliyetlidir. Daha fazla bilgi için [Toplu hizmet kotaları ve limitlerine](batch-quota-limit.md#resource-quotas)bakın.    

> [!NOTE]
> Düşük öncelikli VM'ler şu anda [kullanıcı abonelik modunda](batch-api-basics.md#account)oluşturulan Toplu İş hesapları için desteklenmez.
>

## <a name="create-and-update-pools"></a>Havuz oluşturma ve güncelleştirme

Toplu İşlem havuzu hem özel hem de düşük öncelikli VM'ler içerebilir (işlem düğümleri olarak da adlandırılır). Hem özel hem de düşük öncelikli VM'ler için hedef işlem düğümü sayısını ayarlayabilirsiniz. Düğümlerin hedef sayısı havuzda olmasını istediğiniz VM sayısını belirtir.

Örneğin, 5 özel VM ve 20 düşük öncelikli VM hedefiolan Azure bulut hizmeti VM'lerini kullanarak bir havuz oluşturmak için:

```csharp
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: "cspool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5") // WS 2016
);
```

5 özel VM ve 20 düşük öncelikli VM hedefiolan Azure sanal makinelerini (bu durumda Linux VM'leri) kullanarak bir havuz oluşturmak için:

```csharp
ImageReference imageRef = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "16.04-LTS",
    version: "latest");

// Create the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration("batch.node.ubuntu 16.04", imageRef);

pool = batchClient.PoolOperations.CreatePool(
    poolId: "vmpool",
    targetDedicatedComputeNodes: 5,
    targetLowPriorityComputeNodes: 20,
    virtualMachineSize: "Standard_D2_v2",
    virtualMachineConfiguration: virtualMachineConfiguration);
```

Hem özel hem de düşük öncelikli VM'ler için geçerli düğüm sayısını alabilirsiniz:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Havuz düğümlerinin düğümün özel mi yoksa düşük öncelikli vm mi olduğunu gösteren bir özelliği vardır:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

Havuzdaki bir veya daha fazla düğüm önlendiğinde, havuzdaki bir liste düğümleri işlemi yine de bu düğümleri döndürür. Mevcut düşük öncelikli düğüm sayısı değişmeden kalır, ancak bu düğümlerin durumları **Önceden Belirlenmiş** duruma ayarlanmıştür. Toplu iş, yeni düğümler gibi görev yürütmeiçin kullanılabilir hale gelmeden önce değiştirme VM'leri bulmaya çalışır ve başarılı olursa düğümler **Oluşturma** ve **ardından Başlangıç** durumlarına geçer.

## <a name="scale-a-pool-containing-low-priority-vms"></a>Düşük öncelikli VM'ler içeren bir havuzu ölçeklendirin

Yalnızca özel VM'lerden oluşan havuzlarda olduğu gibi, Resize yöntemini arayarak veya otomatik ölçek kullanarak düşük öncelikli VM'ler içeren bir havuzu ölçeklendirmek mümkündür.

Havuz yeniden boyutlandırma işlemi **hedefLowPriorityNodes**değerini güncelleştiren ikinci bir isteğe bağlı parametre alır:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

Havuz otomatik ölçeklendirme formülü düşük öncelikli VM'leri aşağıdaki gibi destekler:

-   Hizmet tanımlı **değişkenin**değerini $TargetLowPriorityNodes.

-   Hizmet tanımlı değişkenin değerini **$CurrentLowPriorityNodes.**

-   Hizmet tanımlı değişken in değerini **$PreemptedNodeCount.** 
    Bu değişken, önceden belirtilmemiş düğüm sayısına bağlı olarak, önceden belirtilmemiş düğüm sayısını ölçeklendirmenize veya azaltmanıza olanak tanır.

## <a name="jobs-and-tasks"></a>İş ve görevler

İş ve görevler düşük öncelikli düğümler için çok az ek yapılandırma gerektirir; tek destek aşağıdaki gibidir:

-   Bir işin JobManagerTask özelliği yeni bir özelliğe sahiptir, **AllowLowPriorityNode**. 
    Bu özellik doğru olduğunda, iş yöneticisi görevi özel veya düşük öncelikli düğüm üzerinde zamanlanabilir. Bu özellik yanlışsa, iş yöneticisi görevi yalnızca özel bir düğüme zamanlanır.

-   Görev uygulaması için bir [ortam değişkeni](batch-compute-node-environment-variables.md) kullanılabilir, böylece düşük öncelikli veya adanmış bir düğümüzerinde çalışıp çalışmadığını belirleyebilir. Ortam değişkeni AZ_BATCH_NODE_IS_DEDICATED.

## <a name="handling-preemption"></a>Kullanım preemption

VM'ler bazen preempted olabilir; preemption gerçekleştiğinde, Toplu iş aşağıdakileri yapar:

-   Önceden saptamış VM'ler durumlarını **Preempted olarak güncelleştirmiş**durumdadır.
-   Görevler önceden engellenen düğüm VM'lerde çalışıyorsa, bu görevler yeniden sıralanır ve yeniden çalıştırılır.
-   VM etkin bir şekilde silinir ve vm'de yerel olarak depolanan tüm verilerin kaybolmasına neden olur.
-   Havuz sürekli olarak kullanılabilir düşük öncelikli düğümlerin hedef sayısına ulaşmaya çalışır. Değiştirme kapasitesi bulunduğunda, düğümler kimliklerini tutar, ancak görev planlaması için kullanılabilir olmadan önce **Oluşturma** ve **Başlangıç** durumları üzerinden yeniden başlatılır.
-   Önalım sayıları Azure portalında metrik olarak kullanılabilir.

## <a name="metrics"></a>Ölçümler

Düşük öncelikli düğümler için [Azure portalında](https://portal.azure.com) yeni ölçümler kullanılabilir. Bu ölçümler şunlardır:

- Düşük Öncelikli Düğüm Sayısı
- Düşük Öncelikli Çekirdek Sayısı 
- Preempted Düğüm Sayısı

Azure portalındaki ölçümleri görüntülemek için:

1. Portaldaki Toplu İş hesabınıza gidin ve Toplu İşlem hesabınızın ayarlarını görüntüleyin.
2. **İzleme** bölümünden **Ölçümler'i** seçin.
3. **Kullanılabilir Ölçümler** listesinden istediğiniz ölçümleri seçin.

![Düşük öncelikli düğümler için ölçümler](media/batch-low-pri-vms/low-pri-metrics.png)

## <a name="next-steps"></a>Sonraki adımlar

* Batch kullanmaya hazırlanan herkes için gerekli bilgileri içeren [Geliştiriciler için Batch özelliğine genel bakış](batch-api-basics.md) konusunu okuyun. Bu makalede havuzlar, düğümler, işler ve görevler gibi Batch hizmet kaynakları ve Batch uygulamanızı oluştururken kullanabileceğiniz birçok API özelliği hakkında daha ayrıntılı bilgi verilmektedir.
* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
* Düşük öncelikli VM'lerden Spot VM'lere geçişi planlamaya başlayın. **Bulut Hizmeti yapılandırma** havuzları ile düşük öncelikli VM kullanıyorsanız, sanal makine **yapılandırma** havuzlarına geçmeyi planlıyorsunuz.
