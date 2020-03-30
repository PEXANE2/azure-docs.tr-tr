---
title: İşlem kaynaklarını en iyi duruma getirmek için görevleri paralel olarak çalıştırma - Azure Toplu İş
description: Azure Toplu İşlem havuzunda daha az işlem düğümü kullanarak ve her düğümde eşzamanlı görevler çalıştırarak verimliliği artırın ve maliyetleri düşürün
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 538a067c-1f6e-44eb-a92b-8d51c33d3e1a
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/17/2019
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5465771cb97ef9d8d5c451a6bafc61c4621d3c4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023642"
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>Toplu işlem düğümlerinin kullanımını en üst düzeye çıkarmak için görevleri aynı anda çalıştırın 

Azure Toplu İş havuzunuzdaki her bir bilgi işlem düğümünde aynı anda birden fazla görev çalıştırarak, havuzdaki daha az sayıda düğümde kaynak kullanımını en üst düzeye çıkarabilirsiniz. Bazı iş yükleri için bu, daha kısa iş sürelerine ve daha düşük maliyete neden olabilir.

Bazı senaryolar düğümün tüm kaynaklarını tek bir göreve ayırmanın avantajını kullansa da, birden çok görevin bu kaynakları paylaşmasına izin veren birkaç durum yararlanır:

* Görevler veri paylaşabildiği zaman **veri aktarımınen en aza indirilmesi.** Bu senaryoda, paylaşılan verileri daha az sayıda düğüme kopyalayarak ve her düğüme paralel olarak görevleri yürüterek veri aktarım ücretlerini önemli ölçüde azaltabilirsiniz. Bu, özellikle her düğüme kopyalanacak verilerin coğrafi bölgeler arasında aktarılması gerekiyorsa geçerlidir.
* Görevler büyük miktarda bellek gerektirdiğinde, ancak yalnızca kısa sürelerde ve yürütme sırasında değişken zamanlarda **bellek kullanımını en üst düzeye** çıkarmak. Bu tür ani artışlarla verimli bir şekilde başa çıkmak için daha az, ancak daha büyük, daha büyük hesaplama düğümleri çalıştırabilirsiniz. Bu düğümler her düğümüzerinde paralel olarak çalışan birden çok görev olurdu, ancak her görev farklı zamanlarda düğümlerin bol belleğinden yararlanır.
* Bir havuz içinde düğümarası iletişim gerektiğinde **düğüm numarası limitlerinin azaltılması.** Şu anda, düğümler arası iletişim için yapılandırılan havuzlar 50 işlem düğümüyle sınırlıdır. Böyle bir havuzdaki her düğüm görevleri paralel olarak yürütebiliyorsa, aynı anda daha fazla sayıda görev yürütülebilir.
* **Bir işlem kümesini**(örneğin, bir işlem ortamını Azure'a ilk taşıdığınızda) çoğaltma. Geçerli şirket içi çözümünüz bilgi işlem düğümü başına birden çok görev yürütürse, bu yapılandırmayı daha yakından yansıtmak için en fazla düğüm görevi sayısını artırabilirsiniz.

## <a name="example-scenario"></a>Örnek senaryo
Paralel görev yürütmenin yararlarını göstermek için örnek olarak, görev uygulamanızın CPU ve [\_Standart D1](../cloud-services/cloud-services-sizes-specs.md) düğümlerinin yeterli olması gibi bellek gereksinimleri ne kadar olduğunu söyleyebiliriz. Ancak, işi gerekli zamanda bitirmek için, bu düğümlerden 1.000'ine ihtiyaç vardır.

1 CPU\_çekirdeğiolan Standart D1 düğümlerini kullanmak yerine, her biri 16 çekirdekli [Standart\_D14](../cloud-services/cloud-services-sizes-specs.md) düğümlerini kullanabilir ve paralel görev yürütülmesini etkinleştirebilirsiniz. Bu nedenle, 1.000 düğüm yerine *16 kat daha az düğüm* kullanılabilir, yalnızca 63 gerekir. Ayrıca, her düğüm için büyük uygulama dosyaları veya başvuru verileri gerekiyorsa, veriler yalnızca 63 düğüme kopyalandığından iş süresi ve verimlilik yeniden iyileşir.

## <a name="enable-parallel-task-execution"></a>Paralel görev yürütmeyi etkinleştirme
Havuz düzeyinde paralel görev yürütme için işlem düğümleri yapılandırırsınız. Toplu İşlem .NET kitaplığı yla, bir havuz oluşturduğunuzda [CloudPool.MaxTasksPerComputeNode][maxtasks_net] özelliğini ayarlayın. Toplu REST API kullanıyorsanız, havuz oluşturma sırasında istek gövdesindeki [maxTasksPerNode][rest_addpool] öğesini ayarlayın.

Azure Toplu İşlem, temel düğüm sayısına (4 x) kadar düğüm başına görevleri ayarlamanızı sağlar. Örneğin, havuz "Büyük" (dört çekirdek) boyutudüğümleriyle yapılandırılırsa, `maxTasksPerNode` 16 olarak ayarlanabilir. Ancak, düğümün kaç çekirdeği olursa olsun, düğüm başına 256'dan fazla görev olamaz. Düğüm boyutlarının her biri için çekirdek sayısı hakkında ayrıntılı bilgi için [Bulut Hizmetleri Için Boyutlar bölümüne](../cloud-services/cloud-services-sizes-specs.md)bakın. Hizmet sınırları hakkında daha fazla bilgi için [Azure Toplu İş hizmeti için Kotalar ve sınırlar bölümüne](batch-quota-limit.md)bakın.

> [!TIP]
> Havuzunuz için otomatik `maxTasksPerNode` [ölçeklendirme formülü][enable_autoscaling] oluştururken değeri dikkate aldığınızdan emin olun. Örneğin, değerlendiren bir formül, düğüm başına görevlerdeki artışdan önemli ölçüde `$RunningTasks` etkilenebilir. Daha fazla bilgi için [azure toplu iş havuzunda otomatik olarak hesap düğümlerini ölçeklendirin.](batch-automatic-scaling.md)
>
>

## <a name="distribution-of-tasks"></a>Görevlerin dağılımı
Bir havuzdaki işlem düğümleri görevleri aynı anda yürütebiliyorsa, görevlerin havuzdaki düğümler arasında nasıl dağıtılmasını istediğinizi belirtmeniz önemlidir.

[CloudPool.TaskSchedulingPolicy][task_schedule] özelliğini kullanarak, görevlerin havuzdaki tüm düğümlere eşit olarak atanması gerektiğini belirtebilirsiniz ("yayılma"). Veya görevler havuzdaki başka bir düğüme ("paketleme") atanmadan önce her düğüme mümkün olduğunca çok görev atanması gerektiğini belirtebilirsiniz.

Bu özelliğin nasıl değerli olduğuna bir örnek olarak, [CloudPool.MaxTasksPerComputeNode][maxtasks_net] değeri 16 ile yapılandırılan [Standart\_D14](../cloud-services/cloud-services-sizes-specs.md) düğümleri havuzunu (yukarıdaki örnekte) göz önünde bulundurun. [CloudPool.TaskSchedulingPolicy][task_schedule] [Paketinin Bir ComputeNodeFillType][fill_type] ile *Pack*yapılandırılırsa, her düğümün tüm 16 çekirdeğinin kullanımını en üst düzeye çıkarır ve [otomatik ölçeklendirme havuzunun](batch-automatic-scaling.md) havuzdan kullanılmayan düğümleri budamasını sağlar (herhangi bir görev atanmadan düğümler). Bu, kaynak kullanımını en aza indirir ve para tasarrufu sağlar.

## <a name="batch-net-example"></a>Toplu .NET örneği
Bu [Toplu İşlem .NET][api_net] API kodu snippet, düğüm başına en fazla dört görev içeren bir havuz oluşturma isteğini gösterir. Havuzdaki başka bir düğüme görev atamadan önce her düğümü görevlerle dolduracak bir görev zamanlama ilkesi belirtir. Toplu İşlem .NET API'sini kullanarak havuz ekleme hakkında daha fazla bilgi için [BatchClient.PoolOperations.CreatePool][poolcreate_net]'a bakın.

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Toplu REST örneği
Bu [Toplu REST][api_rest] API snippet düğüm başına en fazla dört görev ile iki büyük düğüm içeren bir havuz oluşturmak için bir istek gösterir. REST API'sini kullanarak havuz ekleme hakkında daha fazla bilgi için [bkz.][rest_addpool]

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicatedComputeNodes":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [!NOTE]
> Öğeyi ve `maxTasksPerNode` [MaxTasksPerComputeNode][maxtasks_net] özelliğini yalnızca havuz oluşturma zamanında ayarlayabilirsiniz. Bir havuz oluşturulduktan sonra değiştirilemezler.
>
>

## <a name="code-sample"></a>Kod örneği
GitHub'daki [ParallelNodeTasks][parallel_tasks_sample] projesi [CloudPool.MaxTasksPerComputeNode][maxtasks_net] özelliğinin kullanımını göstermektedir.

Bu C# konsolu uygulaması, bir veya daha fazla işlem düğümü içeren bir havuz oluşturmak için [Toplu İşlem .NET][api_net] kitaplığını kullanır. Değişken yükü simüle etmek için bu düğümlerde yapılandırılabilir sayıda görev yürütür. Uygulamadan çıktı, her görevi hangi düğümlerin yürüttüğünün belirtolduğunu belirtir. Uygulama ayrıca iş parametrelerinin ve süresinin bir özetini de sağlar. Örnek uygulamanın iki farklı çalışmadan çıktının özet bölümü aşağıda görünür.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Örnek uygulamanın ilk yürütülmesi, havuzda tek bir düğüm ve düğüm başına varsayılan bir görev ayarı ile iş süresinin 30 dakikadan fazla olduğunu gösterir.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Örneğin ikinci çalışması, iş süresinde önemli bir azalma gösterir. Bunun nedeni, havuzun düğüm başına dört görevle yapılandırıldırılmış olmasıdır, bu da paralel görev yürütmesinin işi yaklaşık dörtte bir sürede tamamlamasına olanak tanır.

> [!NOTE]
> Yukarıdaki özetlerde yer alan iş süreleri havuz oluşturma süresini içermez. Yukarıdaki işlerin her biri, işlem düğümleri teslim sırasında *Boşta* durumda olan daha önce oluşturulmuş havuzlara gönderilmiştir.
>
>

## <a name="next-steps"></a>Sonraki adımlar
### <a name="batch-explorer-heat-map"></a>Toplu Explorer Isı Haritası
[Toplu İşlem Gezgini,][batch_labs] Azure Toplu İş uygulamaları nın oluşturulmasına, hata ayıklanmasına ve izlenmesine yardımcı olan ücretsiz, zengin özellikli, bağımsız bir istemci aracıdır. Toplu İşlem Gezgini, görev yürütmenin görselleştirilmesini sağlayan bir *Isı Haritası* özelliği içerir. [ParallelTasks][parallel_tasks_sample] örnek uygulamasını yürütürken, her düğümdeki paralel görevlerin yürütülmesini kolayca görselleştirmek için Isı Haritası özelliğini kullanabilirsiniz.


[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchExplorer/
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

