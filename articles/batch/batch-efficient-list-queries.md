---
title: Etkili liste sorguları tasarlama
description: Havuzlar, işler, görevler ve işlem düğümleri gibi Batch kaynakları hakkında bilgi isteğinde bulunduğunuzu filtreleyerek performansı artırın.
ms.topic: how-to
ms.date: 06/18/2020
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 3a767cc8ae3c8c48e1e40e0735c33fa807ba0015
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88933524"
---
# <a name="create-queries-to-list-batch-resources-efficiently"></a>Toplu Işlem kaynaklarını etkili bir şekilde listelemek için sorgular oluşturma

Neredeyse tüm Batch uygulamalarının, genellikle düzenli aralıklarla Batch hizmetini sorgulayan bir tür izleme veya başka işlem gerçekleştirmesi gerekir. Örneğin, bir işte kalan sıraya alınmış görevler olup olmadığını anlamak için, işteki her görevde veri almanız gerekir. Havuzunuzdaki düğümlerin durumunu öğrenmek için, havuzdaki her düğümde veri almanız gerekir. Bu makalede, bu tür sorguların en verimli şekilde nasıl yürütüleceği açıklanmaktadır.

[Batch .net](/dotnet/api/microsoft.azure.batch) kitaplığı ile işleri, görevleri, işlem düğümlerini ve diğer kaynakları sorguladığınızda, hizmet tarafından döndürülen veri miktarını azaltarak Azure Batch uygulamanızın performansını artırabilirsiniz.

> [!NOTE]
> Batch hizmeti, bir işteki görevleri saymakta olan ortak senaryolar ve Batch havuzundaki işlem düğümlerini saymak için API desteği sağlar. Bunlar için bir liste sorgusu kullanmak yerine, [görev sayısını Al](/rest/api/batchservice/job/gettaskcounts) ve [Havuz düğüm sayılarını Listele](/rest/api/batchservice/account/listpoolnodecounts) işlemlerini çağırabilirsiniz. Bu işlemler liste sorgusundan daha verimlidir, ancak her zaman güncel olmayan daha sınırlı bilgiler döndürür. Daha fazla bilgi için bkz. [görevleri ve işlem düğümlerini duruma göre sayma](batch-get-resource-counts.md).

## <a name="specify-a-detail-level"></a>Ayrıntı düzeyi belirtin

Bir üretim toplu Iş uygulamasında, işler, görevler ve işlem düğümleri gibi varlıklar binlerce sayı olabilir. Bu kaynaklarla ilgili bilgileri istediğinizde, potansiyel olarak büyük miktarda veri, Batch hizmetinden her bir sorgudaki uygulamanıza "tel çapraz" olmalıdır. Bir sorgu tarafından döndürülen öğelerin sayısını ve bilgi türünü sınırlayarak, sorgularınızın hızını ve bu nedenle uygulamanızın performansını artırabilirsiniz.

Bu [Batch .net](/dotnet/api/microsoft.azure.batch) API kod parçacığı, her görevin *Tüm* özellikleriyle birlikte bir işle ilişkili *her* görevi listeler:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Bununla birlikte, sorgunuza bir "ayrıntı düzeyi" uygulayarak daha verimli bir liste sorgusu gerçekleştirebilirsiniz. Bunu, [JobOperations. ListTasks](/dotnet/api/microsoft.azure.batch.joboperations) yöntemine bir [Odatadetaillevel](/dotnet/api/microsoft.azure.batch.odatadetaillevel) nesnesi sağlayarak yapabilirsiniz. Bu kod parçacığı yalnızca, tamamlanan görevlerin KIMLIK, komut satırı ve işlem düğümü bilgi özelliklerini döndürür:

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

Bu örnek senaryoda, işte binlerce görev varsa, ikinci sorgudaki sonuçlar genellikle birinciden çok daha hızlı döndürülür. Batch .NET API 'SI ile öğeleri listeettiğiniz zaman ODATADetailLevel kullanımı hakkında daha fazla bilgi [aşağıda](#efficient-querying-in-batch-net)verilmiştir.

> [!IMPORTANT]
> Uygulamanızın en yüksek verimliliğini ve performansını sağlamak için .NET API listesi çağrılarınız için her zaman bir ODATADetailLevel nesnesi vermenizi öneririz. Ayrıntı düzeyi belirterek, Batch hizmeti yanıt sürelerini azaltmaya, ağ kullanımını iyileştirmenize ve istemci uygulamaları tarafından bellek kullanımını en aza indirmenize yardımcı olabilirsiniz.

## <a name="filter-select-and-expand"></a>Filtrele, seç ve Genişlet

[Batch .net](/dotnet/api/microsoft.azure.batch) ve [Batch Rest](/rest/api/batchservice/) API 'leri, bir listede döndürülen öğelerin sayısını ve her biri için döndürülen bilgi miktarını azaltmaya olanak sağlar. Bunu, liste sorguları gerçekleştirirken **filtre**, **seçme**ve **dizeleri Genişlet** ' i belirterek yapabilirsiniz.

### <a name="filter"></a>Filtre

Filtre dizesi döndürülen öğelerin sayısını azaltan bir ifadedir. Örneğin, yalnızca bir iş için çalışan görevleri listeleyebilir veya yalnızca görevleri çalıştırmaya hazırlamış olan işlem düğümlerini listeleyebilirsiniz.

Filtre dizesi bir veya daha fazla ifadeden oluşur ve bir özellik adı, işleç ve değer içeren bir ifade içerir. Belirtime özellikler, her bir özellik için desteklenen işleçler gibi, Sorgulayabileceğiniz her bir varlık türüne özeldir.  Birden çok ifade mantıksal işleçler ve kullanılarak birleştirilebilir `and` `or` .

Bu örnek filtre dizesinde yalnızca çalışan "render" görevleri listelenir: `(state eq 'running') and startswith(id, 'renderTask')` .

### <a name="select"></a>Şunu seçin:

Select String her öğe için döndürülen özellik değerlerini sınırlandırır. Virgülle ayrılmış özellik adlarının bir listesini belirtirsiniz ve sorgu sonuçlarındaki öğeler için yalnızca bu özellik değerleri döndürülür. Sorgulamakta olduğunuz varlık türü için özelliklerden herhangi birini belirtebilirsiniz.

Bu örnek Select String, her görev için yalnızca üç özellik değerinin döndürülüp döndürülmeyeceğini belirtir: `id, state, stateTransitionTime` .

### <a name="expand"></a>Genişlet

Expand dize, belirli bilgileri almak için gerekli olan API çağrılarının sayısını azaltır. Bir genişletme dizesi kullandığınızda her öğe hakkında daha fazla bilgi tek bir API çağrısıyla elde edilebilir. İlk olarak, varlıkların listesini almak ve sonra listedeki her öğe için bilgi istemek yerine, tek bir API çağrısında aynı bilgileri elde etmek için bir genişletme dizesi kullanır ve bu da API çağrılarını azaltarak performansı artırmaya yardımcı olur.

Select dizesine benzer şekilde, genişletme dizesi belirli verilerin liste sorgu sonuçlarına dahil edilip edilmeyeceğini denetler. Tüm özellikler gerekli olduğunda ve Select String belirtilmediğinde, istatistik bilgilerini almak için *genişletme dizesi kullanılmalıdır* . Bir SELECT dizesi, özelliklerin bir alt kümesini almak için kullanılırsa, `stats` Select dizesinde belirtilebilir ve genişletme dizesinin belirtilmesi gerekmez.

Expand dize yalnızca liste işleri, iş zamanlamaları, görevler ve havuzlar üzerinde kullanıldığında desteklenir. Şu anda yalnızca istatistik bilgilerini destekler.

Bu örnek dize genişletme, listedeki her öğe için İstatistik bilgilerinin döndürülmesini belirtir: `stats` .

> [!NOTE]
> Üç sorgu dizesi türünden birini oluştururken (Filter, Select ve Expand), özellik adlarının ve durumunun REST API öğelerinin karşılıklarıyla eşleştiğinden emin olmanız gerekir. Örneğin, .NET [cloudtask](/dotnet/api/microsoft.azure.batch.cloudtask) sınıfıyla çalışırken, .NET özelliği [Cloudtask. State](/dotnet/api/microsoft.azure.batch.cloudtask.state#Microsoft_Azure_Batch_CloudTask_State)olsa **bile durum yerine** **State** belirtmeniz gerekir. .NET ve REST API 'Ler arasındaki özellik eşlemeleri için aşağıdaki tablolara bakın.

### <a name="rules-for-filter-select-and-expand-strings"></a>Filter, Select ve Expand dizeleri için kurallar

- Filtre içindeki Özellikler adları, Select ve Genişlet dizeleri Batch [.net](/dotnet/api/microsoft.azure.batch) veya diğer Batch SDK 'larından birini kullandığınızda bile, [yığın Rest](/rest/api/batchservice/) API 'sinde olduğu gibi görünmelidir.
- Tüm özellik adları büyük/küçük harfe duyarlıdır, ancak özellik değerleri büyük/küçük harfe duyarsızdır.
- Tarih/saat dizeleri iki biçimden biri olabilir ve önünde olmalıdır `DateTime` .
  
  - W3C-DTF biçim örneği: `creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - RFC 1123 biçim örneği: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Boolean dizeleri ya da `true` `false` .
- Geçersiz bir özellik veya işleç belirtilirse, bir `400 (Bad Request)` hata ortaya kaydedilir.

## <a name="efficient-querying-in-batch-net"></a>Batch .NET 'te verimli sorgulama

[Batch .net](/dotnet/api/microsoft.azure.batch) API 'si Içinde, [Odatadetaillevel](/dotnet/api/microsoft.azure.batch.odatadetaillevel) sınıfı filtre sağlamak için kullanılır, seçme ve dizeleri listelemek için dizeleri genişletme. ODataDetailLevel sınıfı, oluşturucuda belirtime üç genel dize özelliklerine sahiptir veya doğrudan nesne üzerinde ayarlanır. Ardından, ODataDetailLevel nesnesini [Listpools](/dotnet/api/microsoft.azure.batch.pooloperations), [ListJobs](/dotnet/api/microsoft.azure.batch.joboperations)ve [ListTasks](/dotnet/api/microsoft.azure.batch.joboperations)gibi çeşitli liste işlemlerine bir parametre olarak geçitirsiniz.

- [Odatadetaillevel. FilterClause](/dotnet/api/microsoft.azure.batch.odatadetaillevel.filterclause): döndürülen öğelerin sayısını sınırlayın.
- [Odatadetaillevel. SelectClause](/dotnet/api/microsoft.azure.batch.odatadetaillevel.selectclause): her öğe ile hangi özellik değerlerinin döndürüleceğini belirtin.
- [Odatadetaillevel. ExpandClause](/dotnet/api/microsoft.azure.batch.odatadetaillevel.expandclause): her öğe için ayrı çağrılar yerine tek bir API çağrısındaki tüm öğeler için veri alın.

Aşağıdaki kod parçacığı, Batch hizmetini belirli bir havuz kümesinin istatistikleri için etkin bir şekilde sorgulamak üzere Batch .NET API 'sini kullanır. Bu senaryoda Batch kullanıcısının hem test hem de üretim havuzları vardır. Test havuzu kimlikleri "test" önekini alır ve üretim havuzu kimlikleri "prod" önekini alır. Kod parçacığında, *Mybatchclient* , [batchclient](/dotnet/api/microsoft.azure.batch.batchclient) sınıfının düzgün başlatılmış bir örneğidir.

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
> Select ve expand yan tümceleri ile yapılandırılan [Odatadetaillevel](/dotnet/api/microsoft.azure.batch.odatadetaillevel) örneği, döndürülen veri miktarını sınırlamak Için [Pooloperations. getpool](/dotnet/api/microsoft.azure.batch.pooloperations.getpool#Microsoft_Azure_Batch_PoolOperations_GetPool_System_String_Microsoft_Azure_Batch_DetailLevel_System_Collections_Generic_IEnumerable_Microsoft_Azure_Batch_BatchClientBehavior__)gibi uygun alma yöntemlerine de geçirilebilir.

## <a name="batch-rest-to-net-api-mappings"></a>Toplu iş geri kalanı .NET API eşlemeleri

Filtre içindeki Özellik adları, Select ve Genişlet dizeleri, hem ad hem de durum ' da REST API karşılamalarını yansıtmalıdır. Aşağıdaki tablolar, .NET ve REST API karşılıkları arasında eşlemeler sağlar.

### <a name="mappings-for-filter-strings"></a>Filtre dizeleri için eşlemeler

- **.Net liste yöntemleri**: Bu SÜTUNDAKI .NET API yöntemlerinin her biri, bir [Odatadetaillevel](/dotnet/api/microsoft.azure.batch.odatadetaillevel) nesnesini parametre olarak kabul eder.
- **Rest liste istekleri**: Bu sütunda bağlantılı her bir REST API sayfası, *filtre* dizelerinde izin verilen özellikleri ve işlemleri belirten bir tablo içerir. Bu özellik adlarını ve işlemlerini bir [Odatadetaillevel. FilterClause](/dotnet/api/microsoft.azure.batch.odatadetaillevel.filterclause) dizesi oluştururken kullanırsınız.

| .NET liste yöntemleri | REST liste istekleri |
| --- | --- |
| [CertificateOperations. ListCertificates](/dotnet/api/microsoft.azure.batch.certificateoperations) |[Bir hesaptaki sertifikaları listeleme](/rest/api/batchservice/certificate/list) |
| [CloudTask. ListNodeFiles](/dotnet/api/microsoft.azure.batch.cloudtask) |[Bir görevle ilişkili dosyaları listeleme](/rest/api/batchservice/file/listfromtask) |
| [JobOperations. ListJobPreparationAndReleaseTaskStatus](/dotnet/api/microsoft.azure.batch.joboperations) |[İş için iş hazırlama ve iş bırakma görevlerinin durumunu listeleyin](/rest/api/batchservice/job/listpreparationandreleasetaskstatus) |
| [JobOperations. ListJobs](/dotnet/api/microsoft.azure.batch.joboperations) |[Bir hesaptaki işleri listeleyin](/rest/api/batchservice/job/list) |
| [JobOperations. ListNodeFiles](/dotnet/api/microsoft.azure.batch.joboperations) |[Bir düğümdeki dosyaları listeleme](/rest/api/batchservice/file/listfromcomputenode) |
| [JobOperations. ListTasks](/dotnet/api/microsoft.azure.batch.joboperations) |[Bir işle ilişkili görevleri listeleyin](/rest/api/batchservice/task/list) |
| [JobScheduleOperations. Listjobzamanlamalar](/dotnet/api/microsoft.azure.batch.jobscheduleoperations) |[Bir hesaptaki iş zamanlamalarını listeleme](/rest/api/batchservice/jobschedule/list) |
| [JobScheduleOperations. ListJobs](/dotnet/api/microsoft.azure.batch.jobscheduleoperations) |[İş zamanlamasıyla ilişkili işleri listeleyin](/rest/api/batchservice/job/listfromjobschedule) |
| [PoolOperations. ListComputeNodes](/dotnet/api/microsoft.azure.batch.pooloperations) |[Bir havuzdaki işlem düğümlerini listeleme](/rest/api/batchservice/computenode/list) |
| [PoolOperations. ListPools](/dotnet/api/microsoft.azure.batch.pooloperations) |[Bir hesaptaki havuzları listeleme](/rest/api/batchservice/pool/list) |

### <a name="mappings-for-select-strings"></a>Seçme dizeleri için eşlemeler

- **Batch .net türleri**: Batch .NET API türleri.
- **REST API varlıklar**: Bu sütundaki her sayfa, türün REST API özellik adlarını listeeden bir veya daha fazla tablo içerir. Bu özellik adları, *Select* dizeleri oluşturduğunuzda kullanılır. Bir [Odatadetaillevel. SelectClause](/dotnet/api/microsoft.azure.batch.odatadetaillevel.selectclause) dizesi oluştururken aynı özellik adlarını kullanırsınız.

| Batch .NET türleri | REST API varlıkları |
| --- | --- |
| [Sertifika](/dotnet/api/microsoft.azure.batch.certificate) |[Bir sertifika hakkında bilgi edinme](/rest/api/batchservice/certificate/get) |
| [CloudJob](/dotnet/api/microsoft.azure.batch.cloudjob) |[İş hakkında bilgi alın](/rest/api/batchservice/job/get) |
| [CloudJobSchedule](/dotnet/api/microsoft.azure.batch.cloudjobschedule) |[İş zamanlaması hakkında bilgi alın](/rest/api/batchservice/jobschedule/get) |
| [ComputeNode](/dotnet/api/microsoft.azure.batch.computenode) |[Bir düğüm hakkında bilgi edinme](/rest/api/batchservice/computenode/get) |
| [CloudPool](/dotnet/api/microsoft.azure.batch.cloudpool) |[Havuz hakkında bilgi edinme](/rest/api/batchservice/pool/get) |
| [CloudTask](/dotnet/api/microsoft.azure.batch.cloudtask) |[Bir görev hakkında bilgi edinme](/rest/api/batchservice/task/get) |

## <a name="example-construct-a-filter-string"></a>Örnek: bir filtre dizesi oluşturun

[Odatadetaillevel. FilterClause](/dotnet/api/microsoft.azure.batch.odatadetaillevel.filterclause)için bir filtre dizesi oluşturduğunuzda, gerçekleştirmek istediğiniz liste işlemine karşılık gelen REST API belge sayfasını bulmak için yukarıdaki "Filtre dizeleri için eşlemeler" altındaki tabloya başvurun. Filtrelenebilir özellikleri ve bu sayfadaki ilk multirow tablosunda desteklenen işleçlerini bulacaksınız. Çıkış kodu sıfır olmayan tüm görevleri almak isterseniz, örneğin, [bir işle ilişkili görevleri listede](/rest/api/batchservice/task/list) bu satır ilgili özellik dizesini ve izin verilen işleçleri belirler:

| Özellik | İzin verilen işlemler | Tür |
|:--- |:--- |:--- |
| `executionInfo/exitCode` |`eq, ge, gt, le , lt` |`Int` |

Bu nedenle, sıfır dışında çıkış koduna sahip tüm görevleri listelemek için filtre dizesi şöyle olur:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Örnek: Select String oluşturun

[Odatadetaillevel. SelectClause](/dotnet/api/microsoft.azure.batch.odatadetaillevel.selectclause)oluşturmak için, "dizeleri seç için eşlemeler" altındaki tabloya bakın ve listelerken varlık türüne karşılık gelen REST API sayfasına gidin. Seçilebilir özellikleri ve bu sayfadaki ilk multirow tablosunda desteklenen işleçlerini bulacaksınız. Bir listedeki her bir görev için yalnızca KIMLIĞI ve komut satırını almak istiyorsanız, [bir görevle ilgili bilgi edinmek](/rest/api/batchservice/task/get)için bu satırları uygulanabilir tabloda bulabilirsiniz:

| Özellik | Tür | Notlar |
|:--- |:--- |:--- |
| `id` |`String` |`The ID of the task.` |
| `commandLine` |`String` |`The command line of the task.` |

Yalnızca listelenen her görevle ilgili KIMLIĞI ve komut satırını dahil etmek için seçim dizesi şöyle olacaktır:

`id, commandLine`

## <a name="code-samples"></a>Kod örnekleri

### <a name="efficient-list-queries-code-sample"></a>Etkili liste sorguları kod örneği

GitHub 'daki [verimlilik Entlistqueries](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries) örnek projesi bir uygulamadaki performansı ne kadar etkili bir şekilde etkilediğini gösterir. Bu C# konsol uygulaması, bir işe çok sayıda görev oluşturur ve ekler. Daha sonra, [JobOperations. ListTasks](/dotnet/api/microsoft.azure.batch.joboperations) yöntemine birden çok çağrı yapar ve döndürülecek veri miktarını değiştirmek için farklı özellik değerleriyle yapılandırılan [Odatadetaillevel](/dotnet/api/microsoft.azure.batch.odatadetaillevel) nesnelerini geçirir. Aşağıdakine benzer bir çıktı üretir:

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

Geçen zamanlarda gösterildiği gibi, özellikleri ve döndürülen öğelerin sayısını sınırlayarak sorgu yanıt sürelerini önemli ölçüde düşürebilirsiniz. Bu ve diğer örnek projeleri GitHub 'daki [Azure-Batch-Samples](https://github.com/Azure-Samples/azure-batch-samples) deposunda bulabilirsiniz.

### <a name="batchmetrics-library-and-code-sample"></a>Batchölçümler kitaplığı ve kod örneği

Yukarıdaki Verimlilientlistqueries kod örneğine ek olarak, [Batchölçümler](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp/BatchMetrics) örnek projesi Batch API 'si kullanılarak Azure Batch iş ilerlemesini verimli bir şekilde nasıl izleyeceğinizi gösterir.

[Batchölçümler](https://github.com/Azure-Samples/azure-batch-samples/tree/master/CSharp/BatchMetrics) örneği, kendi projelerinizle birleştirebileceğiniz bir .NET sınıf kitaplığı projesi ve kitaplığın kullanımını sağlamak ve göstermek için basit bir komut satırı programı içerir.

Proje içindeki örnek uygulama aşağıdaki işlemleri gösterir:

1. Yalnızca ihtiyacınız olan özellikleri indirmek için belirli öznitelikleri seçme
2. Son sorgudan bu yana yalnızca değişiklikleri indirmek için durum geçiş süreleriyle filtreleme

Örneğin, aşağıdaki yöntem Batchölçümlerini kitaplığında görüntülenir. `id` `state` Sorgulanan varlıklar için yalnızca ve özelliklerinin alınması gerektiğini belirten bir ODATADetailLevel döndürür. Ayrıca, belirtilen parametrenin döndürülmesinden sonra yalnızca durumu değişmiş olan varlıkların de belirtir `DateTime` .

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

- [Eşzamanlı düğüm görevleriyle Azure Batch işlem kaynak kullanımını en üst düzeye çıkarmayı](batch-parallel-node-tasks.md)öğrenin. Bazı iş yükü türleri, paralel görevleri daha büyük (ancak daha az) işlem düğümlerinde yürütmenin avantajlarından yararlanabilir. Bu tür bir senaryoya ilişkin ayrıntılar için makalesindeki [Örnek senaryoya](batch-parallel-node-tasks.md#example-scenario) göz atın.
- [Görevleri ve düğümleri duruma göre sayarak Batch çözümlerini izlemeyi](batch-get-resource-counts.md) öğrenin


[api_net]: /dotnet/api/microsoft.azure.batch
[api_net_listjobs]: /dotnet/api/microsoft.azure.batch.joboperations
[api_rest]: /rest/api/batchservice/
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: /dotnet/api/microsoft.azure.batch.odatadetaillevel
[odata_ctor]: /dotnet/api/microsoft.azure.batch.odatadetaillevel
[odata_expand]: /dotnet/api/microsoft.azure.batch.odatadetaillevel
[odata_filter]: /dotnet/api/microsoft.azure.batch.odatadetaillevel
[odata_select]: /dotnet/api/microsoft.azure.batch.odatadetaillevel

[net_list_certs]: /dotnet/api/microsoft.azure.batch.certificateoperations
[net_list_compute_nodes]: /dotnet/api/microsoft.azure.batch.pooloperations
[net_list_job_schedules]: /dotnet/api/microsoft.azure.batch.jobscheduleoperations
[net_list_jobprep_status]: /dotnet/api/microsoft.azure.batch.joboperations
[net_list_jobs]: /dotnet/api/microsoft.azure.batch.joboperations
[net_list_nodefiles]: /dotnet/api/microsoft.azure.batch.joboperations
[net_list_pools]: /dotnet/api/microsoft.azure.batch.pooloperations
[net_list_schedule_jobs]: /dotnet/api/microsoft.azure.batch.jobscheduleoperations
[net_list_task_files]: /dotnet/api/microsoft.azure.batch.cloudtask
[net_list_tasks]: /dotnet/api/microsoft.azure.batch.joboperations

[rest_list_certs]: /rest/api/batchservice/certificate/list
[rest_list_compute_nodes]: /rest/api/batchservice/computenode/list
[rest_list_job_schedules]: /rest/api/batchservice/jobschedule/list
[rest_list_jobprep_status]: /rest/api/batchservice/job/listpreparationandreleasetaskstatus
[rest_list_jobs]: /rest/api/batchservice/job/list
[rest_list_nodefiles]: /rest/api/batchservice/file/listfromcomputenode
[rest_list_pools]: /rest/api/batchservice/pool/list
[rest_list_schedule_jobs]: /rest/api/batchservice/job/listfromjobschedule
[rest_list_task_files]: /rest/api/batchservice/file/listfromtask
[rest_list_tasks]: /rest/api/batchservice/task/list

[rest_get_cert]: /rest/api/batchservice/certificate/get
[rest_get_job]: /rest/api/batchservice/job/get
[rest_get_node]: /rest/api/batchservice/computenode/get
[rest_get_pool]: /rest/api/batchservice/pool/get
[rest_get_schedule]: /rest/api/batchservice/jobschedule/get
[rest_get_task]: /rest/api/batchservice/task/get

[net_cert]: /dotnet/api/microsoft.azure.batch.certificate
[net_job]: /dotnet/api/microsoft.azure.batch.cloudjob
[net_node]: /dotnet/api/microsoft.azure.batch.computenode
[net_pool]: /dotnet/api/microsoft.azure.batch.cloudpool
[net_schedule]: /dotnet/api/microsoft.azure.batch.cloudjobschedule
[net_task]: /dotnet/api/microsoft.azure.batch.cloudtask

[rest_get_task_counts]: /rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_node_counts]: /rest/api/batchservice/account/listpoolnodecounts
