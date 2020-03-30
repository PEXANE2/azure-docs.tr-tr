---
title: Verimli liste sorguları tasarlama - Azure Toplu İş | Microsoft Dokümanlar
description: Havuzlar, işler, görevler ve bilgi işlem düğümleri gibi Toplu Iş kaynakları hakkında bilgi isterken sorgularınızı filtreleyerek performansı nızı artırın.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 031fefeb-248e-4d5a-9bc2-f07e46ddd30d
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 12/07/2018
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: df923ac479ce5f5a3668c18c616b11348dc6c0b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022248"
---
# <a name="create-queries-to-list-batch-resources-efficiently"></a>Toplu iş kaynaklarını verimli bir şekilde listelemek için sorgular oluşturma

Burada, Toplu İşlem kitaplığıyla işleri, görevleri, işlem düğümlerini ve diğer kaynakları sorgularken hizmet tarafından döndürülen veri miktarını azaltarak Azure Toplu [Uygulamanızın][api_net] performansını nasıl artırabileceğinizi öğreneceksiniz.

Hemen hemen tüm Toplu iş uygulamalarının, genellikle düzenli aralıklarla Toplu İşlem hizmetini sorgulayan bir tür izleme veya başka bir işlem gerçekleştirmesi gerekir. Örneğin, bir görevde bekleyen sıralanmış görevler olup olmadığını belirlemek için, işteki her görevle ilgili veri almanız gerekir. Havuzunuzdaki düğümlerin durumunu belirlemek için havuzdaki her düğüm hakkında veri almanız gerekir. Bu makalede, bu tür sorguların nasıl en verimli şekilde yürütülecek şekilde açıklanmaktadır.

> [!NOTE]
> Toplu İşlem hizmeti, bir işteki görevleri sayma ve Toplu İşlem düğümlerini toplu birleştirme havuzunda sayma ortak senaryoları için özel API desteği sağlar. Bunlar için bir liste sorgusu kullanmak yerine, [Görev Sayımları Al][rest_get_task_counts] ve [Liste Havuzu Düğüm Sayıları][rest_get_node_counts] işlemlerini arayabilirsiniz. Bu işlemler bir liste sorgusundan daha verimlidir, ancak daha sınırlı bilgi döndürün. Bkz. [Görevleri Say ve düğümleri duruma göre hesapla.](batch-get-resource-counts.md) 


## <a name="meet-the-detaillevel"></a>Ayrıntı Düzeyi ile Tanışın
Üretim Toplu işlemi uygulamasında, işler, görevler ve bilgi işlem düğümleri gibi varlıklar binlerce numara lanabilir. Bu kaynaklar hakkında bilgi istediğinizde, büyük miktarda verinin toplu iş hizmetinden her sorguda uygulamanız için "kabloyu geçmesi" gerekir. Bir sorgu tarafından döndürülen öğe sayısını ve bilgi türünü sınırlayarak, sorgularınızın hızını ve dolayısıyla uygulamanızın performansını artırabilirsiniz.

Bu [Toplu İşlem .NET][api_net] API kodu snippet, bir işle ilişkili *her* görevi ve her görevin *tüm* özelliklerini listeler:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Ancak, sorgunuza bir "ayrıntı düzeyi" uygulayarak çok daha verimli bir liste sorgusu gerçekleştirebilirsiniz. Bunu, [JobOperations.ListTasks][net_list_tasks] yöntemine [bir ODATADetailLevel][odata] nesnesi sağlayarak yaparsınız. Bu parçacık yalnızca tamamlanan görevlerin kimlik, komut satırı ve bilgi düğümü bilgi özelliklerini döndürür:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

Bu örnek senaryoda, işte binlerce görev varsa, ikinci sorgunun sonuçları genellikle ilkinden çok daha hızlı döndürülür. Toplu Iş .NET API ile öğeleri listelediğinizde ODATADetailLevel kullanma hakkında daha fazla bilgi [aşağıda](#efficient-querying-in-batch-net)yer almaktadır.

> [!IMPORTANT]
> Uygulamanızın maksimum verimliliğini ve performansını sağlamak için .NET API liste çağrılarınıza *her zaman* bir ODATADetailLevel nesnesi sağlamanızı öneririz. Ayrıntı düzeyi belirterek, Toplu Iş parti hizmeti yanıt sürelerini düşürmeye, ağ kullanımını geliştirmeye ve istemci uygulamaları tarafından bellek kullanımını en aza indirmeye yardımcı olabilirsiniz.
> 
> 

## <a name="filter-select-and-expand"></a>Filtreleme, seçme ve genişletme
[Toplu .NET][api_net] ve [Toplu DINLENME][api_rest] API'leri, hem bir listede döndürülen madde sayısını hem de her biri için döndürülen bilgi miktarını azaltma olanağı sağlar. Bunu, liste sorgularını gerçekleştirirken **filtreyi,** **seçin**ve **dizeleri genişleterek** yaparsınız.

### <a name="filter"></a>Filtre
Filtre dizesi döndürülen öğe sayısını azaltan bir ifadedir. Örneğin, yalnızca bir iş için çalışan görevleri listele veya yalnızca görevleri çalıştırmaya hazır olan işlem düğümlerini listele.

* Filtre dizesi, bir özellik adı, işleç ve değerden oluşan bir ifadeyle bir veya daha fazla ifadeden oluşur. Belirtilebilen özellikler, sorguladığınız her varlık türüne ve her özellik için desteklenen işleçlere özgür.
* Birden çok ifade mantıksal işleçler `and` `or`kullanılarak birleştirilebilir ve.
* Bu örnek filtre dizesi yalnızca çalışan `(state eq 'running') and startswith(id, 'renderTask')`"render" görevleri listeler: .

### <a name="select"></a>Şunu seçin:
Select string, her öğe için döndürülen özellik değerlerini sınırlar. Özellik adlarının listesini belirtirsiniz ve sorgu sonuçlarındaki öğeler için yalnızca bu özellik değerleri döndürülür.

* Select string, virgülle ayrılmış özellik adlarının bir listesini oluşur. Sorguladığınız varlık türü için özelliklerden herhangi birini belirtebilirsiniz.
* Bu örnek, select string, her görev için yalnızca üç `id, state, stateTransitionTime`özellik değeri döndürülmesi gerektiğini belirtir: .

### <a name="expand"></a>Genişlet
Genişletme dizesi, belirli bilgileri elde etmek için gereken API çağrılarının sayısını azaltır. Genişletme dizesi kullandığınızda, tek bir API çağrısıyla her öğe hakkında daha fazla bilgi elde edilebilir. Varlıkların listesini önce almak, ardından listedeki her öğe için bilgi istemek yerine, tek bir API çağrısında aynı bilgileri elde etmek için genişletme dizesini kullanırsınız. Daha az API çağrısı daha iyi performans anlamına gelir.

* Seçili dizebenzer şekilde, genişletme dizesi belirli verilerin liste sorgusu sonuçlarına dahil edilip edilmeyeceğini denetler.
* Genişletme dizesi yalnızca işleri, iş zamançizelgelerini, görevleri ve havuzları listelemede kullanıldığında desteklenir. Şu anda yalnızca istatistik bilgilerini destekler.
* Tüm özellikler gerektiğinde ve seçili dize belirtilmediğinde, istatistik bilgilerini almak için genişletme *dizesi* kullanılmalıdır. Bir özellik alt kümesi elde etmek için bir `stats` seçstring kullanılırsa, o zaman seçili dize belirtilebilir ve genişletme dizesi belirtilmesi gerekmez.
* Bu örnek genişletme dizesinde, listedeki her öğe için `stats`istatistik bilgilerinin döndürülmesi gerektiğini belirtir: .

> [!NOTE]
> Üç sorgu dize türünden herhangi birini (filtreleme, seçin ve genişletin) yaparken, özellik adlarının ve büyük/küçük harf adlarının REST API öğesi karşıtlarıyla eşleştiğinden emin olmalısınız. Örneğin, .NET [CloudTask](/dotnet/api/microsoft.azure.batch.cloudtask) sınıfıyla çalışırken, .NET özelliği [CloudTask.State](/dotnet/api/microsoft.azure.batch.cloudtask.state#Microsoft_Azure_Batch_CloudTask_State)olsa bile **Durum**yerine **durum** belirtmeniz gerekir. .NET ve REST API'leri arasındaki özellik eşlemeleri için aşağıdaki tablolara bakın.
> 
> 

### <a name="rules-for-filter-select-and-expand-strings"></a>Dizeleri filtreleme, seçme ve genişletme kuralları
* [Toplu İşlem .NET'i][api_net] veya diğer Toplu İşlem SDK'lardan birini kullandığınızda bile, filtre, seçme ve genişletme dizelerinde özellik adları [Toplu REST][api_rest] API'sinde olduğu gibi görünmelidir.
* Tüm özellik adları büyük/küçük harf duyarlıdır, ancak özellik değerleri büyük/küçük harf duyarlıdır.
* Tarih/saat dizeleri iki biçimden biri olabilir ve 'den önce .'den `DateTime`önce olmalıdır.
  
  * W3C-DTF formatı örneği:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  * RFC 1123 format örneği:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
* Boolean dizeleri `true` ya `false`da .
* Geçersiz bir özellik veya işleç `400 (Bad Request)` belirtilirse, bir hata ortaya çıkacaktır.

## <a name="efficient-querying-in-batch-net"></a>Toplu İşlem .NET'te verimli sorgulama
Toplu [İşlem .NET][api_net] API'sinde, [ODATADetailLevel][odata] sınıfı filtre sağlamak, dizeleri listeişlemlerine genişletmek ve genişletmek için kullanılır. ODataDetailLevel sınıfı, oluşturucuda belirtilebilen veya doğrudan nesneüzerinde ayarlanabilen üç ortak dize özelliğine sahiptir. Daha sonra [ListPools,][net_list_pools] [ListJobs][net_list_jobs]ve [ListTasks][net_list_tasks]gibi çeşitli liste işlemleri için bir parametre olarak ODataDetailLevel nesne geçmek.

* [ODATADetailLevel][odata]. [FilterClause][odata_filter]: Döndürülen madde sayısını sınırlayın.
* [ODATADetailLevel][odata]. [SelectClause][odata_select]: Her maddeyle hangi özellik değerlerinin döndürüldünü belirtin.
* [ODATADetailLevel][odata]. [ExpandClause][odata_expand]: Her öğe için ayrı çağrılar yerine tek bir API çağrısındaki tüm öğeler için veri alın.

Aşağıdaki kod snippet, belirli bir havuz kümesinin istatistikleri için Toplu İşlem hizmetini verimli bir şekilde sorgulamak için Toplu İşlem .NET API'sini kullanır. Bu senaryoda, Toplu İşlem kullanıcısının hem test hem de üretim havuzları vardır. Test havuzu idiler "test" ile önceden belirlenmiş ve üretim havuzu idiler "prod" ile önceden belirlenmiştir. Snippet'te, *myBatchClient,* [BatchClient](/dotnet/api/microsoft.azure.batch.batchclient) sınıfının düzgün bir şekilde başlatılmasını öntür.

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [!TIP]
> Select ve Expand yan tümceleriyle yapılandırılan [ODATADetailLevel'in][odata] bir örneği, döndürülen veri miktarını sınırlamak için [PoolOperations.GetPool](/dotnet/api/microsoft.azure.batch.pooloperations.getpool#Microsoft_Azure_Batch_PoolOperations_GetPool_System_String_Microsoft_Azure_Batch_DetailLevel_System_Collections_Generic_IEnumerable_Microsoft_Azure_Batch_BatchClientBehavior__)gibi uygun Get yöntemlerine de aktarılabilir.
> 
> 

## <a name="batch-rest-to-net-api-mappings"></a>Toplu REST .NET API eşlemeleri için
Filtre, seçme ve genişletme dizelerindeki özellik adları, hem ad hem de büyük/küçük harf olarak REST API karşılıklarını *yansıtmalıdır.* Aşağıdaki tablolar .NET ve REST API karşılıkları arasında eşlemeler sağlar.

### <a name="mappings-for-filter-strings"></a>Filtre dizeleri için eşlemeler
* **.NET list yöntemleri**: Bu sütundaki .NET API yöntemlerinin her biri [odatadetaillevel][odata] nesnesini parametre olarak kabul eder.
* **REST list istekleri**: Bu sütuna bağlı her REST API sayfası, *filtre* dizelerinde izin verilen özellikleri ve işlemleri belirten bir tablo içerir. [Bir ODATADetailLevel.FilterClause][odata_filter] dizesi oluşturup bu özellik adlarını ve işlemlerini kullanırsınız.

| .NET liste yöntemleri | REST listesi istekleri |
| --- | --- |
| [Sertifikaİşlemleri.ListCertificates][net_list_certs] |[Sertifikaları bir hesapta listele][rest_list_certs] |
| [CloudTask.ListNodeFiles][net_list_task_files] |[Görevle ilişkili dosyaları listelama][rest_list_task_files] |
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] |[Bir iş için iş hazırlama ve iş bırakma görevlerinin durumunu listele][rest_list_jobprep_status] |
| [İş İşlemleri.ListJobs][net_list_jobs] |[Bir hesaptaki işleri listele][rest_list_jobs] |
| [JobOperations.ListNodeFiles][net_list_nodefiles] |[Düğümdeki dosyaları listele][rest_list_nodefiles] |
| [İş İşlemleri.ListGörevleri][net_list_tasks] |[Bir işle ilişkili görevleri listele][rest_list_tasks] |
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] |[Bir hesaptaki iş zamanlamalarını listele][rest_list_job_schedules] |
| [İşÇizelgesiİşlemleri.ListJobs][net_list_schedule_jobs] |[İş zamanlamasıyla ilişkili işleri listele][rest_list_schedule_jobs] |
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] |[Bir havuzdaki işlem düğümlerini listele][rest_list_compute_nodes] |
| [PoolOperations.ListPools][net_list_pools] |[Bir hesaptaki havuzları listele][rest_list_pools] |

### <a name="mappings-for-select-strings"></a>Seçili dizeleri için eşlemeler
* **Toplu .NET türleri**: Toplu .NET API türleri.
* **REST API varlıkları**: Bu sütundaki her sayfa, türüne ait REST API özellik adlarını listeleyen bir veya daha fazla tablo içerir. Bu özellik *adları, seçili* dizeleri oluşturabilirsiniz kullanılır. [Bir ODATADetailLevel.SelectClause][odata_select] dizesi oluşturacağınız zaman bu aynı özellik adlarını kullanırsınız.

| Toplu .NET türleri | REST API varlıkları |
| --- | --- |
| [Sertifika][net_cert] |[Sertifika hakkında bilgi alın][rest_get_cert] |
| [CloudJob][net_job] |[Bir iş hakkında bilgi alın][rest_get_job] |
| [CloudJobSchedule][net_schedule] |[İş zamanlamasını zedebedin][rest_get_schedule] |
| [İşlemoDüğüm][net_node] |[Düğüm hakkında bilgi alın][rest_get_node] |
| [CloudPool][net_pool] |[Havuz hakkında bilgi alın][rest_get_pool] |
| [CloudTask][net_task] |[Görev hakkında bilgi alın][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Örnek: bir filtre dizesi oluşturma
[ODATADetailLevel.FilterClause][odata_filter]için bir filtre dizesi oluşturup, gerçekleştirmek istediğiniz liste işlemine karşılık gelen REST API dokümantasyon sayfasını bulmak için yukarıdaki tabloya "Filtre dizeleri eşlemeleri" altında danışın. Filtrelenebilir özellikleri ve desteklenen işleçleri bu sayfada ilk çok satırlı tabloda bulabilirsiniz. Çıkış kodu sıfır olmayan tüm görevleri almak isterseniz, örneğin, bu satır [Liste'deki bir işle ilişkili görevleri][rest_list_tasks] geçerli özellik dizesi ve izin verilebilen işleçleri belirtir:

| Özellik | İzin verilen işlemler | Tür |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

Böylece, sıfır olmayan bir çıkış kodu ile tüm görevleri listelemek için filtre dizesi olacaktır:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Örnek: seçili bir dize oluşturma
[ODATADetailLevel.SelectClause'u][odata_select]oluşturmak için yukarıdaki tabloya "Seçili dizeler için eşlemeler" altında danışın ve listelediğiniz varlık türüne karşılık gelen REST API sayfasına gidin. Seçilebilir özellikleri ve desteklenen işleçleri bu sayfada ilk çok satırlı tabloda bulabilirsiniz. Örneğin, bir listedeki her görev için yalnızca kimlik ve komut satırını almak isterseniz, bu satırları bir [görev hakkında bilgi al'daki][rest_get_task]ilgili tabloda bulabilirsiniz:

| Özellik | Tür | Notlar |
|:--- |:--- |:--- |
| `id` |`String` |`The ID of the task.` |
| `commandLine` |`String` |`The command line of the task.` |

Listelenen her göreve yalnızca kimlik ve komut satırını dahil etmek için seçili dize aşağıdakileri olacaktır:

`id, commandLine`

## <a name="code-samples"></a>Kod örnekleri
### <a name="efficient-list-queries-code-sample"></a>Verimli liste sorguları kod örneği
Liste sorgusunun bir uygulamadaki performansı ne kadar etkili etkileyebileceğini görmek için GitHub'daki [EfficientListQueries][efficient_query_sample] örnek projesine göz atın. Bu C# konsolu uygulaması bir işe çok sayıda görev oluşturur ve ekler. Daha sonra, [JobOperations.ListTasks][net_list_tasks] yöntemine birden çok arama yapar ve döndürülecek veri miktarını değiştirmek için farklı özellik değerleriyle yapılandırılan [ODATADetailLevel][odata] nesnelerini geçer. Aşağıdakilere benzer çıktı üretir:

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Geçen zamanlarda gösterildiği gibi, özellikleri ve döndürülen öğe sayısını sınırlayarak sorgu yanıt sürelerini büyük ölçüde düşürebilirsiniz. Bu ve diğer örnek projeleri GitHub'daki [azure toplu toplu örnek][github_samples] deposunda bulabilirsiniz.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics kitaplığı ve kod örneği
Yukarıdaki EfficientListQueries kod örneğine ek olarak, [Toplu İşlemler][batch_metrics] projesini [azure toplu örnekleri][github_samples] GitHub deposunda bulabilirsiniz. BatchMetrics örnek projesi, Toplu İş API'sini kullanarak Azure Toplu iş ilerlemesini verimli bir şekilde nasıl izleyeceğimi gösterir.

[BatchMetrics][batch_metrics] örneği, kendi projelerinize dahil edebileceğiniz bir .NET sınıfı kitaplık projesi ve kitaplığın kullanımını uygulamak ve göstermek için basit bir komut satırı programı içerir.

Proje içindeki örnek uygulama aşağıdaki işlemleri gösterir:

1. Yalnızca ihtiyacınız olan özellikleri indirmek için belirli öznitelikleri seçme
2. Son sorgudan bu yana yalnızca değişiklikleri indirmek için durum geçiş sürelerine filtre uygulama

Örneğin, aşağıdaki yöntem BatchMetrics kitaplığında görünür. Sorgulanan varlıklar için yalnızca özelliklerin ve `id` `state` özelliklerin alınması gerektiğini belirten bir ODATADetailDüzey döndürür. Ayrıca, yalnızca belirtilen `DateTime` parametreden sonra durumu değişen varlıkların döndürülmesi gerektiğini belirtir.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Sonraki adımlar
### <a name="parallel-node-tasks"></a>Paralel düğüm görevleri
[Eşzamanlı düğüm görevleri ile Azure Toplu işlem kaynak kullanımını en üst düzeye](batch-parallel-node-tasks.md) çıkarmak Toplu uygulama performansı ile ilgili başka bir makaledir. Bazı iş yükü türleri, daha büyük ancak daha az bilgi işlem düğümlerinde paralel görevlerin yürütülmesinden yararlanabilir. Böyle bir senaryoyla ilgili ayrıntılar için makaledeki [örnek senaryoya](batch-parallel-node-tasks.md#example-scenario) göz atın.


[api_net]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: https://docs.microsoft.com/rest/api/batchservice/
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

[rest_get_task_counts]: /rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_node_counts]: /rest/api/batchservice/account/listpoolnodecounts
