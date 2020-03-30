---
title: MPI uygulamalarını çalıştırmak için çok örnekli görevleri kullanma - Azure Toplu İş | Microsoft Dokümanlar
description: Azure Toplu İş'teki çok örnekli görev türünü kullanarak İleti Geçme Arabirimi (MPI) uygulamalarını nasıl uygulayacaklarını öğrenin.
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.date: 03/13/2019
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1896fea3c401299b4f77235ab3c02d85708b7041
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77023676"
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a>Toplu İşlemde İleti Geçme Arabirimi (MPI) uygulamalarını çalıştırmak için çok örnekli görevleri kullanma

Çok örnekli görevler, aynı anda birden çok işlem düğümünde bir Azure Toplu Iş görevi çalıştırmanıza olanak tanır. Bu görevler, Toplu İşlem'de İleti Geçme Arabirimi (MPI) uygulamaları gibi yüksek performanslı bilgi işlem senaryoları sağlar. Bu makalede, [Toplu İşlem .NET][api_net] kitaplığını kullanarak çok örnekli görevleri nasıl yürüteceklerini öğrenirsiniz.

> [!NOTE]
> Bu makaledeki örnekler Batch .NET, MS-MPI ve Windows bilgi işlem düğümlerine odaklanırken, burada tartışılan çok örnekli görev kavramları diğer platformlar ve teknolojiler için de geçerlidir (örneğin Linux düğümlerinde Python ve Intel MPI).
>
>

## <a name="multi-instance-task-overview"></a>Çok örnekli göreve genel bakış
Toplu İşlem'de, her görev normalde tek bir işlem düğümünde yürütülür ve bir işe birden çok görev gönderirsiniz ve Toplu Iş Partisi her görevi düğümüzerinde yürütülmesi için zamanlar. Ancak, bir görevin **çok örnekli ayarlarını**yapılandırarak, Toplu İşlem'e bunun yerine bir birincil görev ve daha sonra birden çok düğümde yürütülecek birkaç alt görev oluşturmasını söylersiniz.

![Çok örnekli göreve genel bakış][1]

Bir göreve çok örnek ayarları olan bir görev gönderdiğinde, Toplu İşlem çok örnekli görevlere özgü birkaç adım gerçekleştirir:

1. Toplu İşlem hizmeti, çok örnekli ayarları temel alan bir **birincil** ve birkaç **alt görev** oluşturur. Toplam görev sayısı (birincil artı tüm alt görevler) çok örnekli ayarlarda belirttiğiniz **örnek** (işlem düğümleri) sayısıyla eşleşir.
2. Toplu işlem düğümlerinden birini **ana**olarak belirtir ve asıl görevi asıl görevde yürütmek üzere zamanlar. Çok örnekli göreve ayrılan işlem düğümlerinin geri kalanında, düğüm başına bir alt görev de yürütülmesi için alt görevleri zamanlar.
3. Birincil ve tüm alt görevler, çok örnekli ayarlarda belirttiğiniz **ortak kaynak dosyalarını** indirir.
4. Ortak kaynak dosyaları indirildikten sonra, birincil ve alt görevler çok örnekli ayarlarda belirttiğiniz **koordinasyon komutunu** çalıştırın. Koordinasyon komutu genellikle görevi yürütmek için düğümleri hazırlamak için kullanılır. Bu, arka plan hizmetlerini başlatmayı [(Microsoft MPI][msmpi_msdn]'ler `smpd.exe`gibi) ve düğümlerin düğümler arası iletileri işlemeye hazır olduğunu doğrulamayı içerebilir.
5. Birincil görev, koordinasyon komutu birincil ve tüm alt görevler tarafından başarıyla tamamlandıktan *sonra* ana düğümdeki **uygulama komutunu** yürütür. Uygulama komutu, çok örnekli görevin komut satırıdır ve yalnızca birincil görev tarafından yürütülür. [MS-MPI][msmpi_msdn]tabanlı bir çözümde, MPI özellikli uygulamanızı `mpiexec.exe`kullanarak yürüttüğünüz yer dir.

> [!NOTE]
> İşlevsel olarak farklı olsa da, "çok örnekli görev" [Başlangıç Görevi][net_starttask] veya [İş Hazırlama Görevi][net_jobprep]gibi benzersiz bir görev türü değildir. Çok örnekli görev, çok örnekli ayarları yapılandırılan standart bir Toplu Iş görevidir (Toplu İşlem .NET'te[CloudTask).][net_task] Bu makalede, bu **çok örnekli görev**olarak bakın.
>
>

## <a name="requirements-for-multi-instance-tasks"></a>Çok örnekli görevler için gereksinimler
Çok örnekli görevler, **düğümler arası iletişim etkin**leştirilmiş ve **eşzamanlı görev yürütme devre dışı bırakılmış**bir havuz gerektirir. Eşzamanlı görev yürütmesini devre dışı kılabilir, [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool) özelliğini 1 olarak ayarlayın.

> [!NOTE]
> Toplu işlem, düğümler arası iletişim etkinleştirilmiş bir havuzun boyutunu [sınırlar.](batch-quota-limit.md#pool-size-limits)


Bu kod snippet Toplu Iş .NET kitaplığı kullanarak çok örnekli görevler için bir havuz oluşturmak için nasıl gösterir.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> Çok örnekli bir görevi, internode iletişimindevre dışı bırakıldığı bir havuzda çalıştırmaya çalışırsanız veya *maxTasksPerNode* değeri 1'den büyükse, görev asla zamanlanmaz- "etkin" durumda süresiz olarak kalır. 


### <a name="use-a-starttask-to-install-mpi"></a>MPI'yi yüklemek için StartTask'ı kullanma
ÇOK örnekli bir görevle MPI uygulamalarını çalıştırmak için öncelikle havuzdaki bilgi işlem düğümlerine bir MPI uygulaması (örneğin MS-MPI veya Intel MPI) yüklemeniz gerekir. Bu, düğüm bir havuza katıldığında veya yeniden başlatıldığında çalıştıran bir [Başlangıç Görevi][net_starttask]kullanmak için iyi bir zamandır. Bu kod snippet bir [kaynak dosyası][net_resourcefile]olarak MS-MPI kurulum paketi belirten bir StartTask oluşturur. Kaynak dosyası düğüme indirildikten sonra başlat görevin komut satırı yürütülür. Bu durumda, komut satırı MS-MPI gözetimsiz bir yükleme gerçekleştirir.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Uzaktan doğrudan bellek erişimi (RDMA)
Toplu Iş havuzunuzdaki bilgi işlem düğümleri için A9 gibi [RDMA özellikli](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bir boyut seçtiğinizde, MPI uygulamanız Azure'un yüksek performanslı, düşük gecikmeli uzaktan doğrudan bellek erişimi (RDMA) ağından yararlanabilir.

Aşağıdaki makalelerde "RDMA özellikli" olarak belirtilen boyutlara bakın:

* **CloudServiceConfiguration** havuzları

  * [Bulut Hizmetleri için Boyutlar](../cloud-services/cloud-services-sizes-specs.md) (yalnızca Windows)
* **VirtualMachineConfiguration** havuzları

  * [Azure'daki sanal makineler için boyutlar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)
  * [Azure'daki sanal makineler için boyutlar](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)

> [!NOTE]
> [Linux bilgi işlem düğümlerinde](batch-linux-nodes.md)RDMA'dan yararlanmak için düğümlerde **Intel MPI** kullanmanız gerekir. 
>

## <a name="create-a-multi-instance-task-with-batch-net"></a>Batch .NET ile çok örnekli bir görev oluşturma
Artık havuz gereksinimlerini ve MPI paket yüklemesini kapsadığımıza göre, çok örnekli görevi oluşturalım. Bu snippet'te standart bir [CloudTask][net_task]oluşturuyoruz ve [ardından MultiInstanceSettings][net_multiinstance_prop] özelliğini yapılandırıyoruz. Daha önce de belirtildiği gibi, çok örnekli görev ayrı bir görev türü değil, çok örnekli ayarlarla yapılandırılan standart bir Toplu Iş partisi görevidir.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Birincil görev ve alt görevler
Bir görev için çok örnekli ayarları oluşturduğunuzda, görevi yürütmek için bilgi işlem düğümleri sayısını belirtirsiniz. Görevi bir işe gönderdiğinizde, Toplu İşlem hizmeti bir **birincil** görev ve birlikte belirttiğiniz düğüm sayısıyla eşleşen yeterli **alt görev** oluşturur.

Bu görevlere 0 ile *numberOfInstances* aralığında bir bir sonda kimliği atanır - 1. id 0 ile görev birincil görevdir ve diğer tüm id'ler alt görevlerdir. Örneğin, bir görev için aşağıdaki çok örnek li ayarları oluşturursanız, birincil görevin kimliği 0 olur ve alt görevlerde 1 ile 9 arası kimlikler olur.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Ana düğüm
Çok örnekli bir görev gönderdiğinizde, Toplu İşlem hizmeti işlem düğümlerinden birini "asıl" düğüm olarak belirler ve ana düğümde yürütülecek birincil görevi zamanlar. Alt görevler, çok örnekli göreve ayrılan düğümlerin geri kalanında yürütülecek şekilde zamanlanır.

## <a name="coordination-command"></a>Koordinasyon komutu
**Koordinasyon komutu** hem birincil hem de alt görevler tarafından yürütülür.

Koordinasyon komutunun çağrılması engelliyor--Toplu işlem komutu tüm alt görevler için başarıyla döndürülene kadar uygulama komutunu yürütmez. Bu nedenle koordinasyon komutu gerekli arka plan hizmetlerini başlatmalı, kullanıma hazır olduklarını doğrulamalı ve sonra çıkmalıdır. Örneğin, MS-MPI sürüm 7'yi kullanan bir çözüm için bu koordinasyon komutu düğümdeki SMPD hizmetini başlatır ve sonra çıkar:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Bu koordinasyon `start` komutunun kullanımına dikkat edin. Uygulama yürütmeden `smpd.exe` hemen sonra geri dönmüyor, çünkü bu gereklidir. [Başlat][cmd_start] komutu kullanılmadan, bu koordinasyon komutu geri dönmez ve bu nedenle uygulama komutunun çalışmasını engeller.

## <a name="application-command"></a>Uygulama komutu
Birincil görev ve tüm alt görevler koordinasyon komutunu yürütmeyi tamamladıktan sonra, çok örnekli görevin komut satırı *yalnızca*birincil görev tarafından yürütülür. Biz buna koordinasyon komutundan ayırt etmek için **uygulama komutu** diyoruz.

MS-MPI uygulamaları için, MPI özellikli uygulamanızı `mpiexec.exe`yürütmek için uygulama komutunu kullanın. Örneğin, MS-MPI sürüm 7'yi kullanan bir çözüm için uygulama komutu aşağıda verilmiştir:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> MS-MPI varsayılan `mpiexec.exe` olarak `CCP_NODES` değişkeni kullandığından [(Çevre değişkenlerine](#environment-variables)bakın) yukarıdaki örnek uygulama komut satırı onu dışlar.
>
>

## <a name="environment-variables"></a>Ortam değişkenleri
Toplu iş, çok örnekli bir göreve ayrılan bilgi işlem düğümlerinde çok örnekli görevlere özgü birkaç [ortam değişkeni][msdn_env_var] oluşturur. Koordinasyon ve uygulama komut satırlarınız, yürüttükleri komut dosyaları ve programlar gibi bu ortam değişkenlerine de başvuruyapabilir.

Aşağıdaki ortam değişkenleri, çok örnekli görevlertarafından kullanılmak üzere Toplu İşlem hizmeti tarafından oluşturulur:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Bu ve diğer Toplu işlem düğümü ortam değişkenleri, bunların içeriği ve görünürlüğü de dahil olmak üzere tam ayrıntılar [için][msdn_env_var]bkz.

> [!TIP]
> Toplu Linux MPI kod örneği, bu ortam değişkenlerinden bazılarının nasıl kullanılabileceğinin bir örneğini içerir. [Koordinasyon cmd][coord_cmd_example] Bash komut dosyası Azure Depolama'dan ortak uygulama ve giriş dosyalarını indirir, ana düğümde Ağ Dosya Sistemi (NFS) paylaşımını sağlar ve nfs istemcileri olarak çok örnekli göreve ayrılan diğer düğümleri yapılandırır.
>
>

## <a name="resource-files"></a>Kaynak dosyalar
Çok örnekli görevler için göz önünde bulundurulması gereken iki kaynak dosyası kümesi vardır: *tüm* görevlerin karşıdan yüklediği **ortak kaynak dosyaları** (hem birincil hem de alt görevler) ve yalnızca *birincil* görevin karşıdan yüklendiği çok örnekli görevin kendisi için belirtilen **kaynak dosyaları.**

Bir görev için çok örnekli ayarlarda bir veya daha fazla **yaygın kaynak dosyası** belirtebilirsiniz. Bu yaygın kaynak dosyaları [Azure Depolama'dan](../storage/common/storage-introduction.md) her düğümün **görev dizinin** birincil ve tüm alt görevler tarafından paylaşılır içine indirilir. `AZ_BATCH_TASK_SHARED_DIR` Ortam değişkenini kullanarak uygulama ve koordinasyon komut satırlarından görev paylaşılan dizine erişebilirsiniz. Yol, `AZ_BATCH_TASK_SHARED_DIR` çok örnekli göreve ayrılan her düğümde aynıdır, böylece birincil ve tüm alt görevler arasında tek bir koordinasyon komutunu paylaşabilirsiniz. Toplu iş, dizini uzaktan erişim anlamında "paylaşmaz", ancak çevre değişkenleri hakkında daha önce ipucunda belirtildiği gibi montaj veya paylaşım noktası olarak kullanabilirsiniz.

Çok örnekli görev için belirttiğiniz kaynak dosyaları varsayılan olarak görevin çalışma `AZ_BATCH_TASK_WORKING_DIR`dizinine indirilir. Belirtildiği gibi, ortak kaynak dosyalarının aksine, yalnızca birincil görev çok örnekli görevin kendisi için belirtilen kaynak dosyalarını karşıdan yüklenir.

> [!IMPORTANT]
> Her zaman ortam `AZ_BATCH_TASK_SHARED_DIR` değişkenlerini kullanın ve `AZ_BATCH_TASK_WORKING_DIR` komut satırlarınızdaki bu dizinlere bakın. Yolları el ile oluşturmaya çalışmayın.
>
>

## <a name="task-lifetime"></a>Görev ömrü
Birincil görevin ömrü, çok örnekli görevin tamamının yaşam ayını denetler. Birincil çıkışlar olduğunda, tüm alt görevler sonlandırılır. Birincil çıkış kodu görevin çıkış kodudur ve bu nedenle yeniden deneme amacıyla görevin başarısını veya başarısızlığını belirlemek için kullanılır.

Alt görevlerden herhangi biri başarısız olursa, örneğin sıfır olmayan bir geri dönüş koduyla çıkarken, çok örnekli görevin tamamı başarısız olur. Çok örnekli görev daha sonra sonlandırılır ve yeniden deneme sınırına kadar yeniden denener.

Çok örnekli bir görevi sildiğinizde, birincil ve tüm alt görevler Toplu İşlem hizmeti tarafından da silinir. Tüm alt görev dizinleri ve dosyaları, standart bir görev için olduğu gibi işlem düğümlerinden silinir.

[MaxTaskRetryCount,][net_taskconstraint_maxretry] [MaxWallClockTime][net_taskconstraint_maxwallclock]ve [Bekletme Zamanı][net_taskconstraint_retention] özellikleri gibi çok örnekli bir görev için [Görev Kısıtlamaları,][net_taskconstraints] standart bir görev için olduğu gibi onurlandırıldı ve birincil ve tüm alt görevler için geçerlidir. Ancak, çok örnekli görevi işe ekledikten sonra [Bekletme Zamanı][net_taskconstraint_retention] özelliğini değiştirirseniz, bu değişiklik yalnızca birincil göreve uygulanır. Tüm alt görevler özgün [Bekletme Zamanı'nı][net_taskconstraint_retention]kullanmaya devam ediyor.

Bir işlem düğümünün son görev listesi, son görev çok örnekli bir görevin parçasıysa, bir alt görevin kimliğini yansıtır.

## <a name="obtain-information-about-subtasks"></a>Alt görevler hakkında bilgi edinin
Toplu İşlem .NET kitaplığını kullanarak alt görevler hakkında bilgi edinmek için [CloudTask.ListSubtasks][net_task_listsubtasks] yöntemini arayın. Bu yöntem, tüm alt görevlerdeki bilgileri ve görevleri gerçekleştiren bilgi düğümü hakkındaki bilgileri döndürür. Bu bilgilerden, her alt görevin kök dizinini, havuz kimliğini, geçerli durumunu, çıkış kodunu ve daha fazlasını belirleyebilirsiniz. Bu bilgileri [PoolOperations.GetNodeFile][poolops_getnodefile] yöntemiyle birlikte kullanarak alt görevin dosyalarını alabilirsiniz. Bu yöntemin birincil görev (id 0) için bilgi döndürmediğini unutmayın.

> [!NOTE]
> Aksi belirtilmedikçe, çok örnekli [CloudTask'ın][net_task] kendisinde çalışan Toplu .NET yöntemleri *yalnızca* birincil göreve uygulanır. Örneğin, çok örnekli bir görevde [CloudTask.ListNodeFiles][net_task_listnodefiles] yöntemini aradiğinizde, yalnızca birincil görevin dosyaları döndürülür.
>
>

Aşağıdaki kod snippet alt görev bilgilerinin nasıl elde edildiğini ve yürütüldikleri düğümlerden dosya içeriğini nasıl talep edilebildiğini gösterir.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Kod örneği
GitHub'daki [MultiInstanceTasks][github_mpi] kod örneği, Toplu işlem düğümlerinde bir [MS-MPI][msmpi_msdn] uygulamasını çalıştırmak için çok örnekli bir görevin nasıl kullanılacağını gösterir. Örneği çalıştırmak için [Hazırlık](#preparation) ve [Yürütme'deki](#execution) adımları izleyin.

### <a name="preparation"></a>Hazırlık
1. [Basit bir MS-MPI programını nasıl derleyip çalıştırılabilen][msmpi_howto]ilk iki adımı izleyin. Bu, aşağıdaki adımın ön koşullarının karşılar.
2. [MPIHelloWorld][helloworld_proj] örnek MPI programının *sürüm* sürümünü oluşturun. Bu, çok örnekli görev tarafından işlem düğümleri üzerinde çalıştırılacak programdır.
3. (adım 2'yi oluşturduğunuz) ve `MPIHelloWorld.exe` `MSMpiSetup.exe` (adım 1'i indirdiğiniz) içeren bir zip dosyası oluşturun. Bu zip dosyasını bir sonraki adımda bir uygulama paketi olarak yüklersiniz.
4. "MPIHelloWorld" adlı bir Toplu Iş [uygulaması](batch-application-packages.md) oluşturmak için [Azure portalını][portal] kullanın ve önceki adımda oluşturduğunuz zip dosyasını uygulama paketinin "1.0" sürümü olarak belirtin. Daha fazla bilgi için [yükleme ve uygulamaları yönetme](batch-application-packages.md#upload-and-manage-applications) bilgisine bakın.

> [!TIP]
> Uygulama *Release* paketinize `MPIHelloWorld.exe` ek bağımlılıklar (örneğin `msvcp140d.dll` veya) `vcruntime140d.dll`eklemek zorunda kalmamanız için bir Sürüm sürümü oluşturun.
>
>

### <a name="execution"></a>Yürütme
1. Azure [toplu örnekleri][github_samples_zip] GitHub'dan indirin.
2. Visual Studio 2019'da MultiInstanceTasks **çözümlerini** açın. Çözüm `MultiInstanceTasks.sln` dosyası şu adreste bulunur:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. `AccountSettings.settings` **Microsoft.Azure.Batch.Samples.Common** projesine Toplu Iş ve Depolama hesabı kimlik bilgilerinizi girin.
4. Toplu İşlem havuzundaki bilgi işlem düğümleri üzerindeki MPI örnek uygulamasını yürütmek için MultiInstanceTasks çözümlerini **oluşturun ve çalıştırın.**
5. *İsteğe Bağlı*: Kaynakları silmeden önce örnek havuzu, işi ve görevi ("MultiInstanceSampleSamplePool", "MultiInstanceSampleSampleJob", "MultiInstanceSampleSampleTask") incelemek için [Azure portalını][portal] veya [Toplu Gezgini'ni][batch_labs] kullanın.

> [!TIP]
> Visual Studio'nuz yoksa [Visual Studio Community'yi][visual_studio] ücretsiz olarak indirebilirsiniz.
>
>

Çıktı `MultiInstanceTasks.exe` aşağıdakilere benzer:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Sonraki adımlar
* Microsoft HPC & Azure Toplu İş Ekibi blogu, [Azure Toplu İşlem'de Linux için MPI desteğini][blog_mpi_linux]tartışır ve [OpenFOAM'u][openfoam] Toplu İşlem le kullanma hakkında bilgiler içerir. [GitHub'da OpenFOAM örneği][github_mpi]için Python kod örneklerini bulabilirsiniz.
* Azure Toplu MPI çözümlerinizde kullanılmak üzere [Linux bilgi işlem düğümlerinden oluşan havuzlar oluşturmayı](batch-linux-nodes.md) öğrenin.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_labs]: https://azure.github.io/BatchExplorer/
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: https://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: https://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Çok örnekli genel bakış"
