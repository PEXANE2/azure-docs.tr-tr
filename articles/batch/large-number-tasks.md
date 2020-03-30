---
title: Çok sayıda görev gönderin - Azure Toplu İş | Microsoft Dokümanlar
description: Tek bir Azure Toplu İş işinde çok sayıda görevi verimli bir şekilde gönderme
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 08/24/2018
ms.author: labrenne
ms.custom: ''
ms.openlocfilehash: c3857e512da5fe4fceefa5f735ddc65f73e11623
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77026056"
---
# <a name="submit-a-large-number-of-tasks-to-a-batch-job"></a>Toplu iş için çok sayıda görev gönderme

Büyük ölçekli Azure Toplu Iş Yükü çalıştırdığınızda, tek bir işe on binlerce, yüz binlerce veya hatta daha fazla görev göndermek isteyebilirsiniz. 

Bu makalede, tek bir Toplu iş için önemli ölçüde artırılmış iş parçacığı ile görevlerin çok sayıda göndermek için rehberlik ve bazı kod örnekleri verir. Görevler gönderildikten sonra, iş için belirttiğiniz havuzda işlenmek üzere Toplu İşlem sırasını girerler.

## <a name="use-task-collections"></a>Görev koleksiyonlarını kullanma

Toplu İş API'leri, bir işe bir *toplama*göreviolarak verimli bir şekilde ekleyecek yöntemler sağlar. Çok sayıda görev eklerken, toplama olarak görev eklemek için uygun yöntemleri veya aşırı yüklemeleri kullanmanız gerekir. Genellikle, işiniz için bir dizi giriş dosyası veya parametre üzerinde yinelenirken görevleri tanımlayarak bir görev koleksiyonu oluşturuyorsunuz.

Tek bir aramada ekleyebileceğiniz görev koleksiyonunun maksimum boyutu, kullandığınız Toplu İşlem API'ye bağlıdır:

* Aşağıdaki Toplu İş API'leri koleksiyonu **100 görevle**sınırlandırır. Sınır görevlerin boyutuna bağlı olarak daha küçük olabilir - örneğin, görevlerde çok sayıda kaynak dosyası veya ortam değişkeni varsa.

    * [REST API](/rest/api/batchservice/task/addcollection)
    * [Python API’si](/python/api/azure-batch/azure.batch.operations.TaskOperations?view=azure-python)
    * [Düğüm.js API](/javascript/api/@azure/batch/task?view=azure-node-latest)

  Bu API'leri kullanırken, toplama sınırını karşılamak için görev sayısını bölmek ve görevlerin ekinde hataları ve yeniden denemeleri işlemek için mantık sağlamanız gerekir. Görev koleksiyonu eklenemeyecek kadar büyükse, istek bir hata oluşturur ve daha az görevle yeniden denenmelidir.

* Aşağıdaki API'ler çok daha büyük görev koleksiyonlarını destekler - yalnızca gönderen istemcideki RAM kullanılabilirliğiyle sınırlıdır. Bu API'ler, görev ekleme başarısız olursa, görev koleksiyonunu alt düzey API'ler için "parçalara" bölmeyi ve yeniden denemeleri saydam olarak işler.

    * [.NET API'si](/dotnet/api/microsoft.azure.batch.cloudjob.addtaskasync?view=azure-dotnet)
    * [Java API’si](/java/api/com.microsoft.azure.batch.protocol.tasks.addcollectionasync?view=azure-java-stable)
    * Toplu CLI şablonları ile [Azure Toplu CLI uzantısı](batch-cli-templates.md)
    * [Python SDK uzantısı](https://pypi.org/project/azure-batch-extensions/)

## <a name="increase-throughput-of-task-submission"></a>Görev gönderiminin iş artış

Bir işe büyük bir görev koleksiyonu eklemek biraz zaman alabilir - örneğin, .NET API üzerinden 20.000 görev eklemek 1 dakikaya kadar. Toplu İş Api'nize ve iş yükünüze bağlı olarak, aşağıdakilerden birini veya birkaçını değiştirerek görev iş kısmını geliştirebilirsiniz:

* **Görev boyutu** - Büyük görevler eklemek, daha küçük görevler eklemekten daha uzun sürer. Bir koleksiyondaki her görevin boyutunu küçültmek için görev komut satırını basitleştirebilir, ortam değişkenlerinin sayısını azaltabilir veya görev yürütme gereksinimlerini daha verimli bir şekilde işleyebilirsiniz. Örneğin, çok sayıda kaynak dosyası kullanmak yerine, havuzda [başlangıç görevi](batch-api-basics.md#start-task) kullanarak görev bağımlılıklarını yükleyin veya bir [uygulama paketi](batch-application-packages.md) veya [Docker kapsayıcısı](batch-docker-container-workloads.md)kullanın.

* **Paralel işlem sayısı** - Toplu ApI'ye bağlı olarak, Toplu Işlem istemcisi tarafından eşzamanlı işlemlerin maksimum sayısını artırarak verimi artırır. Bu ayarı ,NET API'deki [BatchClientParallelOptions.MaxDegreeOfParallelism](/dotnet/api/microsoft.azure.batch.batchclientparalleloptions.maxdegreeofparallelism) özelliğini `threads` veya Batch Python SDK uzantısında [TaskOperations.add_collection](/python/api/azure-batch/azure.batch.operations.TaskOperations?view=azure-python) gibi yöntemlerin parametresini kullanarak yapılandırın. (Bu özellik yerel Toplu Python SDK'da kullanılamaz.) Varsayılan olarak, bu özellik 1 olarak ayarlanır, ancak işlem işlerini geliştirmek için daha yüksek ayarlayın. Ağ bant genişliği ve bazı CPU performansı tüketerek artan iş kısmını dengelersiniz. Görev çıktısı 100 kata `MaxDegreeOfParallelism` kadar `threads`artar veya. Uygulamada, eşzamanlı işlem sayısını 100'ün altına ayarlamanız gerekir. 
 
  Toplu İşlem şablonları içeren Azure Toplu İşlem CLI uzantısı, kullanılabilir çekirdek sayısına bağlı olarak eşzamanlı işlem sayısını otomatik olarak artırır, ancak bu özellik CLI'de yapılandırılamaz. 

* **HTTP bağlantı sınırları** - Eşzamanlı HTTP bağlantılarının sayısı, çok sayıda görev eklendiğinde Toplu İşlem istemcisinin performansını azaltabilir. HTTP bağlantılarının sayısı belirli API'lerle sınırlıdır. Örneğin.NET API ile geliştirilirken [ServicePointManager.DefaultConnectionLimit](/dotnet/api/system.net.servicepointmanager.defaultconnectionlimit) özelliği varsayılan olarak 2 olarak ayarlanır. Değeri paralel işlem sayısına yakın veya daha büyük bir sayıya çıkarmanızı öneririz.

## <a name="example-batch-net"></a>Örnek: Toplu .NET

Aşağıdaki C# parçacıkları, Toplu İşlem .NET API'yi kullanarak çok sayıda görev eklerken yapılandırmak için ayarları gösterir.

Görev çıktısını artırmak [için, BatchClient'ın](/dotnet/api/microsoft.azure.batch.batchclient?view=azure-dotnet) [MaxDegreeOfParallelism](/dotnet/api/microsoft.azure.batch.batchclientparalleloptions.maxdegreeofparallelism) özelliğinin değerini artırın. Örnek:

```csharp
BatchClientParallelOptions parallelOptions = new BatchClientParallelOptions()
  {
    MaxDegreeOfParallelism = 15
  };
...
```
[AddTaskAsync](/dotnet/api/microsoft.azure.batch.cloudjob.addtaskasync?view=azure-dotnet) veya [AddTask](/dotnet/api/microsoft.azure.batch.cloudjob.addtask?view=azure-dotnet
) yönteminin uygun aşırı yükünü kullanarak işe bir görev koleksiyonu ekleyin. Örnek:

```csharp
// Add a list of tasks as a collection
List<CloudTask> tasksToAdd = new List<CloudTask>(); // Populate with your tasks
...
await batchClient.JobOperations.AddTaskAsync(jobId, tasksToAdd, parallelOptions);
```


## <a name="example-batch-cli-extension"></a>Örnek: Toplu CLI uzantısı

[Toplu CLI şablonları](batch-cli-templates.md)ile Azure Toplu CLI uzantılarını kullanarak, görev [fabrikası](https://github.com/Azure/azure-batch-cli-extensions/blob/master/doc/taskFactories.md)içeren bir iş şablonu JSON dosyası oluşturun. Görev fabrikası, tek bir görev tanımından bir iş için ilgili görevler koleksiyonunu yapılandırır.  

Bu durumda, 250.000 görevleri çok sayıda tek boyutlu parametrik süpürme iş için örnek bir iş şablonu aşağıdadır. Görev komut satırı basit `echo` bir komutdur.

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
Şablonla bir işi çalıştırmak için Azure [Toplu CLI şablonlarını ve dosya aktarımını kullan'a](batch-cli-templates.md)bakın.

## <a name="example-batch-python-sdk-extension"></a>Örnek: Toplu Python SDK uzantısı

Azure Toplu Python SDK uzantısını kullanmak için önce Python SDK'yı ve uzantıyı yükleyin:

```
pip install azure-batch
pip install azure-batch-extensions
```

SDK `BatchExtensionsClient` uzantısını kullanan bir ayarlama:

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

[Task.add_collection](/python/api/azure-batch/azure.batch.operations.TaskOperations?view=azure-python)kullanarak görev toplama ekleyin. Eşzamanlı `threads` işlem sayısını artırmak için parametreyi ayarlayın:

```python
try:
    client.task.add_collection(job_id, threads=100)
except Exception as e:
    raise e
```

Toplu Python SDK uzantısı, görev fabrikası için JSON belirtimini kullanarak işe görev parametreleri eklemeyi de destekler. Örneğin, önceki Toplu CLI şablonörneğindekine benzer bir parametrik süpürme için iş parametrelerini yapılandırın:

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

İş parametrelerini işe ekleyin. Eşzamanlı `threads` işlem sayısını artırmak için parametreyi ayarlayın:

```python
try:
    client.job.add(job_parameter, threads=50)
except Exception as e:
    raise e
```

## <a name="next-steps"></a>Sonraki adımlar

* [Toplu CLI şablonlarıyla](batch-cli-templates.md)Azure Toplu CLI uzantısını kullanma hakkında daha fazla bilgi edinin.
* Toplu Python [SDK uzantısı](https://pypi.org/project/azure-batch-extensions/)hakkında daha fazla bilgi edinin.
