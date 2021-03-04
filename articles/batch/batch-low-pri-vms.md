---
title: Uygun maliyetli düşük öncelikli VM 'Lerde iş yüklerini çalıştırma
description: Azure Batch iş yüklerinin maliyetini azaltmak için düşük öncelikli VM 'Ler sağlamayı öğrenin.
author: mscurrell
ms.topic: how-to
ms.date: 03/03/2021
ms.custom: seodec18
ms.openlocfilehash: cafc7216e8112640f823ecee1aea055ab78b3fc6
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102098477"
---
# <a name="use-low-priority-vms-with-batch"></a>Batch ile düşük öncelikli VM’ler kullanma

Azure Batch, toplu iş yüklerinin maliyetini azaltmak için düşük öncelikli sanal makineler (VM) sağlar. Düşük öncelikli VM 'Ler, çok düşük maliyetli bir işlem gücünün kullanılmasını sağlayarak yeni toplu Iş yükü türlerini mümkün hale getirir.

Düşük öncelikli VM 'Ler, Azure 'daki fazlalık kapasiteden yararlanır. Havuzlarınızda düşük öncelikli VM 'Ler belirttiğinizde Azure Batch bu fazlalığı kullanılabilir olduğunda kullanabilir.

Düşük öncelikli VM 'Lerin kullanılması için zorunluluğunu getirir, bu VM 'Lerin her zaman ayrılamaz veya kullanılabilir kapasiteye bağlı olarak herhangi bir zamanda yok edilebilir hale gelebilir. Bu nedenle, düşük öncelikli VM 'Ler, iş tamamlanma süresinin esnek olduğu ve çalışmanın birçok VM 'ye dağıtıldığı, toplu işlem ve zaman uyumsuz işleme iş yükleri için uygundur.

Düşük öncelikli VM 'Ler, ayrılmış VM 'lerle karşılaştırıldığında önemli ölçüde azaltılan bir fiyatla sunulur. Fiyatlandırma ayrıntıları için bkz. [Batch fiyatlandırması](https://azure.microsoft.com/pricing/details/batch/).

> [!NOTE]
> Artık [tek örnekli VM 'ler](../virtual-machines/spot-vms.md) ve [VM Ölçek Kümeleri](../virtual-machine-scale-sets/use-spot.md)için [spot VM 'ler](https://azure.microsoft.com/pricing/spot/) kullanılabilir. Spot VM 'ler düşük öncelikli VM 'lerin bir gelişmesidir, ancak bu fiyatlandırmayla farklılık gösterebilir ve spot VM 'Ler ayrılırken isteğe bağlı bir en yüksek fiyat ayarlanabilir.
>
>Azure Batch havuzları, [toplu API 'lerin ve araçların](./batch-apis-tools.md)yeni sürümleriyle gelecekte spot VM 'leri desteklemeye başlar. Spot VM desteği kullanılabilir olduktan sonra düşük öncelikli VM 'Ler kullanım dışı olacaktır; Bu işlem, sanal API 'Ler ve araç sürümleri en az 12 ay boyunca desteklenmeye devam edecektir ve bu da geçiş için VM 'Leri kullanmak için yeterli zaman sağlar.
>
> Spot VM 'Ler yalnızca sanal makine yapılandırma havuzları için desteklenecektir. Spot VM 'Leri kullanmak için, tüm bulut hizmeti yapılandırma havuzlarının [sanal makine yapılandırma havuzlarına geçirilmesi](batch-pool-cloud-service-to-virtual-machine-configuration.md)gerekir.

## <a name="batch-support-for-low-priority-vms"></a>Düşük öncelikli VM 'Ler için Batch desteği

Azure Batch, düşük öncelikli VM 'Lerin kullanımını ve avantajlarından faydalanmayı kolaylaştıran çeşitli yetenekler sağlar:

- Batch havuzları, hem adanmış VM 'ler hem de düşük öncelikli VM 'Ler içerebilir. Her bir sanal makine türünün sayısı, bir havuz oluşturulduğunda veya mevcut bir havuz için herhangi bir zamanda, açık yeniden boyutlandırma işlemi kullanılarak veya otomatik ölçeklendirmeli şekilde değiştirildiğinde belirtilebilir. İş ve görev gönderimi, havuzdaki VM türlerinden bağımsız olarak değişmeden kalabilir. Ayrıca, işleri mümkün olduğunca farklı bir şekilde çalıştırmak için düşük öncelikli VM 'Leri kullanmak için bir havuz yapılandırabilir, ancak işleri çalışır durumda tutmak için kapasite minimum eşiğin altına düşerse adanmış VM 'leri de çalıştırabilirsiniz.
- Toplu iş havuzları, düşük öncelikli VM 'lerin hedef sayısını otomatik olarak arar. VM 'Ler devre dışı veya kullanılamaz durumdaysa, yığın kayıp kapasiteyi değiştirmeyi ve hedefe geri döndürmeyi dener.
- Görevler kesintiye uğradığında, Batch tarafından yeniden çalıştırılacak görevler algılanır ve otomatik olarak requeues.
- Düşük öncelikli VM 'Ler, ayrılmış VM 'lerden farklı olan ayrı bir vCPU kotasına sahiptir. Düşük öncelikli VM 'lere yönelik kota, düşük öncelikli VM 'Lerin maliyeti daha düşük olduğundan adanmış VM 'lerin kotasından daha yüksektir. Daha fazla bilgi için bkz. [Batch hizmeti kotaları ve limitleri](batch-quota-limit.md#resource-quotas).

> [!NOTE]
> Düşük öncelikli VM 'Ler, [Kullanıcı aboneliği modunda](accounts.md)oluşturulan Batch hesapları için şu anda desteklenmiyor.

## <a name="considerations-and-use-cases"></a>Hususlar ve kullanım örnekleri

Çok sayıda Batch iş yükü düşük öncelikli VM 'Ler için uygun bir uygulamadır. İşleri birçok paralel göreve kesildiğinde veya çok sayıda sanal makineye yayılmış ve dağıtılan çok sayıda iş olduğunda kullanmayı düşünün.

Toplu işleme kullanım örneklerinin bazı örnekleri düşük öncelikli VM 'Leri kullanmak için uygundur:

- **Geliştirme ve test**: özellikle büyük ölçekli çözümler geliştiriliyorsa, önemli tasarruflar gerçekleştirilebilir. Tüm test türleri avantaj sağlayabilir, ancak büyük ölçekli yük testi ve gerileme testi harika kullanımlardır.
- **İsteğe bağlı kapasite** ekleme: düşük öncelikli VM 'ler, normal adanmış VM 'leri desteklemek için kullanılabilir. Kullanılabilir olduğunda, işler ölçeklendirebilir ve bu nedenle daha düşük maliyetli daha hızlı tamamlanır; kullanılabilir olmadığında, adanmış VM 'lerin taban çizgisi kullanılabilir kalır.
- **Esnek iş yürütme süresi**: işlerin tamamlanmaları durumunda esneklik varsa, kapasite süresi toleranslı olabilir; Bununla birlikte, düşük öncelikli VM 'Lerin eklenmesi genellikle daha hızlı ve daha düşük bir maliyetle çalışır.

Toplu iş havuzları, düşük öncelikli VM 'Leri birkaç şekilde kullanacak şekilde yapılandırılabilir:

- Bir havuz yalnızca düşük öncelikli VM 'Leri kullanabilir. Bu durumda toplu Işlem, kullanılabilir olduğunda, tüm açık kapasiteyi kurtarır. Bu yapılandırma işleri yürütme yöntemidir.
- Düşük öncelikli VM 'Ler, ayrılmış VM 'lerin sabit bir taban çizgisiyle birlikte kullanılabilir. Ayrılmış VM 'lerin sabit sayısı, bir işin devam etmesini sağlamak için her zaman bir kapasite olmasını sağlar.
- Bir havuz adanmış ve düşük öncelikli VM 'lerin dinamik bir karışımını kullanabilir, böylece, en düşük öncelikli VM 'Ler kullanılabilir olduğunda yalnızca kullanılır, ancak tam fiyatlı ayrılmış VM 'ler gerektiğinde ölçeği artırılır. Bu yapılandırma işlerin devam etmesini sağlamak için kullanılabilen minimum kapasite miktarını korur.

Düşük öncelikli VM 'lerin kullanımını planlarken aşağıdakileri göz önünde bulundurun:

- Azure 'da daha fazla kapasite kullanımını en üst düzeye çıkarmak için uygun işlerin ölçeğini değiştirebilirsiniz.
- Kimi zaman VM 'Ler kullanılamayabilir veya geçersiz hale gelebilir, bu da işler için kapasiteyi düşürür ve görev kesintiye uğramasına ve yeniden yürütülmesine neden olabilir.
- Daha kısa yürütme sürelerine sahip görevler, düşük öncelikli VM 'Ler ile en iyi şekilde çalışmaya eğilimlidir. Daha uzun görevlere sahip işler, kesintiye uğradığında daha fazla etkilenebilir. Uzun süre çalışan görevler yürütülürken ilerlemeye işaret eden denetim uygular, bu etki azalabilir. 
- Birden çok VM kullanan uzun süre çalışan MPı işleri, düşük öncelikli VM 'Leri kullanmak için uygun değildir, çünkü bir geçersiz bir VM, her zaman yeniden çalıştırmak zorunda olmasına neden olabilir.
- [Ağ güvenlik grubu (NSG) kuralları](batch-virtual-network.md#network-security-groups-specifying-subnet-level-rules) yanlış yapılandırılmışsa düşük öncelikli düğümler kullanılamaz olarak işaretlenebilir.

## <a name="create-and-manage-pools-with-low-priority-vms"></a>Düşük öncelikli VM 'lerle havuzlar oluşturun ve yönetin

Bir Batch havuzu hem adanmış hem de düşük öncelikli VM 'Leri içerebilir (işlem düğümleri olarak da adlandırılır). Hem ayrılmış hem de düşük öncelikli VM 'Ler için işlem düğümlerinin hedef sayısını ayarlayabilirsiniz. Hedef düğüm sayısı, havuzda olmasını istediğiniz sanal makinelerin sayısını belirtir.

Örneğin, Azure sanal makinelerini kullanarak (Bu durumda Linux VM 'lerde) 5 ayrılmış VM ve 20 düşük öncelikli VM 'Ler içeren bir havuz oluşturmak için:

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

Hem ayrılmış hem de düşük öncelikli VM 'Ler için geçerli düğüm sayısını alabilirsiniz:

```csharp
int? numDedicated = pool1.CurrentDedicatedComputeNodes;
int? numLowPri = pool1.CurrentLowPriorityComputeNodes;
```

Havuz düğümlerinin, düğümün adanmış veya düşük öncelikli bir VM olup olmadığını belirten bir özelliği vardır:

```csharp
bool? isNodeDedicated = poolNode.IsDedicated;
```

VM 'Ler zaman zaman değiştirilebilir. Bu durumda, önceden bırakılan düğüm VM 'lerinde çalışan görevler yeniden kuyruğa ve yeniden çalıştırılır.

Sanal makine yapılandırma havuzları için Batch Ayrıca şunları da yapar:

- Üzerinde olan VM 'Lerin durumları, yok edilmiş **olarak güncelleştirilir**. 
- VM, sanal makinede yerel olarak depolanan verilerin kaybedilmesi için etkin şekilde silinir.
- Havuzdaki bir liste düğümleri işlemi, yine de yok edilir düğümleri döndürür.
- Havuz, kullanılabilir düşük öncelikli düğümlerin hedef sayısına sürekli ulaşmaya çalışır. Değiştirme kapasitesi bulunduğunda düğümler kimliklerini tutar, ancak görev zamanlama için kullanılabilir olmadan **önce durumlar** **oluşturularak** başlatılır.
- Önalım sayıları Azure portal bir ölçüm olarak kullanılabilir.

## <a name="scale-pools-containing-low-priority-vms"></a>Düşük öncelikli VM 'Ler içeren ölçek havuzları

Yalnızca ayrılmış VM 'lerden oluşan havuzlarda olduğu gibi, yeniden boyutlandırma yöntemini çağırarak veya otomatik ölçeklendirme kullanarak düşük öncelikli VM 'Ler içeren bir havuzu ölçeklendirmek mümkündür.

Havuz yeniden boyutlandırma işlemi, **Targetlowprioritynodes** değerini güncelleştiren ikinci bir isteğe bağlı parametre alır:

```csharp
pool.Resize(targetDedicatedComputeNodes: 0, targetLowPriorityComputeNodes: 25);
```

Havuz otomatik ölçeklendirme formülü, düşük öncelikli VM 'Leri aşağıdaki gibi destekler:

- **$TargetLowPriorityNodes** hizmet tanımlı değişkenin değerini alabilir veya ayarlayabilirsiniz.
- **$CurrentLowPriorityNodes** hizmet tanımlı değişkenin değerini alabilirsiniz.
- **$PreemptedNodeCount** hizmet tanımlı değişkenin değerini alabilirsiniz. Bu değişken, yok edilen durumdaki düğüm sayısını döndürür ve kullanılmayan düğüm sayısına bağlı olarak, ayrılmış düğümlerin sayısını büyütme veya azaltma olanağı sağlar.

## <a name="configure-jobs-and-tasks"></a>İşleri ve görevleri yapılandırma

İşler ve görevler düşük öncelikli düğümler için çok daha fazla yapılandırma gerektirir. Aşağıdakileri göz önünde bulundurun:

- Bir işin JobManagerTask özelliği bir **Allowlowprioritynode** özelliğine sahiptir. Bu özellik true olduğunda, İş Yöneticisi görevi adanmış veya düşük öncelikli bir düğümde zamanlanabilir. Yanlış ise, İş Yöneticisi görevi yalnızca ayrılmış bir düğüm için zamanlanır.
- AZ_BATCH_NODE_IS_DEDICATED [ortam değişkeni](batch-compute-node-environment-variables.md) , bir görev uygulaması tarafından, düşük öncelikli veya ayrılmış bir düğümde çalışıp çalışmadığını belirleyebilmesi için kullanılabilir.

## <a name="view-metrics-for-low-priority-vms"></a>Düşük öncelikli VM 'Ler için ölçümleri görüntüleme

Yeni ölçümler, düşük öncelikli düğümler için [Azure Portal](https://portal.azure.com) kullanılabilir. Bu ölçümler şunlardır:

- Low-Priority düğüm sayısı
- Low-Priority çekirdek sayısı
- Önden düğüm sayısı

Azure portal bu ölçümleri görüntülemek için

1. Azure portalında Batch hesabınıza gidin.
2. **İzleme** bölümünden **ölçümler** ' i seçin.
3. **Ölçüm** listesinden istediğiniz ölçümleri seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Batch hizmeti iş akışı ve](batch-service-workflow-features.md) havuzlar, düğümler, işler ve görevler gibi birincil kaynaklar hakkında bilgi edinin.
- Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
- Düşük öncelikli VM 'lerden sanal makinelere kadar geçiş yapmak için başlatın. **Bulut hizmeti yapılandırma** havuzlarıyla düşük öncelikli VM 'ler kullanıyorsanız bunun yerine [ **sanal makine yapılandırma** havuzlarına](nodes-and-pools.md#configurations) geçiş yapın.
