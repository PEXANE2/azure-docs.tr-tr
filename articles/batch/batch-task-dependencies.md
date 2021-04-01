---
title: Görevleri çalıştırmak için görev bağımlılıkları oluşturma
description: Azure Batch, MapReduce stilini ve benzer büyük veri iş yüklerini işlemeye yönelik diğer görevlerin tamamlanmasına bağlı görevler oluşturun.
ms.topic: how-to
ms.date: 12/28/2020
ms.custom: H1Hack27Feb2017, devx-track-csharp
ms.openlocfilehash: ef05a98fffc3c0684ad0fa29f2f9f039b388f5ad
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97803944"
---
# <a name="create-task-dependencies-to-run-tasks-that-depend-on-other-tasks"></a>Diğer görevlere bağımlı görevleri çalıştırmak için görev bağımlılıkları oluşturma

Batch görevi bağımlılıklarıyla, bir veya daha fazla üst görevin tamamlanmasından sonra işlem düğümlerinde yürütülmek üzere zamanlanan görevler oluşturursunuz. Örneğin, her bir 3B filmin çerçevesini ayrı ve paralel görevlerle işleyen bir iş oluşturabilirsiniz. Son görev--"birleştirme görevi"--tüm çerçeveler başarıyla işlendikten sonra, işlenen kareleri tam filmle birleştirir.

Görev bağımlılıklarının yararlı olduğu bazı senaryolar şunlardır:

- Buluttaki MapReduce stili iş yükleri.
- Veri işleme görevleri, yönlendirilmiş bir çevrimsiz grafik (DAG) olarak ifade edilebilir işler.
- Sonraki görev başlamadan önce her görevin tamamlaması gereken, önceden işleme ve işleme sonrası süreçler.
- Aşağı akış görevlerinin yukarı akış görevlerinin çıktısına bağlı olduğu diğer işler.

Varsayılan olarak, bağımlı görevler yalnızca üst görev başarıyla tamamlandıktan sonra yürütme için zamanlanır. İsteğe bağlı olarak, varsayılan davranışı geçersiz kılmak için bir [bağımlılık eylemi](#dependency-actions)  belirtebilir ve üst görev başarısız olduğunda görevleri çalıştırabilirsiniz.

## <a name="task-dependencies-with-batch-net"></a>Batch .NET ile görev bağımlılıkları

Bu makalede, [Batch .net](/dotnet/api/microsoft.azure.batch) kitaplığı 'nı kullanarak görev bağımlılıklarını yapılandırmayı tartıştık. Önce, işlerinizde [görev bağımlılığını nasıl etkinleştireceğinizi](#enable-task-dependencies) ve ardından [bağımlılıklara sahip bir görevin nasıl yapılandırılacağını](#create-dependent-tasks)gösterir. Ayrıca, üst öğe başarısız olursa bağımlı görevleri çalıştırmak için bir bağımlılık eyleminin nasıl belirtildüğüne de açıklıyoruz. Son olarak, Batch 'nin desteklediği [bağımlılık senaryolarını](#dependency-scenarios) tartıştık.

## <a name="enable-task-dependencies"></a>Görev bağımlılıklarını etkinleştir

Batch uygulamanızda görev bağımlılıklarını kullanmak için, önce görevi görev bağımlılıklarını kullanacak şekilde yapılandırmanız gerekir. Batch .NET sürümünde, [Usestaskdependencies](/dotnet/api/microsoft.azure.batch.cloudjob.usestaskdependencies) özelliğini olarak ayarlayarak [cloudişiniz](/dotnet/api/microsoft.azure.batch.cloudjob) üzerinde etkinleştirin `true` :

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Yukarıdaki kod parçacığında, "batchClient" [batchclient](/dotnet/api/microsoft.azure.batch.batchclient) sınıfının bir örneğidir.

## <a name="create-dependent-tasks"></a>Bağımlı görevler oluşturma

Bir veya daha fazla üst görevin tamamlanmasına bağlı bir görev oluşturmak için, görevin diğer görevlere "bağlı" olduğunu belirtebilirsiniz. Batch .NET 'te [Cloudtask. Bağımlıdson](/dotnet/api/microsoft.azure.batch.cloudtask.dependson) özelliğini [taskdependencies](/dotnet/api/microsoft.azure.batch.taskdependencies) sınıfının bir örneğiyle yapılandırın:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Bu kod parçacığı, görev KIMLIĞI "çiçekler" olan bağımlı bir görev oluşturur. "Çiçekler" görevi "yağmur" ve "Sun" görevlerine bağlıdır. Görev "çiçekler", yalnızca "yağmur" ve "Sun" görevleri başarıyla tamamlandıktan sonra bir işlem düğümünde çalışacak şekilde zamanlanır.

> [!NOTE]
> Varsayılan olarak, bir görevin tamamlandı durumunda olduğu ve çıkış kodu olduğu zaman başarıyla tamamlandı olarak kabul edilir `0` . Batch .NET 'te bu, [cloudtask. State](/dotnet/api/microsoft.azure.batch.cloudtask.state) Özellik değerinin olduğu `Completed` ve Cloudtask 'ın [Taskexecutionınformation. ExitCode](/dotnet/api/microsoft.azure.batch.taskexecutioninformation.exitcode) Özellik değerinin olduğu anlamına gelir `0` . Bunun nasıl değiştirileceğini öğrenmek için [bağımlılık eylemleri](#dependency-actions) bölümüne bakın.

## <a name="dependency-scenarios"></a>Bağımlılık senaryoları

Azure Batch kullanabileceğiniz üç temel görev bağımlılığı senaryosu vardır: bire bir, bire çok ve görev KIMLIĞI aralığı bağımlılığı. Bu üç senaryo, dördüncü senaryo sağlamak için birleştirilebilir: çoktan çoğa.

| Senaryon&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Örnek | Göstermektedir |
|:---:| --- | --- |
|  [Bir-bir](#one-to-one) |*Taskb* , *Taska* 'a bağımlıdır <p/> *Taskb* , *tasü* başarıyla tamamlanana kadar yürütülmek üzere zamanlanmayacak |:::image type="content" source="media/batch-task-dependency/01_one_to_one.png" alt-text="Bire bir görev bağımlılığı senaryosunu gösteren diyagram."::: |
|  [Bire çok](#one-to-many) |*Taskc* , hem *Taska* ve *taskb* 'ye bağlıdır <p/> *Taskc* , hem *Taska* hem de *taskb* başarıyla tamamlanana kadar yürütülmek üzere zamanlanmayacak |:::image type="content" source="media/batch-task-dependency/02_one_to_many.png" alt-text="Bire çok görev bağımlılığı senaryosunu gösteren diyagram."::: |
|  [Görev KIMLIĞI aralığı](#task-id-range) |*Taskd* , bir dizi göreve bağlıdır <p/> *1* ile *10* arasındaki görevler başarıyla tamamlanana kadar *taskd* yürütme için zamanlanmayacak |:::image type="content" source="media/batch-task-dependency/03_task_id_range.png" alt-text="Görev KIMLIĞI aralığı görev bağımlılığı senaryosunu gösteren diyagram."::: |

> [!TIP]
> C, D, E ve F görevlerinin A ve B görevlerine bağlı olduğu durumlar gibi **çoktan çoğa** ilişkiler de oluşturabilirsiniz. Bu, örneğin, aşağı akış görevlerinizin birden çok yukarı akış görevinin çıktısına bağlı olduğu paralel ön işleme senaryolarında yararlıdır.
> 
> Bu bölümdeki örneklerde, bağımlı bir görev yalnızca üst görevler başarıyla tamamlandıktan sonra çalışır. Bu davranış, bağımlı bir görev için varsayılan davranıştır. Bir üst görev başarısız olduktan sonra, varsayılan davranışı geçersiz kılmak için bir [bağımlılık eylemi](#dependency-actions)  belirterek, bağımlı bir görevi çalıştırabilirsiniz.

### <a name="one-to-one"></a>Bir-bir

Bire bir ilişkide, bir görev bir üst görevin başarıyla tamamlanmasına bağlıdır. Bağımlılığı oluşturmak için, [Cloudtask. Bağımlıdson](/dotnet/api/microsoft.azure.batch.cloudtask.dependson) özelliğini doldurduğunuzda [Taskdependencies. onıd](/dotnet/api/microsoft.azure.batch.taskdependencies.onid) static YÖNTEMINE tek bir görev kimliği sağlayın.

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Bire çok

Bire çok ilişkisinde, bir görev birden çok üst görevin tamamlanmasına bağlıdır. Bağımlılığı oluşturmak için, [Cloudtask. Bağımlıdson](/dotnet/api/microsoft.azure.batch.cloudtask.dependson) özelliğini doldurduğunuzda [Taskdependencies. onıds](/dotnet/api/microsoft.azure.batch.taskdependencies.onids) statik yöntemine görev kimliklerinin bir koleksiyonunu sağlayın.

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

### <a name="task-id-range"></a>Görev KIMLIĞI aralığı

Bir üst görev aralığına bir bağımlılık içinde, bir görev, kimlikleri bir Aralık içinde olan görevlerin tamamlanmasına bağlıdır.
Bağımlılığı oluşturmak için, [Cloudtask. Bağımlıdson](/dotnet/api/microsoft.azure.batch.cloudtask.dependson) özelliğini doldurduğunuzda [taskdependencies](/dotnet/api/microsoft.azure.batch.taskdependencies.onidrange) statik yöntemine aralıktaki Ilk ve son görev kimliklerini sağlayın.

> [!IMPORTANT]
> Bağımlılıklarınız için görev KIMLIĞI aralıklarını kullandığınızda, yalnızca tamsayı değerlerini temsil eden kimlikleri olan görevler Aralık tarafından seçilir. Örneğin, Aralık `1..10` görevleri seçer `3` `7` , ancak değil `5flamingoes` .
>
> Aralık bağımlılıklarını değerlendirmek için öndeki sıfırlar önemli değildir, bu nedenle dize tanımlayıcıları olan görevler `4` ve `04` `004` hepsi aralığın *içinde* olur ve hepsi görev olarak değerlendirilir ve bu `4` nedenle, tamamlanacak ilk işlem bağımlılığı karşılar.
>
> Aralıktaki her görevin, başarıyla tamamlanarak ya da **karşılamak** üzere ayarlanmış bir [bağımlılık eylemine](#dependency-actions) eşlenen bir hata ile tamamlayarak bağımlılığı karşılaması gerekir.

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

Varsayılan olarak, bağımlı bir görev veya görev kümesi yalnızca bir üst görev başarıyla tamamlandıktan sonra çalışır. Bazı senaryolarda, üst görev başarısız olsa bile bağımlı görevleri çalıştırmak isteyebilirsiniz. Bağımlılık eylemi belirterek varsayılan davranışı geçersiz kılabilirsiniz.

Bağımlılık eylemi, ana görevin başarısı veya başarısızlığı temelinde bağımlı bir görevin çalıştırılmasına uygun olup olmadığını belirtir. Örneğin, bağımlı bir görevin, yukarı akış görevinin tamamlanmasında veri beklediğini varsayalım. Yukarı akış görevi başarısız olursa, bağımlı görev hala eski verileri kullanarak çalıştırılabilir. Bu durumda, bir bağımlılık eylemi bağımlı görevin, ana görevin başarısız olmasına rağmen çalışmaya uygun olduğunu belirtebilir.

Bağımlılık eylemi, üst görevin çıkış koşulunu temel alır. Aşağıdaki çıkış koşullarından herhangi biri için bir bağımlılık eylemi belirtebilirsiniz:

- Bir ön işleme hatası oluştuğunda.
- Bir dosya karşıya yükleme hatası oluştuğunda. Görev, **ExitCodes** veya **Exitcoderanges** ile belirtilen bir çıkış koduyla karşılaşıyorsa ve karşıya dosya yükleme hatasıyla karşılaşırsa, çıkış kodu tarafından belirtilen eylem önceliklidir.
- Görev, **ExitCodes** özelliği tarafından tanımlanan bir çıkış kodu ile çıktığında.
- Görev, **Exitcoderanges** özelliği tarafından belirtilen bir aralık dahilinde çıkış kodu ile çıktığında.
- Varsayılan durum, görev **ExitCodes** veya **Exitcoderanges** tarafından tanımlanmayan bir çıkış kodu ile çıkış yaptığında veya görev bir ön Işleme hatasıyla çıkarsa ve **PreProcessingError** özelliği ayarlanmamışsa ya da görev bir dosya yükleme hatasıyla başarısız olursa ve **fileuploaderror** özelliği ayarlanmamışsa. 

.NET için, bu koşullara ilişkin daha fazla ayrıntı için [Exitconditions](/dotnet/api/microsoft.azure.batch.exitconditions) sınıfına bakın.

.NET ' te bir bağımlılık eylemi belirtmek için, çıkış koşulunun [Exitoptions. DependencyAction](/dotnet/api/microsoft.azure.batch.exitoptions.dependencyaction) özelliğini aşağıdakilerden birine ayarlayın:

- Uygun **: üst** görev belirtilen bir hatayla sonlandırıldıysanız bağımlı görevlerin çalışmaya uygun olduğunu gösterir.
- **Engelle**: bağımlı görevlerin çalıştırmaya uygun olmadığını gösterir.

**DependencyAction** özelliği için varsayılan ayar, çıkış kodu 0 ve diğer tüm çıkış **Koşulları için** **engellenir** .

Aşağıdaki kod parçacığı, bir üst görev için **DependencyAction** özelliğini ayarlar. Üst görev bir ön işleme hatasıyla veya belirtilen hata kodlarıyla çıkış içeriyorsa, bağımlı görev engellenir. Üst görev sıfır olmayan başka bir hatayla sonlandıysanız, bağımlı görev çalışmaya uygundur.

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

GitHub 'daki [Taskdependencies](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies) örnek projesi şunları gösterir:

- Bir işte görev bağımlılığını etkinleştirme.
- Diğer görevlere bağımlı görevler oluşturma.
- Bu görevleri işlem düğümleri havuzunda yürütme.

## <a name="next-steps"></a>Sonraki adımlar

- Görevlerin işlem düğümlerinde yürüteceği uygulamaları dağıtmanın ve sürümlerinin kolay bir yolunu sağlayan Batch 'in [uygulama paketleri](batch-application-packages.md) özelliği hakkında bilgi edinin.
- [İşler ve görevler için hata denetimi](batch-job-task-error-checking.md)hakkında bilgi edinin.
