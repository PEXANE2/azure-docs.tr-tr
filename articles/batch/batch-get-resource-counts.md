---
title: Görevler ve düğümler için durumları sayma - Azure Toplu İş | Microsoft Dokümanlar
description: Toplu İşlem çözümlerini yönetmeye ve izlemeye yardımcı olmak için Azure Toplu Iş görevlerinin durumunu ve düğümleri hesaplayın.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: article
ms.date: 09/07/2018
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: a7b58e96918d26851812aa96c18043121c081e94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023931"
---
# <a name="monitor-batch-solutions-by-counting-tasks-and-nodes-by-state"></a>Görevleri ve düğümleri duruma göre sayarak Toplu iş çözümlerini izleme

Büyük ölçekli Azure Toplu İş çözümlerini izlemek ve yönetmek için, çeşitli eyaletlerdedoğru kaynak sayımlarına ihtiyacınız vardır. Azure Toplu İşlem, *Toplu* Iş ve *işlem düğümleri*için bu sayıları almak için verimli işlemler sağlar. Büyük görev veya düğüm koleksiyonları hakkında ayrıntılı bilgi döndüren olası zaman alan liste sorguları yerine bu işlemleri kullanın.

* [Görev Sayımlarını Al,][rest_get_task_counts] bir işte etkin, çalışan ve tamamlanan görevlerin ve başarılı veya başarısız olan görevlerin toplam sayısını alır. 

  Her durumdaki görevleri sayarak, iş ilerlemesini bir kullanıcıya daha kolay görüntüleyebilir veya işi etkileyebilecek beklenmeyen gecikmeleri veya hataları algılayabilirsiniz. Toplu Hizmet API sürümü 2017-06-01.5.1 ve ilgili SDK'lar ve araçlar dan itibaren Görev Sayımları Alın'da kullanılabilir.

* [Liste Havuzu Düğümü Sayıları,][rest_get_node_counts] her havuzda çeşitli durumlarda bulunan özel ve düşük öncelikli işlem düğümlerinin sayısını alır: oluşturma, boşta, çevrimdışı, önceden önlenmiş, yeniden başlatma, yeniden görüntüleme, başlatma ve diğerleri. 

  Her durumdaki düğümleri sayarak, işinizi çalıştırmak için yeterli işlem kaynağına ne zaman sahip olduğunuzu belirleyebilir ve havuzlarınızla ilgili olası sorunları belirleyebilirsiniz. Liste Havuzu Düğümü Sayıları Toplu Servis API sürümü 2018-03-01.6.1 ve ilgili SDK'lar ve araçlar itibariyle kullanılabilir.

Hizmetin görev sayısı veya düğüm sayımı işlemlerini desteklemeyen bir sürümünü kullanıyorsanız, bu kaynakları saymak için bunun yerine bir liste sorgusu kullanın. Ayrıca, uygulamalar, dosyalar ve işler gibi diğer Toplu Iş kaynakları hakkında bilgi almak için bir liste sorgusu kullanın. Liste sorgularına filtre uygulama hakkında daha fazla bilgi için [bkz.](batch-efficient-list-queries.md)

## <a name="task-state-counts"></a>Görev durumu sayar

Görev Sayımlarını Al işlemi görevleri aşağıdaki durumlara göre sayar:

- **Etkin** - Sıraya girebilen ve çalıştırılabilen, ancak şu anda bir işlem düğümüne atanmamış bir görev. Görev, henüz `active` tamamlanmamış [bir üst göreve bağlıysa](batch-task-dependencies.md) da görevdir. 
- **Çalışan** - Bir işlem düğümüne atanmış, ancak henüz tamamlanmamış bir görev. Bir görev, `running` durumu veya `running`görev `preparing` çalışması hakkında bilgi [al'da][rest_get_task] belirtildiği gibi, durumu olduğunda olarak sayılır.
- **Tamamlandı** - Başarılı bir şekilde tamamlandığı veya başarısız olarak tamamlandığı ve yeniden deneme sınırını da tükettikleri için artık çalıştırılamaya uygun olmayan bir görev. 
- **Başarılı** - Görev yürütme sonucu . `success` Toplu iş, bir görevin başarılmış mı `TaskExecutionResult` yoksa başarısız mı olduğunu [executionInfo][rest_get_exec_info] özelliğinin özelliğini denetleyerek belirler.
- **Başarısız oldu** Görev yürütme sonucu . `failure`

Aşağıdaki .NET kod örneği, görev sayımlarını duruma göre nasıl alındırır gösterilmektedir: 

```csharp
var taskCounts = await batchClient.JobOperations.GetJobTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
```

Bir iş için görev sayımları almak için REST ve diğer desteklenen diller için benzer bir desen kullanabilirsiniz. 

> [!NOTE]
> 2018-08-01.7.0'den önceki Toplu Hizmet `validationStatus` API sürümleri de Görev Sayımları Al yanıtında bir özelliği döndürer. Bu özellik, Toplu İşlem'in durum sayar ve Liste Görevleri API'sinde bildirilen durumlarla tutarlılık sayıp sayılmadığını gösterir. Bir değer, yalnızca Toplu İşlem'in iş için en az bir kez tutarlılık için denetlendiğini `validated` gösterir. `validationStatus` Özelliğin değeri, Görev Sayımlarını Al'ın iade ettiği sayımların şu anda güncel olup olmadığını göstermez.
>

## <a name="node-state-counts"></a>Düğüm durumu sayar

Liste Havuzu Düğümü Sayıları işlemi, her havuzda aşağıdaki durumlara göre işlem düğümlerini sayar. Her havuzda özel düğümler ve düşük öncelikli düğümler için ayrı toplam sayımlar sağlanır.

- **Oluşturma** - Henüz bir havuza katılmaya başlamamış, Azure'a ayrılmış bir VM.
- **Boşta** - Şu anda bir görev çalıştıran kullanılabilir bir işlem düğümü.
- **LeavingPool** - Kullanıcı açıkça kaldırıldığı ndan veya havuz yeniden boyutlandırma veya otomatik küçültme nedeniyle havuzdan ayrılan bir düğüm.
- **Çevrimdışı** - Toplu İşlem'in yeni görevleri zamanlamak için kullanamadığı bir düğüm.
- **Önceden belirtilmeden** - Azure VM'yi geri aldığından havuzdan kaldırılan düşük öncelikli bir düğüm. Değiştirme `preempted` düşük öncelikli VM kapasitesi kullanılabilir olduğunda bir düğüm yeniden başlatılabilir.
- **Yeniden başlatma** - Yeniden başlatılan bir düğüm.
- **Yeniden görüntüleme** - İşletim sisteminin yeniden yüklendiği bir düğüm.
- **Çalışan** - Bir veya daha fazla görevi çalıştıran bir düğüm (başlangıç görevi dışında).
- **Başlangıç** - Toplu İşlem hizmetinin başladığı düğüm. 
- **StartTaskFailed** - [Başlangıç görevinin][rest_start_task] başarısız olduğu ve tüm yeniden denemeleri `waitForSuccess` tüketdiği ve başlangıç görevinde ayarlandığı bir düğüm. Düğüm görevleri çalıştırmak için kullanılabilir değildir.
- **Bilinmiyor** - Toplu İşlem hizmetiyle teması kaybeden ve durumu bilinmeyen bir düğüm.
- **Kullanılamaz** - Hatalar nedeniyle görev yürütme için kullanılabilen bir düğüm.
- **WaitingForStartTask** - Başlangıç görevinin çalışmaya başladığı, `waitForSuccess` ancak ayarlandığı ve başlangıç görevinin tamamlanmadığı bir düğüm.

Aşağıdaki C# snippet, geçerli hesaptaki tüm havuzlar için düğüm sayılarının nasıl listelendiğini gösterir:

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts())
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```
Aşağıdaki C# snippet, geçerli hesaptaki belirli bir havuz için düğüm sayılarının nasıl listelendiğini gösterir.

```csharp
foreach (var nodeCounts in batchClient.PoolOperations.ListPoolNodeCounts(new ODATADetailLevel(filterClause: "poolId eq 'testpool'")))
{
    Console.WriteLine("Pool Id: {0}", nodeCounts.PoolId);

    Console.WriteLine("Total dedicated node count: {0}", nodeCounts.Dedicated.Total);

    // Get dedicated node counts in Idle and Offline states; you can get additional states.
    Console.WriteLine("Dedicated node count in Idle state: {0}", nodeCounts.Dedicated.Idle);
    Console.WriteLine("Dedicated node count in Offline state: {0}", nodeCounts.Dedicated.Offline);

    Console.WriteLine("Total low priority node count: {0}", nodeCounts.LowPriority.Total);

    // Get low-priority node counts in Running and Preempted states; you can get additional states.
    Console.WriteLine("Low-priority node count in Running state: {0}", nodeCounts.LowPriority.Running);
    Console.WriteLine("Low-priority node count in Preempted state: {0}", nodeCounts.LowPriority.Preempted);
}
```
Havuzlar için düğüm sayıları almak için REST ve diğer desteklenen diller için benzer bir desen kullanabilirsiniz.
 
## <a name="next-steps"></a>Sonraki adımlar

* Batch hizmeti kavramları ve özellikler hakkında daha fazla bilgi edinmek için bkz. [Batch özelliklerine genel bakışı](batch-api-basics.md). Makalede, havuzlar, bilgi işlem düğümleri, işler ve görevler gibi birincil Toplu Iş kaynakları açıklanmıştır ve hizmetin özelliklerine genel bir bakış sağlar.

* Toplu iş kaynaklarını listeleyen sorgulara filtre uygulama hakkında bilgi [için](batch-efficient-list-queries.md)bkz.


[rest_get_task_counts]: /rest/api/batchservice/job/gettaskcounts
[rest_get_node_counts]: /rest/api/batchservice/account/listpoolnodecounts
[rest_get_task]: /rest/api/batchservice/task/get
[rest_list_tasks]: /rest/api/batchservice/task/list
[rest_get_exec_info]: /rest/api/batchservice/task/get
[rest_start_task]: /rest/api/batchservice/pool/add#starttask

