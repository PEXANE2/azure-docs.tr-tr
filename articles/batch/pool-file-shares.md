---
title: Azure Toplu Toplu havuzlar için Azure dosya paylaşımı | Microsoft Dokümanlar
description: Azure Toplu İşlem'de bir Linux veya Windows havuzundaki bilgi işlem düğümlerinden Azure Dosyaları paylaşımını nasıl monte edebilirsiniz?
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/24/2018
ms.author: labrenne
ms.custom: ''
ms.openlocfilehash: 156dad25af5abd1b4d5db32569faf09a23fadfb1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022520"
---
# <a name="use-an-azure-file-share-with-a-batch-pool"></a>Toplu Iş havuzunda Azure dosya paylaşımı kullanma

[Azure Files,](../storage/files/storage-files-introduction.md) Sunucu İleti Bloğu (SMB) protokolü aracılığıyla erişilebilen bulutta tam olarak yönetilen dosya paylaşımları sunar. Bu makalede, havuz bilgi işlem düğümlerinde azure dosya paylaşımı nı monte etmek ve kullanmak için bilgi ve kod örnekleri verilmektedir. Kod örnekleri Toplu İşlem .NET ve Python SDK'larını kullanır, ancak benzer işlemleri diğer Toplu SDK'lar ve araçları kullanarak gerçekleştirebilirsiniz.

Toplu iş, verileri okumak ve yazmak için Azure Depolama bloblarını kullanmak için yerel API desteği sağlar. Ancak, bazı durumlarda havuz bilgi işlem düğümlerinizden bir Azure dosya paylaşımına erişmek isteyebilirsiniz. Örneğin, Bir Kobİ dosya paylaşımına bağlı olan eski bir iş yükünüz vardır veya görevlerinizin paylaşılan verilere erişmesi veya paylaşılan çıktı oluşturması gerekir. 

## <a name="considerations-for-use-with-batch"></a>Toplu İş ile kullanım için dikkat edilecek hususlar

* Nispeten düşük sayıda paralel görev çalıştıran havuzlarınız olduğunda Azure dosya paylaşımı kullanmayı düşünün. Beklenen havuz boyutu ve varlık dosyası sayısı göz önüne alındığında, Azure Dosyalarının (Azure Depolama hesabı kullanan) kullanIlip kullanılmayeceğini belirlemek için [performans ve ölçek hedeflerini](../storage/files/storage-files-scale-targets.md) gözden geçirin. 

* Azure dosya paylaşımları [maliyet etkindir](https://azure.microsoft.com/pricing/details/storage/files/) ve veri çoğaltma ile başka bir bölgeye yapılandırılabilir, bu nedenle genel olarak gereksizdir. 

* Bir Azure dosya paylaşımını şirket içi bir bilgisayardan aynı anda monte edebilirsiniz.

* Ayrıca Azure dosya paylaşımları için genel [planlama hususlarına](../storage/files/storage-files-planning.md) bakın.


## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

Toplu Iş hesabınıza bağlı bir depolama hesabında veya ayrı bir depolama hesabında dosya [paylaşımı oluşturun.](../storage/files/storage-how-to-create-file-share.md)

## <a name="mount-a-share-on-a-windows-pool"></a>Windows havuzuna paylaşım damla

Bu bölümde, Windows düğümleri havuzuna Azure dosya paylaşımı monte etmek ve kullanmak için adımlar ve kod örnekleri sağlar. Ek arka plan için, Windows'a Azure dosya paylaşımı ekleme [belgelerine](../storage/files/storage-how-to-use-files-windows.md) bakın. 

Toplu İşlem'de, bir görev windows düğümünde çalıştırılıştırken her kez paylaşıma monte etmeniz gerekir. Şu anda, Windows düğümleri üzerindeki görevler arasındaki ağ bağlantısını sürdürmek mümkün değildir.

Örneğin, her `net use` görev komut satırının bir parçası olarak dosya paylaşımını monte etmek için bir komut ekleyin. Dosya paylaşımını monte etmek için aşağıdaki kimlik bilgileri gereklidir:

* **Kullanıcı adı**\\\<: AZURE\>depolama hesap\\adı , örneğin, AZURE*mystorageaccountnamename*
* **Şifre** \<: StorageAccountKeyWhichEnds in==>, örneğin, *XXXXXXXXXXXXXXXXXXXXX==*

Aşağıdaki komut, depolama hesabı *mystorageaccountname* s: sürücü olarak bir dosya payı *myfileshare* *bağlar:*

```
net use S: \\mystorageaccountname.file.core.windows.net\myfileshare /user:AZURE\mystorageaccountname XXXXXXXXXXXXXXXXXXXXX==
```

Basitlik için, buradaki örnekler kimlik bilgilerini doğrudan metinolarak geçirir. Uygulamada, kimlik bilgilerini ortam değişkenlerini, sertifikaları veya Azure Anahtar Kasası gibi bir çözümü kullanarak yönetmenizi şiddetle öneririz.

Montaj işlemini kolaylaştırmak için, isteğe bağlı olarak düğümler üzerindeki kimlik bilgilerini devam ettir. Ardından, paylaşım bilgilerini zedemeden bağlayabilirsiniz. Aşağıdaki iki adımı gerçekleştirin:

1. Havuz `cmdkey` yapılandırmasında bir başlangıç görevi kullanarak komut satırı yardımcı programını çalıştırın. Bu, her Windows düğümündeki kimlik bilgilerini devam eder. Başlangıç görevi komut satırı benzer:

   ```
   cmd /c "cmdkey /add:mystorageaccountname.file.core.windows.net /user:AZURE\mystorageaccountname /pass:XXXXXXXXXXXXXXXXXXXXX=="

   ```

2. Her düğümü kullanarak `net use`her görevin bir parçası olarak pay montaj. Örneğin, aşağıdaki görev komut satırı dosya paylaşımını *S:* sürücü olarak bağlar. Bunu, paylaşıma başvuran bir komut veya komut dosyası takip eder. Önbelleğe alınmış kimlik bilgileri' `net use`ye yapılan çağrıda kullanılır. Bu adım, havuzdaki başlangıç görevinde kullandığınız görevler için aynı kullanıcı kimliğini kullandığınızı varsayar ve bu da tüm senaryolar için uygun değildir.

   ```
   cmd /c "net use S: \\mystorageaccountname.file.core.windows.net\myfileshare" 
   ```

### <a name="c-example"></a>C# örneği
Aşağıdaki C# örneği, başlangıç görevini kullanarak Windows havuzundaki kimlik bilgilerinin nasıl devam edilebildiğini gösterir. Depolama dosyası hizmet adı ve depolama kimlik bilgileri tanımlı sabitler olarak geçirilir. Burada, başlangıç görevi havuz kapsamı olan standart (yönetici olmayan) otomatik kullanıcı hesabı altında çalışır.

```csharp
...
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: PoolId,
    targetDedicatedComputeNodes: PoolNodeCount,
    virtualMachineSize: PoolVMSize,
    virtualMachineConfiguration: virtualMachineConfiguration);

// Start task to store credentials to mount file share
string startTaskCommandLine = String.Format("cmd /c \"cmdkey /add:{0} /user:AZURE\\{1} /pass:{2}\"", StorageFileService, StorageAccountName, StorageAccountKey);

pool.StartTask = new StartTask
{
    CommandLine = startTaskCommandLine,
    UserIdentity = new UserIdentity(new AutoUserSpecification(
        elevationLevel: ElevationLevel.NonAdmin, 
        scope: AutoUserScope.Pool))
};

pool.Commit();
```

Kimlik bilgilerini depoladıktan sonra, paylaşımı monte etmek ve okuma veya yazma işlemlerinde paya başvurmak için görev komut satırlarınızı kullanın. Temel bir örnek olarak, aşağıdaki snippet'teki `dir` görev komut satırı, dosya paylaşımındaki dosyaları listelemek için komutu kullanır. Havuzda başlangıç görevini çalıştırmak için kullandığınız aynı [kullanıcı kimliğini](batch-user-accounts.md) kullanarak her iş görevini çalıştırdığınızdan emin olun. 

```csharp
...
string taskId = "myTask";
string taskCommandLine = String.Format("cmd /c \"net use {0} {1} & dir {2}\"", ShareMountPoint, StorageFileShare, ShareMountPoint);

CloudTask task = new CloudTask(taskId, taskCommandLine);
task.UserIdentity = new UserIdentity(new AutoUserSpecification(
    elevationLevel: ElevationLevel.NonAdmin,
    scope: AutoUserScope.Pool));
tasks.Add(task);
```

## <a name="mount-a-share-on-a-linux-pool"></a>Linux havuzuna paylaşım dama

Azure dosya paylaşımları [CIFS çekirdek istemcisi](https://wiki.samba.org/index.php/LinuxCIFS)kullanılarak Linux dağıtımlarına monte edilebilir. Aşağıdaki örnek, Ubuntu 16.04 LTS işlem düğümleri havuzuna dosya paylaşımının nasıl monte edilebildiğini gösterir. Farklı bir Linux dağıtımı kullanıyorsanız, genel adımlar benzerdir, ancak dağıtım için uygun paket yöneticisini kullanın. Ayrıntılar ve ek örnekler için [bkz.](../storage/files/storage-how-to-use-files-linux.md)

İlk olarak, yönetici kullanıcı kimliği `cifs-utils` altında paketi yükleyin ve yerel dosya sisteminde montaj noktasını (örneğin, */mnt/MyAzureFileShare)* oluşturun. Montaj noktası için bir klasör dosya sisteminin herhangi bir yerinde oluşturulabilir, ancak `/mnt` bunu klasörün altında oluşturmak yaygın bir kuraldır. Doğrudan (Ubuntu'da) `/mnt` veya `/mnt/resource` (diğer dağıtımlarda) bir montaj noktası oluşturmamaya dikkat edin.

```
apt-get update && apt-get install cifs-utils && sudo mkdir -p /mnt/MyAzureFileShare
```

Ardından, bu `mount` kimlik bilgilerini sağlayarak dosya paylaşımını monte etmek için komutu çalıştırın:

* **Kullanıcı**adı \<:\>storageaccountname , örneğin, *mystorageaccountname*
* **Şifre** \<: StorageAccountKeyWhichEnds in==>, örneğin, *XXXXXXXXXXXXXXXXXXXXX==*

Aşağıdaki komut */ mnt/ MyAzureFileShare*de *depolama hesabı mystorageaccountname* bir dosya payı *myfileshare* bağlar: 

```
mount -t cifs //mystorageaccountname.file.core.windows.net/myfileshare /mnt/MyAzureFileShare -o vers=3.0,username=mystorageaccountname,password=XXXXXXXXXXXXXXXXXXXXX==,dir_mode=0777,file_mode=0777,serverino && ls /mnt/MyAzureFileShare
```

Basitlik için, buradaki örnekler kimlik bilgilerini doğrudan metinolarak geçirir. Uygulamada, kimlik bilgilerini ortam değişkenlerini, sertifikaları veya Azure Anahtar Kasası gibi bir çözümü kullanarak yönetmenizi şiddetle öneririz.

Linux havuzunda, bu adımların tümünün tek bir başlangıç görevinde birleştirilmesi veya bir komut dosyasında çalıştırılabilirsiniz. Havuzda yönetici kullanıcı olarak başlangıç görevini çalıştırın. Paylaşıma başvuran havuzda başka görevleri çalıştırmadan önce başlangıç görevinizi başarıyla tamamlamayı bekleyecek şekilde ayarlayın.

### <a name="python-example"></a>Python örneği

Aşağıdaki Python örneği, bir başlangıç görevinde payı birleştirmek için bir Ubuntu havuzunu nasıl yapılandırıştırılatır' ı gösterir. Montaj noktası, dosya paylaşımı bitiş noktası ve depolama kimlik bilgileri tanımlı sabitler olarak geçirilir. Başlangıç görevi, havuz kapsamı olan bir yönetici otomatik kullanıcı hesabı altında çalışır.

```python
pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=batchmodels.ImageReference(
            publisher="Canonical",
            offer="UbuntuServer",
            sku="16.04.0-LTS",
            version="latest"),
        node_agent_sku_id="batch.node.ubuntu 16.04"),
    vm_size=_POOL_VM_SIZE,
    target_dedicated_nodes=_POOL_NODE_COUNT,
    start_task=batchmodels.StartTask(
        command_line="/bin/bash -c \"apt-get update && apt-get install cifs-utils && mkdir -p {} && mount -t cifs {} {} -o vers=3.0,username={},password={},dir_mode=0777,file_mode=0777,serverino\"".format(
            _COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_SHARE_ENDPOINT, _COMPUTE_NODE_MOUNT_POINT, _STORAGE_ACCOUNT_NAME, _STORAGE_ACCOUNT_KEY),
        wait_for_success=True,
        user_identity=batchmodels.UserIdentity(
            auto_user=batchmodels.AutoUserSpecification(
                scope=batchmodels.AutoUserScope.pool,
                elevation_level=batchmodels.ElevationLevel.admin)),
    )
)
batch_service_client.pool.add(pool)
```

Payı monte ettikten ve bir işi tanımladıktan sonra, görev komut satırlarınızdaki paylaşımı kullanın. Örneğin, aşağıdaki temel komut, dosya paylaşımındaki dosyaları listelemek için kullanır. `ls`

```python
...
task = batch.models.TaskAddParameter(
    id='mytask',
    command_line="/bin/bash -c \"ls {}\"".format(_COMPUTE_NODE_MOUNT_POINT))

batch_service_client.task.add(job_id, task)
```


## <a name="next-steps"></a>Sonraki adımlar

* Toplu İşlem'de veri okuyup yazmak için diğer seçenekler için [Toplu İşlem özelliğine genel bakış](batch-api-basics.md) ve iş ve görev [çıktısını devam taslayın'a](batch-task-output.md)bakın.

* Ayrıca toplu konteyner iş yükleri için dosya sistemleri dağıtmak için [Tersane tarifleri](https://github.com/Azure/batch-shipyard/tree/master/recipes) içeren [Toplu Tersane](https://github.com/Azure/batch-shipyard) araç kiti, bakın.
