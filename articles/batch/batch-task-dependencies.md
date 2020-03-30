---
title: Görevleri çalıştırmak için görev bağımlılıkları oluşturma - Azure Toplu İş
description: Azure Toplu İş'te MapReduce stilini ve benzer büyük veri iş yüklerini işlemek için diğer görevlerin tamamlanmasına bağlı görevler oluşturun.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: b8d12db5-ca30-4c7d-993a-a05af9257210
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca771117e889afc8e143c4ca4626ab2d3bb4da2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022911"
---
# <a name="create-task-dependencies-to-run-tasks-that-depend-on-other-tasks"></a>Diğer görevlere bağlı görevleri çalıştırmak için görev bağımlılıkları oluşturma

Bir görevi çalıştırmak için görev bağımlılıklarını veya görev kümesini yalnızca bir üst görev tamamlandıktan sonra tanımlayabilirsiniz. Görev bağımlılıklarının yararlı olduğu bazı senaryolar şunlardır:

* Cloud'da CloudReduce stili iş yükleri.
* Veri işleme görevleri yönlendirilmiş asiklik grafik (DAG) olarak ifade edilebilen işler.
* Bir sonraki görevin başlatılaabilmesi için her görevin tamamlanması gereken ön işleme ve işleme sonrası işlemler.
* Akış aşağı görevlerin akış yukarı görevlerin çıktısına bağlı olduğu diğer herhangi bir iş.

Toplu İşlem görev bağımlılıklarında, bir veya daha fazla üst görev tamamlandıktan sonra bilgi işlem düğümlerinde yürütülmesi için zamanlanan görevler oluşturabilirsiniz. Örneğin, 3B filmin her karesini ayrı, paralel görevlerle işleyen bir iş oluşturabilirsiniz. Son görev --"birleştirme görevi"-- işlenen kareleri yalnızca tüm kareler başarıyla işlendikten sonra tam filmle birleştirir.

Varsayılan olarak, bağımlı görevler yalnızca üst görev başarıyla tamamlandıktan sonra yürütme için zamanlanır. Varsayılan davranışı geçersiz kılmak için bir bağımlılık eylemi belirtebilir ve üst görev başarısız olduğunda görevleri çalıştırabilirsiniz. Ayrıntılar için [Bağımlılık eylemleri](#dependency-actions) bölümüne bakın.  

Bire bir veya bir-çok ilişkisinde diğer görevlere bağlı görevler oluşturabilirsiniz. Ayrıca, görevin belirli bir görev dizileri aralığındaki görev grubu tamamlanmasına bağlı olduğu bir aralık bağımlılığı da oluşturabilirsiniz. Çok-çok ilişkileri oluşturmak için bu üç temel senaryoyu birleştirebilirsiniz.

## <a name="task-dependencies-with-batch-net"></a>Toplu İşlem .NET ile görev bağımlılıkları
Bu makalede, [Toplu İşlem .NET][net_msdn] kitaplığını kullanarak görev bağımlılıklarının nasıl yapılandırılabildiğini tartışıyoruz. Önce işlerinize [görev bağımlılığını](#enable-task-dependencies) nasıl etkinleştirebileceğinizi, ardından da bir görevi bağımlılıklarla nasıl [yapılandırabileceğinizi](#create-dependent-tasks)gösteririz. Üst öğe başarısız olursa, bağımlı görevleri çalıştırmak için bağımlılık eyleminin nasıl belirtilen bir yol olduğunu da açıklarız. Son olarak, Toplu İşlem'in desteklediği [bağımlılık senaryolarını](#dependency-scenarios) tartışıyoruz.

## <a name="enable-task-dependencies"></a>Görev bağımlılıklarını etkinleştirme
Toplu İşlem uygulamanızdaki görev bağımlılıklarını kullanmak için, önce işi görev bağımlılıklarını kullanacak şekilde yapılandırmanız gerekir. Batch .NET'te, [UsesTaskDependencies][net_usestaskdependencies] özelliğini `true`şu şekilde ayarlayarak [CloudJob'unuzda][net_cloudjob] etkinleştirin:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Önceki kod snippet'inde "batchClient" [BatchClient][net_batchclient] sınıfının bir örneğidir.

## <a name="create-dependent-tasks"></a>Bağımlı görevler oluşturma
Bir veya daha fazla üst görevin tamamlanmasına bağlı bir görev oluşturmak için, görevin diğer görevlere "bağlı" olduğunu belirtebilirsiniz. Toplu İşlem .NET'te [CloudTask'ı][net_cloudtask]yapılandırın. [TaskDependencies][net_taskdependencies] sınıfının bir örneği ile özelliği [bağlı:][net_dependson]

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Bu kod snippet görev kimliği "Çiçekler" ile bağımlı bir görev oluşturur. "Çiçekler" görevi "Yağmur" ve "Güneş" görevlerine bağlıdır. "Çiçekler" görevi, yalnızca "Yağmur" ve "Güneş" görevleri başarıyla tamamlandıktan sonra bir işlem düğümünde çalışacak şekilde zamanlanır.

> [!NOTE]
> Varsayılan olarak, bir görev **tamamlanmamış** durumda yken başarıyla tamamlanmış olarak kabul `0`edilir ve çıkış **kodu** . Batch .NET'te bu [bir CloudTask][net_cloudtask]anlamına gelir. [Devlet][net_taskstate] özellik `Completed` değeri ve CloudTask'S [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode][net_exitcode] özellik `0`değeri . Bunu değiştirmek için Bağımlılık [eylemleri](#dependency-actions) bölümüne bakın.
> 
> 

## <a name="dependency-scenarios"></a>Bağımlılık senaryoları
Azure Toplu İş'te kullanabileceğiniz üç temel görev bağımlılık senaryosu vardır: bire bir, bire bir ve görev kimliği aralığı bağımlılığı. Bunlar, çok-çok dördüncü bir senaryo sağlamak için birleştirilebilir.

| Senaryo&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Örnek |  |
|:---:| --- | --- |
|  [Bir-bir](#one-to-one) |*taskB* *taskA* bağlıdır <p/> *taskB,* taskA başarıyla *tamamlanana* kadar yürütme için zamanlanmayacaktır |![Diyagram: Bire bir görev bağımlılığı][1] |
|  [Bir-çok](#one-to-many) |*taskC* hem *taskA* hem de *taskB'ye* bağlıdır <p/> *taskA* ve *taskB* başarıyla *tamamlanana* kadar taskC yürütme için zamanlanmayacaktır |![Diyagram: bir-çok görev bağımlılığı][2] |
|  [Görev kimliği aralığı](#task-id-range) |*taskD* bir dizi göreve bağlıdır <p/> *taskD,* 1 ile *10* arası imiş görevler *10* başarıyla tamamlanana kadar yürütülmesi için zamanlanmayacaktır |![Diyagram: Görev id aralığı bağımlılığı][3] |

> [!TIP]
> C, D, E ve F görevlerinin her birinin A ve B görevlerine bağlı olduğu durumlar gibi **çok-çok ilişkileri** oluşturabilirsiniz. Bu, örneğin, akış aşağı görevlerinizin birden çok yukarı görevin çıktısına bağlı olduğu paralelleştirilmiş ön işleme senaryolarında yararlıdır.
> 
> Bu bölümdeki örneklerde, bağımlı bir görev yalnızca üst görevler başarıyla tamamlandıktan sonra çalışır. Bu davranış, bağımlı bir görev için varsayılan davranıştır. Varsayılan davranışı geçersiz kılmak için bir bağımlılık eylemi belirterek bir üst görev başarısız olduktan sonra bağımlı bir görev çalıştırabilirsiniz. Ayrıntılar için [Bağımlılık eylemleri](#dependency-actions) bölümüne bakın.

### <a name="one-to-one"></a>Bir-bir
Bire bir ilişkide, görev bir üst görevin başarıyla tamamlanmasına bağlıdır. Bağımlılık oluşturmak için [TaskDependencies][net_taskdependencies]için tek bir görev kimliği sağlayın. [CloudTask'ın][net_cloudtask] [DependsOn][net_dependson] özelliğini doldurduğunuzda [OnId][net_onid] statik yöntemi.

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Bir-çok
Bir-çok ilişkisinde, görev birden çok üst görevin tamamlanmasına bağlıdır. Bağımlılık oluşturmak [için, Görev Bağımlılıkları][net_taskdependencies]için görev kimlikleri bir koleksiyon sağlayın. [CloudTask'ın][net_cloudtask] [Bağlı][net_dependson] özelliğini doldurduğunuzda [OnIds][net_onids] statik yöntemi.

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
``` 

### <a name="task-id-range"></a>Görev kimliği aralığı
Bir dizi üst göreve bağımlılık ta, görev, kimlikleri bir aralık içinde olan görevlerin tamamlanmasına bağlıdır.
Bağımlılık oluşturmak için, [görev Bağımlılıkları][net_taskdependencies]aralığındaki ilk ve son görev kimliklerini sağlayın. [CloudTask'ın][net_cloudtask] [DependsOn][net_dependson] özelliğini doldurduğunuzda [OnIdRange][net_onidrange] statik yöntemi.

> [!IMPORTANT]
> Bağımlılıklarınız için görev kimliği aralıklarını kullandığınızda, yalnızca toplam değerleri temsil eden kimlikleri olan görevler aralık tarafından seçilir. Yani aralık `1..10` görevleri `3` seçecek `7`ve `5flamingoes`, ama değil . 
> 
> Satır aralığı bağımlılıkları değerlendirirken önemli değildir, bu nedenle `4`dize tanımlayıcıları `04` `004` ile görevler , ve tüm aralığı *içinde* `4`olacak ve hepsi görev olarak kabul edilecektir , bu yüzden tamamlamak için ilk bir bağımlılık tatmin edecek.
> 
> Aralıktaki her görev, başarılı bir şekilde tamamlayarak veya **Karşıdan**Önce Ayarlanmış bir bağımlılık eylemine eşlenen bir hatayla tamamlayarak bağımlılığı karşılamalıdır. Ayrıntılar için [Bağımlılık eylemleri](#dependency-actions) bölümüne bakın.
>
>

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a>Bağımlılık eylemleri

Varsayılan olarak, bağımlı bir görev veya görev kümesi yalnızca bir üst görev başarıyla tamamlandıktan sonra çalışır. Bazı senaryolarda, ana görev başarısız olsa bile bağımlı görevleri çalıştırmak isteyebilirsiniz. Bağımlılık eylemi belirterek varsayılan davranışı geçersiz kılabilirsiniz. Bağımlılık eylemi, ana görevin başarısına veya başarısızlığına bağlı olarak bağımlı bir görevin çalıştırılabıp çalıştırılamayacağını belirtir. 

Örneğin, bağımlı bir görevin akış yukarı görevin tamamlanmasından elde edilen verileri beklediğini varsayalım. Akış üstü görev başarısız olursa, bağımlı görev yine de eski verileri kullanarak çalıştırmak mümkün olabilir. Bu durumda, bir bağımlılık eylemi, bağlı görevin üst görevin başarısızlığına rağmen çalıştırılayauygun olduğunu belirtebilir.

Bağımlılık eylemi, üst görev için bir çıkış koşuluna dayanır. Aşağıdaki çıkış koşullarından herhangi biri için bir bağımlılık eylemi belirtebilirsiniz; .NET için ayrıntılar için [ExitConditions][net_exitconditions] sınıfına bakın:

- Ön işleme hatası oluştuğunda.
- Dosya yükleme hatası oluştuğunda. Görev **exitCodes** veya **exitCodeRanges**ile belirtilen bir çıkış kodu ile çıkar ve sonra bir dosya yükleme hatası karşılaşırsa, çıkış kodu tarafından belirtilen eylem önceliklidir.
- Görev **ExitCodes** özelliği tarafından tanımlanan bir çıkış kodu yla çıktığında.
- Görev **ExitCodeRanges** özelliği tarafından belirtilen bir aralık içinde kalan bir çıkış kodu ile çıkar.
- Varsayılan durum, görev **ExitCodes** veya **ExitCodeRanges**tarafından tanımlanmamış bir çıkış kodu ile çıkarsa veya görev ön işlem hatasıyla çıkarsa ve **İşlem Öncesi Hata** özelliği ayarlanmamışsa veya görev dosya yükleme hatasıyla başarısız olursa ve **FileUploadError** özelliği ayarlanmamışsa. 

.NET'te bağımlılık eylemi belirtmek için [ExitOptions'ı][net_exitoptions]ayarlayın. Çıkış durumu için [BağımlılıkEylem][net_dependencyaction] özelliği. **Bağımlılık Eylem** özelliği iki değerden birini alır:

- **BağımlılıkEylem** özelliğini **Karşıla** olarak ayarlamak, ana görev belirtilen bir hatayla çıkarsa bağımlı görevlerin çalıştırılabılanın uygun olduğunu gösterir.
- **BağımlılıkEylem** özelliğini **Engelleme** olarak ayarlamak, bağımlı görevlerin çalıştırılamaydığını gösterir.

**Bağımlılık Eylem** özelliği için varsayılan ayar çıkış kodu 0 için **karşılayın** ve diğer tüm çıkış koşulları için **Engelleme'dir.**

Aşağıdaki kod snippet bir üst görev için **Bağımlılık Eylem** özelliğini ayarlar. Üst görev ön işleme hatasıyla veya belirtilen hata kodlarıyla çıkarsa, bağımlı görev engellenir. Üst görev başka bir sıfır olmayan hatayla çıkarsa, bağımlı görev çalıştırılabilir.

```csharp
// Task A is the parent task.
new CloudTask("A", "cmd.exe /c echo A")
{
    // Specify exit conditions for task A and their dependency actions.
    ExitConditions = new ExitConditions
    {
        // If task A exits with a pre-processing error, block any downstream tasks (in this example, task B).
        PreProcessingError = new ExitOptions
        {
            DependencyAction = DependencyAction.Block
        },
        // If task A exits with the specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible to run 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible to run depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a>Kod örneği
[TaskDependencies][github_taskdependencies] örnek projesi, GitHub'daki [Azure Toplu İş kodu örneklerinden][github_samples] biridir. Bu Visual Studio çözümü şunları göstermektedir:

- Bir işe görev bağımlılığını etkinleştirme
- Diğer görevlere bağlı görevler oluşturma
- Bu görevleri işlem düğümleri havuzunda yürütmek için nasıl.

## <a name="next-steps"></a>Sonraki adımlar
### <a name="application-deployment"></a>Uygulama dağıtımı
Toplu İşlem'in [uygulama paketleri](batch-application-packages.md) özelliği, görevlerinizin bilgi işlem düğümlerinde yürüttüğü uygulamaları hem dağıtmak hem de sürüm altına almak için kolay bir yol sağlar.

### <a name="installing-applications-and-staging-data"></a>Uygulamaların yüklenmesi ve verilerin evreleme
Bkz. Görevleri çalıştırmak için düğümlerinizi hazırlama yöntemlerine genel bir bakış için Azure Toplu İşlem forumundaki [Toplu işlem düğümlerine uygulama yükleme ve hazırlama verilerini][forum_post] görün. Azure Toplu İşlem ekibi üyelerinden biri tarafından yazılan bu yazı, uygulamaları, görev giriş verilerini ve diğer dosyaları bilgi işlem düğümlerinize kopyalamanın farklı yolları hakkında iyi bir astardır.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_exitconditions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitconditions
[net_exitoptions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions
[net_dependencyaction]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diyagram: bire bir bağımlılık"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diyagram: bir-çok bağımlılık"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diyagram: görev id aralığı bağımlılığı"
