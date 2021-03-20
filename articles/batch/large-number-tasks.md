---
title: Bir Batch işine çok sayıda görev gönderme
description: Tek bir Azure Batch işinde çok fazla sayıda görevi verimli bir şekilde göndermeyi öğrenin.
ms.topic: how-to
ms.date: 12/30/2020
ms.custom: devx-track-python, devx-track-csharp
ms.openlocfilehash: 08cf92507a4556afbf56c9cb7e2c9c1b3a6c9479
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97831525"
---
# <a name="submit-a-large-number-of-tasks-to-a-batch-job"></a>Bir Batch işine çok sayıda görev gönderme

Büyük ölçekli Azure Batch iş yüklerini çalıştırdığınızda, tek bir işte on binlerce, yüzlerce binlerce veya hatta daha fazla görev göndermek isteyebilirsiniz.

Bu makalede, çok sayıda görevin, önemli ölçüde artan iş hacmi tek bir toplu işe nasıl gönderileceği gösterilir. Görevler gönderildikten sonra, iş için belirttiğiniz havuzda işlenmek üzere toplu Işlem kuyruğu girer.

## <a name="use-task-collections"></a>Görev koleksiyonlarını kullanma

Çok sayıda görev eklerken, görevleri aynı anda değil bir *koleksiyon* olarak eklemek Için Batch API 'leri tarafından sunulan uygun yöntemleri veya aşırı yüklemeleri kullanın. Genellikle, işiniz için bir giriş dosyası veya parametre kümesi üzerinde yineleme yaparken görevleri tanımlayarak görev koleksiyonu oluşturursunuz.

Tek bir çağrıda ekleyebileceğiniz görev koleksiyonunun en büyük boyutu, kullandığınız Batch API 'sine bağlıdır.

### <a name="apis-allowing-collections-of-up-to-100-tasks"></a>100 'e kadar görevi koleksiyonlara izin veren API 'Ler

Bu Batch API 'Leri, koleksiyonu 100 görevle sınırlandırır. Sınır, görevlerin boyutuna (örneğin, görevlerde çok sayıda kaynak dosyası veya ortam değişkeni varsa) bağlı olarak daha küçük olabilir.

- [REST API](/rest/api/batchservice/task/addcollection)
- [Python API’si](/python/api/azure-batch/azure.batch.operations.TaskOperations)
- [Node.js API’si](/javascript/api/@azure/batch/task)

Bu API 'Leri kullanırken, görev sayısını koleksiyon sınırını karşılayacak şekilde bölmek ve görev ekleme hataları durumunda hataları ve yeniden denemeleri işlemek için mantık sağlamanız gerekir. Bir görev koleksiyonu eklemek için çok büyükse istek bir hata oluşturur ve daha az görevle yeniden denenmelidir.

### <a name="apis-allowing-collections-of-larger-numbers-of-tasks"></a>Daha fazla sayıda görev koleksiyonuna izin veren API 'Ler

Diğer Batch API 'Leri, gönderim istemcisinde yalnızca RAM kullanılabilirliğiyle sınırlı olan çok daha büyük görev koleksiyonlarını destekler. Bu API 'Ler, görev koleksiyonunu alt düzey API 'Ler için "parçalara ayırır" ve görev ekleme hatalarının yeniden denemeleri için saydam bir şekilde işler.

- [.NET API’si](/dotnet/api/microsoft.azure.batch.cloudjob.addtaskasync)
- [Java API’si](/java/api/com.microsoft.azure.batch.protocol.tasks.addcollectionasync)
- Batch CLı şablonlarıyla [Azure Batch CLI uzantısı](batch-cli-templates.md)
- [Python SDK uzantısı](https://pypi.org/project/azure-batch-extensions/)

## <a name="increase-throughput-of-task-submission"></a>Görev gönderimi verimini artırma

Bir işe büyük bir görev koleksiyonu eklemek biraz zaman alabilir. Örneğin, .NET API aracılığıyla 20.000 görevi eklemek bir dakikaya kadar sürebilir. Batch API 'sine ve iş yüküne bağlı olarak, aşağıdakilerden birini veya birkaçını değiştirerek görev işleme hızını artırabilirsiniz.

### <a name="task-size"></a>Görev boyutu

Büyük görevleri eklemek, daha küçük olanlar eklemekten daha uzun sürer. Bir koleksiyondaki her görevin boyutunu azaltmak için, görev komut satırını basitleştirebilir, ortam değişkenlerinin sayısını azaltabilir veya görev yürütme gereksinimlerini daha verimli bir şekilde işleyebilirsiniz.

Örneğin, çok sayıda kaynak dosyası kullanmak yerine, havuzdaki bir [Başlangıç görevini](jobs-and-tasks.md#start-task) kullanarak görev bağımlılıklarını yükler veya bir [uygulama paketi](batch-application-packages.md) veya [Docker kapsayıcısı](batch-docker-container-workloads.md)kullanın.

### <a name="number-of-parallel-operations"></a>Paralel işlem sayısı

Batch API 'sine bağlı olarak, toplu iş istemcisi tarafından en fazla eşzamanlı işlem sayısını artırarak aktarım hızını artırabilirsiniz. Bu ayarı, .NET API 'sindeki [BatchClientParallelOptions. Maxdegreeofparalellik](/dotnet/api/microsoft.azure.batch.batchclientparalleloptions.maxdegreeofparallelism) özelliğini veya `threads` Batch Python SDK uzantısında [TaskOperations.add_collection](/python/api/azure-batch/azure.batch.operations.TaskOperations) gibi yöntemlerin parametresini kullanarak yapılandırın. (Bu özellik yerel Batch Python SDK 'sında kullanılamaz.)

Varsayılan olarak, bu özellik 1 olarak ayarlanır, ancak işlem verimini geliştirmek için daha yüksek bir ayar yapabilirsiniz. Ağ bant genişliğini ve bazı CPU performansını tüketerek artan aktarım hızına sahip olursunuz. Görev verimlilik, veya 100 katına kadar artar `MaxDegreeOfParallelism` `threads` . Uygulamada, eş zamanlı işlemlerin sayısını 100 altına ayarlamanız gerekir.

 Batch şablonları ile Azure Batch CLı uzantısı, kullanılabilir çekirdek sayısına bağlı olarak eşzamanlı işlem sayısını otomatik olarak arttırır, ancak bu özellik CLı 'da yapılandırılamaz.

### <a name="http-connection-limits"></a>HTTP bağlantı sınırları

Çok sayıda eş zamanlı HTTP bağlantısı olması, çok sayıda görev eklerken Batch istemcisinin performansını kısıtlayabilir. Bazı API 'Ler HTTP bağlantısı sayısını sınırlar. .NET API ile geliştirilirken, örneğin, [ServicePointManager. DefaultConnectionLimit](/dotnet/api/system.net.servicepointmanager.defaultconnectionlimit) özelliği varsayılan olarak 2 olarak ayarlanır. Sayıyı, paralel işlem sayısından daha fazla veya daha büyük bir sayıya artırmanız önerilir.

## <a name="example-batch-net"></a>Örnek: Batch .NET

Aşağıdaki C# parçacıkları Batch .NET API 'SI kullanılarak çok sayıda görev eklenirken yapılandırılacak ayarları gösterir.

Görev aktarım hızını artırmak için [Batchclient](/dotnet/api/microsoft.azure.batch.batchclient)'ın [Maxdegreeofparallelilik](/dotnet/api/microsoft.azure.batch.batchclientparalleloptions.maxdegreeofparallelism) özelliğinin değerini artırın. Örnek:

```csharp
BatchClientParallelOptions parallelOptions = new BatchClientParallelOptions()
  {
    MaxDegreeOfParallelism = 15
  };
...
```

[Addtaskasync](/dotnet/api/microsoft.azure.batch.cloudjob.addtaskasync) veya [AddTask](/dotnet/api/microsoft.azure.batch.cloudjob.addtask
) yönteminin uygun aşırı yüklemesini kullanarak işe bir görev koleksiyonu ekleyin. Örnek:

```csharp
// Add a list of tasks as a collection
List<CloudTask> tasksToAdd = new List<CloudTask>(); // Populate with your tasks
...
await batchClient.JobOperations.AddTaskAsync(jobId, tasksToAdd, parallelOptions);
```

## <a name="example-batch-cli-extension"></a>Örnek: Batch CLı uzantısı

[Batch CLI şablonlarıyla](batch-cli-templates.md)Azure Batch CLI uzantılarını kullanarak, bir [görev fabrikası](https://github.com/Azure/azure-batch-cli-extensions/blob/master/doc/taskFactories.md)IÇEREN bir iş şablonu JSON dosyası oluşturun. Görev fabrikası, tek bir görev tanımındaki bir iş için ilgili görevlerin bir koleksiyonunu yapılandırır.  

Aşağıda, çok sayıda görevi olan tek boyutlu bir parametrik tarama işi için örnek bir iş şablonu verilmiştir (Bu durumda, 250.000). Görev komut satırı basit bir `echo` komuttur.

```json
{
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "myjob",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "mypool"
            },
            "taskFactory": {
                "type": "parametricSweep",
                "parameterSets": [
                    {
                        "start": 1,
                        "end": 250000,
                        "step": 1
                    }
                ],
                "repeatTask": {
                    "commandLine": "/bin/bash -c 'echo Hello world from task {0}'",
                    "constraints": {
                        "retentionTime":"PT1H"
                    }
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

Şablonuyla bir işi çalıştırmak için bkz. [CLI şablonlarını Azure Batch ve dosya aktarımını kullanma](batch-cli-templates.md).

## <a name="example-batch-python-sdk-extension"></a>Örnek: Batch Python SDK uzantısı

Azure Batch Python SDK uzantısını kullanmak için önce Python SDK 'Yı ve uzantıyı yüklemeniz gerekir:

```
pip install azure-batch
pip install azure-batch-extensions
```

`BatchExtensionsClient`SDK uzantısını kullanan bir ayarlama:

```python

client = batch.BatchExtensionsClient(
    base_url=BATCH_ACCOUNT_URL, resource_group=RESOURCE_GROUP_NAME, batch_account=BATCH_ACCOUNT_NAME)
...
```

Bir işe eklemek için bir görev koleksiyonu oluşturun. Örnek:

```python
tasks = list()
# Populate the list with your tasks
...
```

[Task.add_collection](/python/api/azure-batch/azure.batch.operations.TaskOperations)kullanarak görev koleksiyonunu ekleyin. `threads`Eşzamanlı işlemlerin sayısını artırmak için parametresini ayarlayın:

```python
try:
    client.task.add_collection(job_id, threads=100)
except Exception as e:
    raise e
```

Batch Python SDK uzantısı, bir görev fabrikasının JSON belirtimini kullanarak işe görev parametreleri eklemeyi de destekler. Örneğin, önceki Batch CLı şablonu örneğinde olduğu gibi bir parametrik tarama için iş parametrelerini yapılandırın:

```python
parameter_sweep = {
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "myjob",
            "poolInfo": {
                "poolId": "mypool"
            },
            "taskFactory": {
                "type": "parametricSweep",
                "parameterSets": [
                    {
                        "start": 1,
                        "end": 250000,
                        "step": 1
                    }
                ],
                "repeatTask": {
                    "commandLine": "/bin/bash -c 'echo Hello world from task {0}'",
                    "constraints": {
                        "retentionTime": "PT1H"
                    }
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
...
job_json = client.job.expand_template(parameter_sweep)
job_parameter = client.job.jobparameter_from_json(job_json)
```

İş parametrelerini işe ekleyin. `threads`Eşzamanlı işlemlerin sayısını artırmak için parametresini ayarlayın:

```python
try:
    client.job.add(job_parameter, threads=50)
except Exception as e:
    raise e
```

## <a name="next-steps"></a>Sonraki adımlar

- [Batch CLI şablonlarıyla](batch-cli-templates.md)Azure Batch CLI uzantısını kullanma hakkında daha fazla bilgi edinin.
- [Batch Python SDK uzantısı](https://pypi.org/project/azure-batch-extensions/)hakkında daha fazla bilgi edinin.
- [Azure Batch için en iyi uygulamalar](best-practices.md)hakkında bilgi edinin.
