---
title: İşlem kaynaklarını iyileştirmek için görevleri paralel olarak çalıştırın
description: Daha az işlem düğümü kullanarak ve bir Azure Batch havuzundaki her düğümde eşzamanlı görevleri çalıştırarak verimliliği ve daha düşük maliyetleri artırın
ms.topic: how-to
ms.date: 10/08/2020
ms.custom: H1Hack27Feb2017, devx-track-csharp
ms.openlocfilehash: 3c3a81aa624ccc67c0f9e8ec23e5ef9b8e61c724
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91851008"
---
# <a name="run-tasks-concurrently-to-maximize-usage-of-batch-compute-nodes"></a>Toplu işlem düğümlerinin kullanımını en üst düzeye çıkarmak için görevleri eşzamanlı olarak çalıştırın

Azure Batch havuzunuzdaki her bir işlem düğümünde birden fazla görev aynı anda çalıştırılarak, havuzdaki daha az sayıda düğüm üzerinde kaynak kullanımını en üst düzeye çıkarabilirsiniz. Bazı iş yükleri için bu, daha kısa iş sürelerine ve maliyeti azaltmaya neden olabilir.

Bazı senaryolar, tüm düğümün kaynaklarını tek bir göreve eklemek avantajına karşın, birkaç durum birden fazla görevin bu kaynakları paylaşmasına izin vermenin avantajlarından yararlanır:

* Görevler veri paylaştığında **veri aktarımını en aza indirir** . Bu senaryoda, paylaşılan verileri daha az sayıda düğüme kopyalayarak ve görevleri her düğümde paralel olarak yürüterek veri aktarımı ücretlerini ciddi ölçüde azaltabilirsiniz. Bu özellikle, her düğüme kopyalanabilecek verilerin coğrafi bölgeler arasında aktarılması gerekiyorsa geçerlidir.
* Görevler büyük miktarda bellek gerektirdiğinde, ancak yalnızca kısa süreler boyunca ve yürütme sırasında değişken sürelerinde **bellek kullanımının** en üst düzeye çıkarın. Bu tür artışları verimli bir şekilde işlemek için daha fazla belleğe sahip daha az, ancak daha büyük işlem düğümleri kullanabilirsiniz. Bu düğümlerde her düğüm üzerinde paralel olarak çalışan birden çok görev olur, ancak her görev farklı zamanlarda düğümlerin plentiful belleğinden faydalanır.
* Bir havuz içinde düğümler arası iletişim gerektiğinde **düğüm sayısı sınırlarını** azaltma. Şu anda, düğümler arası iletişim için yapılandırılan havuzlar 50 işlem düğümüyle sınırlıdır. Bu tür bir havuzdaki her düğüm görevleri paralel olarak yürütebiliyor ise, daha fazla sayıda görev aynı anda yürütülebilir.
* Bir bilgi işlem ortamını Azure 'a taşırken olduğu gibi, **Şirket içi bir hesaplama kümesini çoğaltma**. Mevcut şirket içi çözümünüz işlem düğümü başına birden çok görev çalıştırırsa, bu yapılandırmayı daha yakından yansıtmak için en fazla düğüm görevi sayısını artırabilirsiniz.

## <a name="example-scenario"></a>Örnek senaryo
Paralel görev yürütmesinin avantajlarını gösteren bir örnek olarak, görev uygulamanızın [Standart \_ D1](../cloud-services/cloud-services-sizes-specs.md) düğümlerinin yeterlı olması gibi CPU ve bellek gereksinimleri olduğunu varsayalım. Ancak, işin gerekli zamanda tamamlanabilmesi için, bu düğümlerin 1.000 ' i gereklidir.

\_1 CPU çekirdeği olan standart D1 düğümlerini kullanmak yerine, her biri 16 çekirdeğe sahip [Standart \_ D14](../cloud-services/cloud-services-sizes-specs.md) düğümlerini kullanabilir ve paralel görev yürütmeyi etkinleştirebilirsiniz. Bu nedenle, 1.000 düğüm yerine *16 kez daha az düğüm* kullanılabilir, ancak yalnızca 63 gereklidir. Ayrıca, her düğüm için büyük uygulama dosyaları veya başvuru verileri gerekliyse, veriler yalnızca 63 düğümlere kopyalandığından iş süresi ve verimlilik yeniden geliştirilmiştir.

## <a name="enable-parallel-task-execution"></a>Paralel görev yürütmeyi etkinleştir
Paralel görev yürütme için işlem düğümlerini havuz düzeyinde yapılandırırsınız. Batch .NET kitaplığı ile bir havuz oluştururken [Cloudpool. TaskSlotsPerNode][maxtasks_net] özelliğini ayarlayın. Batch REST API kullanıyorsanız, havuz oluşturma sırasında istek gövdesinde [Taskslotspernode][rest_addpool] öğesini ayarlayın.

Azure Batch, düğüm başına görev yuvalarını, düğüm çekirdekleri sayısına kadar ayarlamanıza olanak sağlar. Örneğin, havuz "büyük" (dört çekirdek) boyutundaki düğümlerle yapılandırıldıysa, `taskSlotsPerNode` 16 olarak ayarlanabilir. Ancak, düğümde kaç çekirdeğin olduğuna bakılmaksızın, düğüm başına 256 taneden fazla görev yuvası kullanamazsınız. Düğüm boyutlarının her biri için çekirdek sayısı hakkında daha fazla bilgi için bkz. [Cloud Services boyutları](../cloud-services/cloud-services-sizes-specs.md). Hizmet limitleri hakkında daha fazla bilgi için bkz. [Azure Batch hizmet Için kotalar ve sınırlar](batch-quota-limit.md).

> [!TIP]
> `taskSlotsPerNode`Havuzunuz için bir [Otomatik ölçeklendirme formülü][enable_autoscaling] oluştururken değeri hesaba aldığınızdan emin olun. Örneğin, değerlendirilen bir formül, `$RunningTasks` düğüm başına görevlerin artışına göre önemli ölçüde etkilenebilir. Daha fazla bilgi için bkz. [bir Azure Batch havuzundaki işlem düğümlerini otomatik olarak ölçeklendirme](batch-automatic-scaling.md) .
>
>

> [!NOTE]
> `taskSlotsPerNode`Element ve [Taskslotspernode][maxtasks_net] özelliğini yalnızca havuz oluşturma zamanında ayarlayabilirsiniz. Havuz zaten oluşturulduktan sonra değiştirilemez.
>
>

## <a name="distribution-of-tasks"></a>Görevlerin dağıtılması
Bir havuzdaki işlem düğümleri görevleri eşzamanlı olarak yürütemediğinde, bu görevlerin havuzdaki düğümlerde nasıl dağıtılmasını istediğinizi belirtmek önemlidir.

[Cloudpool. TaskSchedulingPolicy][task_schedule] özelliğini kullanarak, bu görevlerin havuzdaki tüm düğümlerde eşit olarak atanmasını belirtebilirsiniz ("yayma"). Ya da görevler havuzdaki başka bir düğüme atanmadan önce her düğüme olabildiğince fazla görevin atanması gerektiğini belirtebilirsiniz ("paketleme").

Bu özelliğin nasıl değerli olduğu hakkında bir örnek olarak, bir [Cloudpool. TaskSlotsPerNode][maxtasks_net] değeri 16 Ile yapılandırılan [Standart \_ D14](../cloud-services/cloud-services-sizes-specs.md) düğümlerinin havuzunu (Yukarıdaki örnekte) göz önünde bulundurun. [Cloudpool. TaskSchedulingPolicy][task_schedule] , bir [Computenodefilltype][fill_type] *paketi*ile yapılandırıldıysa, her bir düğümün tüm 16 çekirdeğin kullanımını en üst düzeye çıkarabilir ve bir [Otomatik ölçeklendirme havuzunun](batch-automatic-scaling.md) kullanılmayan düğümleri havuzdan (atanmış herhangi bir görev olmadan) ayıklayabilmesini sağlar. Bu, kaynak kullanımını en aza indirir ve para tasarrufu sağlar.

## <a name="variable-slots-per-task"></a>Görev başına değişken Yuvaları
Görev, bir işlem düğümünde çalışması gereken yuva sayısını belirtmek için [cloudtask. Requiredslotlar][taskslots_net] özelliği ile tanımlanabilir ve varsayılan değer 1 ' dir. Görevleriniz, işlem düğümündeki kaynak kullanımı ile ilgili farklı ağırlıklara sahip olduğunda değişken görev yuvaları ayarlayabilirsiniz. bu nedenle, her işlem düğümü CPU veya bellek gibi sistem kaynaklarını çok fazla sayıda eşzamanlı çalışan görev olmadan alabilir.

Örneğin, özelliği olan bir havuz için `taskSlotsPerNode = 8` , ile diğer görevler sırasında ile çok çekirdekli gereklı CPU yoğun görevleri gönderebilirsiniz `requiredSlots = 8` `requiredSlots = 1` . Bu karma iş yükü havuza zamanlandığında, CPU yoğun görevler yalnızca işlem düğümünde çalışır, diğer görevler diğer düğümlerde aynı anda (sekiz göreve kadar) çalışabilir. Bu, iş yükünüzü işlem düğümleri genelinde dengelemenize ve kaynak kullanımı verimliliğini artırmaya yardımcı olur.

> [!TIP]
> Değişken görev yuvaları kullanılırken, bazı düğümlerde hala boşta duran yuvalar olsa bile, herhangi bir işlem düğümünde kullanılabilir yuva olmadığından, daha fazla gerekli yuva olan büyük görevler geçici olarak zamanlanmayabilir. Düğümlerdeki kullanılabilir yuvalara rekabet şansını artırmak için bu görevler için iş önceliği yükseltebilirsiniz.
>
> Batch hizmeti ayrıca [Taskschedulefailevent](batch-task-schedule-fail-event.md) 'i bir görevi çalıştırmak için zamanlamazsa, gerekli yuvalar kullanılabilir olana kadar zamanlamayı yeniden denemeye devam eder. Olası görev zamanlaması ile ilgili sorunu tespit etmek için bu olayı dinleyebilir ve hafifletme riski aşağıda bulabilirsiniz.
>

> [!NOTE]
> Görevin `requiredSlots` havuzdan daha büyük olması için belirtin `taskSlotsPerNode` . Bu, görevin hiçbir şekilde çalışmasına neden olur. İş, gönderme zamanında havuz bağlı olmadığından veya devre dışı bırakarak/yeniden etkinleştirerek farklı bir havuza değiştirilmediğinden, şu anda Batch hizmeti bu doğrulama Işlemini yapmaz.
>

## <a name="batch-net-example"></a>Batch .NET örneği
Aşağıdaki [Batch .net][api_net] API kod parçacıkları, düğüm başına birden çok görev yuvası içeren bir havuzun nasıl oluşturulacağını ve gerekli yuvalarla görev göndermesinin nasıl yapılacağını gösterir.

### <a name="create-pool"></a>Havuz oluştur
Bu kod parçacığı, düğüm başına dört adet görev yuvası ile dört düğüm içeren bir havuz oluşturma isteğini gösterir. Bu işlem, havuzdaki başka bir düğüme görev atamadan önce her bir düğümü görevlerle dolduracak bir görev zamanlama İlkesi belirtir. Batch .NET API 'sini kullanarak havuz ekleme hakkında daha fazla bilgi için bkz. [Batchclient. PoolOperations. CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicatedComputeNodes: 4
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

pool.TaskSlotsPerNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

### <a name="create-task-with-required-slots"></a>Gerekli yuvalarla görev oluştur
Bu kod parçacığı, varsayılan olmayan bir görev oluşturur `requiredSlots` . Bu görev yalnızca işlem düğümünde yeterli kullanılabilir boş yuva olduğunda çalışır.
```csharp
CloudTask task = new CloudTask(taskId, taskCommandLine)
{
    RequiredSlots = 2
};
```

### <a name="list-compute-nodes-with-counts-for-running-tasks-and-slots"></a>Görevleri ve yuvaları çalıştırmak için, işlem düğümlerini sayılarıyla listeleyin
Bu kod parçacığı, havuzdaki tüm işlem düğümlerini listeler ve her düğüm için çalışan görevler ve görev yuvaları sayısını yazdırır.
```csharp
ODATADetailLevel nodeDetail = new ODATADetailLevel(selectClause: "id,runningTasksCount,runningTaskSlotsCount");
IPagedEnumerable<ComputeNode> nodes = batchClient.PoolOperations.ListComputeNodes(poolId, nodeDetail);

await nodes.ForEachAsync(node =>
{
    Console.WriteLine(node.Id + " :");
    Console.WriteLine($"RunningTasks = {node.RunningTasksCount}, RunningTaskSlots = {node.RunningTaskSlotsCount}");

}).ConfigureAwait(continueOnCapturedContext: false);
```

### <a name="list-task-counts-for-the-job"></a>İş için görev sayılarını listeleyin
Bu kod parçacığı, her görev durumu için hem görevler hem de görev Yuvaları sayısı dahil olmak üzere iş için görev sayısını alır.
```csharp
TaskCountsResult result = await batchClient.JobOperations.GetJobTaskCountsAsync(jobId);

Console.WriteLine("\t\tActive\tRunning\tCompleted");
Console.WriteLine($"TaskCounts:\t{result.TaskCounts.Active}\t{result.TaskCounts.Running}\t{result.TaskCounts.Completed}");
Console.WriteLine($"TaskSlotCounts:\t{result.TaskSlotCounts.Active}\t{result.TaskSlotCounts.Running}\t{result.TaskSlotCounts.Completed}");
```

## <a name="batch-rest-example"></a>Toplu REST örneği
Bu [toplu Iş Rest][api_rest] API parçacığı, düğüm başına en fazla dört görevi olan iki büyük düğüm içeren bir havuz oluşturma isteğini gösterir. REST API kullanarak havuzlar ekleme hakkında daha fazla bilgi için bkz. bir [hesaba havuz ekleme][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  },
  "targetDedicatedComputeNodes":2,
  "taskSlotsPerNode":4,
  "enableInterNodeCommunication":true,
}
```

Bu kod parçacığında varsayılan olmayan bir görev ekleme isteği gösterilmektedir `requiredSlots` . Bu görev yalnızca işlem düğümünde yeterli kullanılabilir boş yuva olduğunda çalışır.
```json
{
  "id": "taskId",
  "commandLine": "bash -c 'echo hello'",
  "userIdentity": {
    "autoUser": {
      "scope": "task",
      "elevationLevel": "nonadmin"
    }
  },
  "requiredSLots": 2
}
```

## <a name="code-sample"></a>Kod örneği
GitHub 'daki [Parallelnodetasks][parallel_tasks_sample] projesi [Cloudpool. TaskSlotsPerNode][maxtasks_net] özelliğinin kullanımını gösterir.

Bu C# konsol uygulaması, bir veya daha fazla işlem düğümü içeren bir havuz oluşturmak için [Batch .net][api_net] kitaplığını kullanır. Değişken yükünün benzetimini yapmak için bu düğümlerde yapılandırılabilir sayıda görevi yürütür. Uygulamanın çıktısı, her bir görevi yürüten düğümleri belirtir. Uygulama, iş parametrelerinin ve sürenin bir özetini de sağlar. Örnek uygulamanın iki farklı çalıştırmasından çıktının Özet bölümü aşağıda görüntülenir.

```
Nodes: 1
Node size: large
Task slots per node: 1
Max slots per task: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Örnek uygulamanın ilk yürütülmesi, havuzda tek bir düğüm ve düğüm başına bir görev için varsayılan ayar olan iş süresinin 30 dakikadan fazla olduğunu gösterir.

```
Nodes: 1
Node size: large
Task slots per node: 4
Max slots per task: 1
Tasks: 32
Duration: 00:08:48.2423500
```

Örneğin ikinci çalıştırmasında iş süresinde önemli bir düşüş gösterilmektedir. Bunun nedeni, havuzun her düğüm için dört görev ile yapılandırılmıştı. Bu, paralel görev yürütmenin iş zamanının neredeyse bir çeyrekte tamamlanmasını sağlar.

> [!NOTE]
> Yukarıdaki özetlerdeki iş süreleri havuz oluşturma süresini içermez. Yukarıdaki işlerin her biri, işlem düğümleri gönderme zamanında *Boşta* durumunda olan önceden oluşturulmuş havuzlara gönderilmiştir.
>
>

## <a name="next-steps"></a>Sonraki adımlar
### <a name="batch-explorer-heat-map"></a>Batch Explorer ısı haritası
[Batch Explorer][batch_labs] , Azure Batch uygulamaları oluşturmaya, hata ayıklamanıza ve izlemenize yardımcı olan ücretsiz, zengin özellikli, tek başına bir istemci aracıdır. Batch Explorer, görev yürütme görselleştirmesini sağlayan bir *ısı haritası* özelliği içerir. [Paralleltasks][parallel_tasks_sample] örnek uygulamasını yürütürken, her bir düğümdeki paralel görevlerin yürütülmesini kolayca görselleştirebilmeniz Için ısı haritası özelliğini kullanabilirsiniz.


[api_net]: /dotnet/api/microsoft.azure.batch
[api_rest]: /rest/api/batchservice/
[batch_labs]: https://azure.github.io/BatchExplorer/
[cloudpool]: /dotnet/api/microsoft.azure.batch.cloudpool
[enable_autoscaling]: /rest/api/batchservice/pool/enableautoscale
[fill_type]: /dotnet/api/microsoft.azure.batch.common.computenodefilltype
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: /dotnet/api/microsoft.azure.batch.cloudpool
[rest_addpool]: /rest/api/batchservice/pool/add
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: /dotnet/api/microsoft.azure.batch.pooloperations
[task_schedule]: /dotnet/api/microsoft.azure.batch.cloudpool
[taskslots_net]: /dotnet/api/microsoft.azure.batch.cloudtask.requiredslots
