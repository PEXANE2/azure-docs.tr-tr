---
title: Azure Storage 'ı izleme | Microsoft Docs
description: Azure depolama 'nın performansını ve kullanılabilirliğini izlemeyi öğrenin.
author: normesta
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: normesta
ms.reviewer: fryu
ms.custom: monitoring
ms.openlocfilehash: c8125001b5960a0bf770e8e015ad757a277629ea
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684894"
---
# <a name="monitor-azure-storage"></a>Azure Storage 'ı izleme

Azure kaynaklarına dayanan önemli uygulamalarınız ve iş süreçleriniz olduğunda, bu kaynakları kullanılabilirlik, performans ve işlem için izlemek istersiniz. Bu makalede, Azure depolama tarafından oluşturulan izleme verileri ve bu verilerdeki uyarıları çözümlemek için Azure Izleyici 'nin özelliklerini nasıl kullanabileceğiniz açıklanır.

> [!NOTE]
> Azure Izleyici 'de Azure depolama günlükleri genel önizleme aşamasındadır ve tüm genel bulut bölgelerinde önizleme testi için kullanılabilir. Önizlemeye kaydolmak için [Bu sayfaya](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u)bakın. Bu önizleme, blob 'lar (Azure Data Lake Storage 2.), dosyalar, kuyruklar, tablolar, genel amaçlı v1 için Premium Depolama hesapları ve genel amaçlı v2 depolama hesapları için günlük Günlükler sunar. Klasik depolama hesapları desteklenmez.

## <a name="monitor-overview"></a>İzlemeye genel bakış

Her depolama kaynağı için Azure portal **genel bakış** sayfası, istekler ve saatlik faturalandırma gibi kaynak kullanımının kısa bir görünümünü içerir. Bu bilgiler faydalıdır, ancak yalnızca küçük miktarda izleme verisi kullanılabilir. Bu verilerden bazıları otomatik olarak toplanır ve depolama kaynağı oluşturandan hemen sonra analiz için kullanılabilir. Bazı yapılandırma ile ek veri koleksiyonu türlerini etkinleştirebilirsiniz.

## <a name="what-is-azure-monitor"></a>Azure İzleyici nedir?
Azure depolama, Azure 'da tam yığın izleme hizmeti olan [Azure izleyici](../../azure-monitor/overview.md)'yi kullanarak izleme verileri oluşturur. Azure Izleyici, diğer bulutlardaki ve Şirket içindeki Azure kaynaklarınızı ve kaynaklarınızı izlemek için tam bir özellik kümesi sağlar. 

Azure Izleyici hakkında daha fazla bilgi edinmek için bkz. Azure [izleyici Ile Azure kaynaklarını izleme](../../azure-monitor/insights/monitor-azure-resource.md). Makalesinde aşağıdaki konular açıklanmaktadır:

- Azure İzleyici nedir?
- İzleme ile ilişkili maliyetler
- Azure 'da toplanan verileri izleme
- Veri toplamayı yapılandırma
- İzleme verilerini analiz etmek ve uyarı vermek için Azure 'da standart araçlar

Aşağıdaki bölümler, Azure depolama 'dan toplanan belirli verileri açıklayarak bu makalede derleme oluşturur. Örnekler, veri toplamayı yapılandırmayı ve bu verileri Azure araçları ile çözümlemeyi gösterir.

## <a name="monitor-data-from-azure-storage"></a>Azure depolama 'dan verileri izleme

Azure depolama, [Azure kaynaklarından gelen verileri izleme](../../azure-monitor/insights/monitor-azure-resource.md#monitoring-data)bölümünde açıklanan diğer Azure kaynaklarıyla aynı türde izleme verilerini toplar. Azure depolama tarafından oluşturulan Günlükler ve ölçümler hakkında daha fazla bilgi için bkz. [Azure depolama izleme veri başvurusu](monitor-storage-reference.md).

Azure Izleyici 'de ölçümler ve Günlükler yalnızca depolama hesaplarını Azure Resource Manager destekler. Azure Izleyici, klasik depolama hesaplarını desteklemez. Klasik bir depolama hesabında ölçümleri veya günlükleri kullanmak istiyorsanız bir Azure Resource Manager depolama hesabına geçiş yapmanız gerekir. Bkz. [Azure Resource Manager 'ye geçirme](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

İsterseniz klasik ölçümleri ve günlükleri kullanmaya devam edebilirsiniz. Aslında, klasik ölçümler ve Günlükler Azure Izleyici 'de ölçümler ve Günlükler ile paralel olarak kullanılabilir. Azure depolama hizmeti eski ölçümler ve günlüklerde sona erene kadar destek yerinde kalır.

### <a name="logs-in-azure-monitor-preview"></a>Azure Izleyici 'de Günlükler (Önizleme)

Günlük girişleri yalnızca hizmet uç noktasında yapılan istekler varsa oluşturulur. Örneğin, bir depolama hesabının kendi BLOB uç noktasında etkinliği varsa ancak tablo veya sıra uç noktalarında etkinlik varsa, yalnızca blob hizmetine ait günlükler oluşturulur. Azure depolama günlükleri, başarılı ve başarısız istekler hakkında bir depolama hizmetine ilişkin ayrıntılı bilgiler içerir. Bu bilgileri kullanarak istekleri ayrı ayrı izleyebilir ve depolama hizmetiyle ilgili sorunları tanılayabilirsiniz. İstekler en iyi çaba temelinde günlüğe kaydedilir.

#### <a name="log-authenticated-requests"></a>Kimliği doğrulanmış istekleri günlüğe yaz

 Aşağıdaki türden kimliği doğrulanmış istekler kaydedilir:

- Başarılı istekler
- Zaman aşımı, azaltma, ağ, yetkilendirme hatalarını ve diğer hataları içeren başarısız istekler
- Başarısız ve başarılı istekler dahil, paylaşılan erişim imzası (SAS) veya OAuth kullanan istekler
- Analiz verilerine yönelik istekler ( **$logs** kapsayıcısı içindeki klasik günlük verileri ve **$Metric** tablolarındaki sınıf ölçüm verileri)

Depolama hizmeti tarafından yapılan, günlük oluşturma veya silme gibi istekleri günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi için bkz. [depolama oturumu açma işlemleri ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) ve [depolama günlüğü biçimi](monitor-storage-reference.md).

#### <a name="log-anonymous-requests"></a>Anonim istekleri günlüğe kaydet

 Aşağıdaki anonim istek türleri günlüğe kaydedilir:

- Başarılı istekler
- Sunucu hataları
- İstemci ve sunucu için zaman aşımı hataları
- 304 hata koduyla başarısız olan istekler ALıNAMADı (değiştirilmedi)

Tüm başarısız anonim istekler günlüğe kaydedilmez. Günlüğe kaydedilen verilerin tam listesi için bkz. [depolama oturumu açma işlemleri ve durum iletileri](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) ve [depolama günlüğü biçimi](monitor-storage-reference.md).

## <a name="configuration"></a>Yapılandırma

Platform ölçümleri ve etkinlik günlüğü otomatik olarak toplanır, ancak kaynak günlüklerini toplamak veya Azure Izleyici dışında iletmek için bir tanılama ayarı oluşturmanız gerekir. Azure portal, Azure CLı veya PowerShell kullanarak bir tanılama ayarı oluşturmak için, bkz. [Azure 'da platform günlüklerini ve ölçümlerini toplamak için tanılama ayarı oluşturma](../../azure-monitor/platform/diagnostic-settings.md).

Bir tanılama ayarı oluşturduğunuzda bir blob, kuyruk, tablo veya dosya gibi günlükleri etkinleştirmek istediğiniz depolama türünü seçin. Azure portal tanılama ayarını oluşturursanız, kaynağı listeden seçebilirsiniz. PowerShell veya Azure CLı kullanıyorsanız, depolama türünün kaynak KIMLIĞINI kullanmanız gerekir. Depolama hesabınızın **Özellikler** sayfasını açarak Azure Portal kaynak kimliğini bulabilirsiniz.

Günlükleri toplamak istediğiniz işlem kategorilerini da belirtmeniz gerekir. Azure depolama kategorileri bu tabloda listelenmiştir.

| Kategori | Açıklama |
|:---|:---|
| StorageRead | Bloblarda okuma işlemleri. |
| StorageWrite | Bloblarda yazma işlemleri. |
| StorageDelete | Bloblarda silme işlemleri. |

## <a name="analyze-metric-data"></a>Ölçüm verilerini çözümleme

Azure depolama ölçümlerini, Ölçüm Gezgini kullanarak diğer Azure hizmetlerinden alınan ölçümlerle çözümleyebilirsiniz. **Azure izleyici** menüsünden **ölçümler** ' i seçerek Ölçüm Gezgini açın. Bu aracı kullanma hakkında daha fazla bilgi için bkz. [Azure Ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md)kullanmaya başlama. 

Bu örnek, **işlemlerin** hesap düzeyinde nasıl görüntüleneceğini gösterir.

![Azure portal ölçümlere erişme ekran görüntüsü](./media/monitor-storage/access-metrics-portal.png)

Boyutları destekleyen ölçümler için, ölçüyü istenen boyut değeriyle filtreleyebilirsiniz. Bu örnek, **API adı** boyutunun değerlerini seçerek belirli bir işlemdeki hesap düzeyinde **işlemlerin** nasıl görüntüleneceğini gösterir.

![Azure portal Dimension ile ölçümlere erişme ekran görüntüsü](./media/monitor-storage/access-metrics-portal-with-dimension.png)

Azure depolamanın desteklediği boyutların tüm listesi için bkz. [ölçüm boyutları](monitor-storage-reference.md#metrics-dimensions).

Tüm Azure Storage ölçümleri şu ad alanlarında bulunur:

- Microsoft. Storage/storageAccounts
- Microsoft. Storage/storageAccounts/blobServices
- Microsoft. Storage/storageAccounts/fileServices
- Microsoft. Storage/storageAccounts/queueServices
- Microsoft. Storage/storageAccounts/tableServices

Azure depolama 'yı içeren tüm Azure Izleyici destek ölçümlerinin bir listesi için bkz. [Azure izleyici desteklenen ölçümler](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).


### <a name="access-metrics"></a>Erişim ölçümleri

> [!TIP]
> Azure CLı veya .NET örneklerini görüntülemek için burada listelenen ilgili sekmeleri seçin.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

#### <a name="list-the-metric-definition"></a>Ölçüm tanımını listeleyin

Depolama hesabınızın ölçüm tanımını veya blob, dosya, tablo veya kuyruk hizmeti gibi ayrı depolama hizmetini listeleyebilirsiniz. [Get-AzMetricDefinition](https://docs.microsoft.com/powershell/module/az.monitor/get-azmetricdefinition?view=azps-3.3.0) cmdlet 'ini kullanın.

Bu örnekte, `<resource-ID>` yer tutucuyu tüm depolama hesabının kaynak kimliğiyle veya blob, dosya, tablo veya kuyruk hizmeti gibi tek bir depolama hizmetinin kaynak kimliği ile değiştirin. Bu kaynak kimliklerini, Azure portal depolama hesabınızın **Özellikler** sayfalarında bulabilirsiniz.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetricDefinition -ResourceId $resourceId
```

#### <a name="read-metric-values"></a>Ölçüm değerlerini oku

Depolama hesabınızın hesap düzeyindeki ölçüm değerlerini veya blob, dosya, tablo veya kuyruk hizmeti gibi ayrı depolama hizmetini okuyabilirsiniz. [Get-AzMetric](https://docs.microsoft.com/powershell/module/Az.Monitor/Get-AzMetric?view=azps-3.3.0) cmdlet 'ini kullanın.

```powershell
   $resourceId = "<resource-ID>"
   Get-AzMetric -ResourceId $resourceId -MetricNames "UsedCapacity" -TimeGrain 01:00:00
```

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

#### <a name="list-the-account-level-metric-definition"></a>Hesap düzeyi ölçüm tanımını listeleyin

Depolama hesabınızın ölçüm tanımını veya blob, dosya, tablo veya kuyruk hizmeti gibi ayrı depolama hizmetini listeleyebilirsiniz. [Az Monitor ölçümleri List-Definitions](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list-definitions) komutunu kullanın.
 
Bu örnekte, `<resource-ID>` yer tutucuyu tüm depolama hesabının kaynak kimliğiyle veya blob, dosya, tablo veya kuyruk hizmeti gibi tek bir depolama hizmetinin kaynak kimliği ile değiştirin. Bu kaynak kimliklerini, Azure portal depolama hesabınızın **Özellikler** sayfalarında bulabilirsiniz.

```azurecli-interactive
   az monitor metrics list-definitions --resource <resource-ID>
```

#### <a name="read-account-level-metric-values"></a>Hesap düzeyindeki ölçüm değerlerini oku

Depolama hesabınızın ölçüm değerlerini veya blob, dosya, tablo veya kuyruk hizmeti gibi ayrı depolama hizmetini okuyabilirsiniz. [Az Monitor ölçümleri List](https://docs.microsoft.com/cli/azure/monitor/metrics?view=azure-cli-latest#az-monitor-metrics-list) komutunu kullanın.

```azurecli-interactive
   az monitor metrics list --resource <resource-ID> --metric "UsedCapacity" --interval PT1H
```

### <a name="net"></a>[.NET](#tab/dotnet)

Azure Izleyici, ölçüm tanımını ve değerlerini okumak için [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) sağlar. [Örnek kod](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) , SDK 'nın farklı parametrelerle nasıl kullanılacağını gösterir. `0.18.0-preview`Depolama ölçümleri için veya sonraki bir sürümü kullanmanız gerekir.
 
Bu örneklerde, `<resource-ID>` yer tutucuyu tüm depolama hesabının kaynak kimliğiyle veya blob, dosya, tablo veya kuyruk hizmeti gibi tek bir depolama hizmetinin kaynak kimliği ile değiştirin. Bu kaynak kimliklerini, Azure portal depolama hesabınızın **Özellikler** sayfalarında bulabilirsiniz.

Değişkeni, `<subscription-ID>` ABONELIĞINIZIN kimliğiyle değiştirin. , Ve değerlerini elde etme konusunda rehberlik için `<tenant-ID>` `<application-ID>` `<AccessKey>` bkz. [Azure AD uygulaması ve kaynaklara erişebilen hizmet sorumlusu oluşturmak için portalı kullanma](https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/). 

#### <a name="list-the-account-level-metric-definition"></a>Hesap düzeyi ölçüm tanımını listeleyin

Aşağıdaki örnek, hesap düzeyinde bir ölçüm tanımının nasıl ekleneceğini göstermektedir:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";


        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            // Enumrate metric definition:
            //    Id
            //    ResourceId
            //    Name
            //    Unit
            //    MetricAvailabilities
            //    PrimaryAggregationType
            //    Dimensions
            //    IsDimensionRequired
        }
    }

```

#### <a name="read-account-level-metric-values"></a>Hesap düzeyindeki ölçüm değerlerini oku

Aşağıdaki örnek, `UsedCapacity` Hesap düzeyinde verilerin nasıl okunacağını gösterir:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        var resourceId = "<resource-ID>";
        var subscriptionId = "<subscription-ID>";
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;

        Response = await readOnlyClient.Metrics.ListAsync(
            resourceUri: resourceId,
            timespan: timeSpan,
            interval: System.TimeSpan.FromHours(1),
            metricnames: "UsedCapacity",

            aggregation: "Average",
            resultType: ResultType.Data,
            cancellationToken: CancellationToken.None);

        foreach (var metric in Response.Value)
        {
            // Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

#### <a name="read-multidimensional-metric-values"></a>Çok boyutlu ölçüm değerlerini oku

Çok boyutlu ölçümler için, belirli boyut değerlerinde ölçüm verilerini okumak istiyorsanız meta veri filtreleri tanımlamanız gerekir.

Aşağıdaki örnek, Multidimension destekleyen ölçümde ölçüm verilerinin nasıl okunacağını gösterir:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for blob storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default";
        var subscriptionId = "<subscription-ID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "<tenant-ID>";
        var applicationId = "<application-ID>";
        var accessKey = "<AccessKey>";

        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;

        Microsoft.Azure.Management.Monitor.Models.Response Response;

        string startDate = DateTime.Now.AddHours(-3).ToUniversalTime().ToString("o");
        string endDate = DateTime.Now.ToUniversalTime().ToString("o");
        string timeSpan = startDate + "/" + endDate;
        // It's applicable to define meta data filter when a metric support dimension
        // More conditions can be added with the 'or' and 'and' operators, example: BlobType eq 'BlockBlob' or BlobType eq 'PageBlob'
        ODataQuery<MetadataValue> odataFilterMetrics = new ODataQuery<MetadataValue>(
            string.Format("BlobType eq '{0}'", "BlockBlob"));

        Response = readOnlyClient.Metrics.List(
                        resourceUri: resourceId,
                        timespan: timeSpan,
                        interval: System.TimeSpan.FromHours(1),
                        metricnames: "BlobCapacity",
                        odataQuery: odataFilterMetrics,
                        aggregation: "Average",
                        resultType: ResultType.Data);

        foreach (var metric in Response.Value)
        {
            //Enumrate metric value
            //    Id
            //    Name
            //    Type
            //    Unit
            //    Timeseries
            //        - Data
            //        - Metadatavalues
        }
    }

```

---

## <a name="analyze-log-data"></a>Günlük verilerini analiz etme

Kaynak günlüklerine bir depolama hesabında blob olarak, olay verileri olarak veya log analitik sorguları aracılığıyla erişebilirsiniz.

Bu günlüklerde görünen alanların ayrıntılı bir başvurusu için bkz. [Azure depolama izleme veri başvurusu](monitor-storage-reference.md).

> [!NOTE]
> Azure Izleyici 'de Azure depolama günlükleri genel önizleme aşamasındadır ve tüm genel bulut bölgelerinde önizleme testi için kullanılabilir. Önizlemeye kaydolmak için [Bu sayfaya](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u)bakın. Bu önizleme, blob 'lar (Azure Data Lake Storage 2.), dosyalar, kuyruklar, tablolar, genel amaçlı v1 için Premium Depolama hesapları ve genel amaçlı v2 depolama hesapları için günlük Günlükler sunar. Klasik depolama hesapları desteklenmez.

### <a name="access-logs-in-a-storage-account"></a>Depolama hesabındaki günlüklere erişim

Günlükler, hedef depolama hesabındaki bir kapsayıcıya depolanan BLOB olarak görünür. Veriler, tek bir Blobun içinde, satır sınırlı bir JSON yükü olarak toplanır ve depolanır. Blobun adı şu adlandırma kuralını izler:

`https://<destination-storage-account>.blob.core.windows.net/insights-logs-<storage-operation>/resourceId=/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<source-storage-account>/blobServices/default/y=<year>/m=<month>/d=<day>/h=<hour>/m=<minute>/PT1H.json`

İşte bir örnek:

`https://mylogstorageaccount.blob.core.windows.net/insights-logs-storagewrite/resourceId=/subscriptions/`<br>`208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/blobServices/default/y=2019/m=07/d=30/h=23/m=12/PT1H.json`

### <a name="access-logs-in-an-event-hub"></a>Bir olay hub 'ında günlüklere erişim

Bir olay hub 'ına gönderilen Günlükler dosya olarak depolanmaz, ancak olay hub 'ının günlük bilgilerini aldığını doğrulayabilirsiniz. Azure portal, Olay Hub 'ınıza gidin ve **gelen iletilerin** sayısının sıfırdan büyük olduğunu doğrulayın. 

![Denetim günlükleri](media/monitor-storage/event-hub-log.png)

Güvenlik bilgileri ve olay yönetimi ve izleme araçlarını kullanarak, Olay Hub 'ınıza gönderilen günlük verilerine erişebilir ve bu verileri okuyabilirsiniz. Daha fazla bilgi için bkz. [Olay Hub 'ma gönderilen izleme verileriyle ne yapabilirim?](https://docs.microsoft.com/azure/azure-monitor/platform/stream-monitoring-data-event-hubs#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub).

### <a name="access-logs-in-a-log-analytics-workspace"></a>Log Analytics çalışma alanındaki günlüklere erişim

Azure Izleyici günlük sorgularını kullanarak, bir Log Analytics çalışma alanına gönderilen günlüklere erişebilirsiniz.

Daha fazla bilgi için bkz. [Azure izleyici 'de Log Analytics kullanmaya başlama](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal).

Veriler Bu tablolarda depolanır.

| Tablo | Açıklama |
|:---|:---|
|Storagebloblogları | Blob depolamada etkinliği tanımlayan Günlükler. |
|StorageFileLogs | Dosya paylaşımlarında etkinliği tanımlayan Günlükler. |
|StorageQueueLogs | Kuyruklarda etkinliği tanımlayan Günlükler.|
|StorageTableLogs| Tablolarda etkinliği tanımlayan Günlükler.|

### <a name="azure-storage-log-analytics-queries-in-azure-monitor"></a>Azure Izleyici 'de Azure depolama Log Analytics sorguları

Azure depolama hesaplarınızı izlemenize yardımcı olması için **günlük araması** çubuğuna girebileceğiniz bazı sorgular aşağıda verilmiştir. Bu sorgular [Yeni dille](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)çalışır.

Azure depolama hesaplarınızı izlemenize yardımcı olması için bu sorguları kullanın:

* Son üç gün içinde en sık kullanılan 10 hatayı listelemek için.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by StatusText
    | top 10 by count_ desc
    ```
* Son üç gün içinde en çok hataya neden olan ilk 10 işlemi listelemek için.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText !contains "Success"
    | summarize count() by OperationName
    | top 10 by count_ desc
    ```
* Son üç güne kadar en uzun uçtan uca gecikme süresi ile ilk 10 işlemi listelemek için.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d)
    | top 10 by DurationMs desc
    | project TimeGenerated, OperationName, DurationMs, ServerLatencyMs, ClientLatencyMs = DurationMs - ServerLatencyMs
    ```
* Son üç güne ait sunucu tarafı azaltma hatalarına neden olan tüm işlemleri listelemek için.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and StatusText contains "ServerBusy"
    | project TimeGenerated, OperationName, StatusCode, StatusText
    ```
* Son üç güne ait anonim erişime sahip tüm istekleri listelemek için.

    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d) and AuthenticationType == "Anonymous"
    | project TimeGenerated, OperationName, AuthenticationType, Uri
    ```
* Son üç gün içinde kullanılan bir dizi pasta grafiği oluşturmak için.
    ```Kusto
    StorageBlobLogs
    | where TimeGenerated > ago(3d)
    | summarize count() by OperationName
    | sort by count_ desc 
    | render piechart
    ```
## <a name="faq"></a>SSS

**Azure depolama, yönetilen diskler veya yönetilmeyen diskler için ölçümleri destekliyor mu?**

Hayır. Azure Işlem, disklerdeki ölçümleri destekler. Daha fazla bilgi için bkz. [yönetilen ve yönetilmeyen diskler için disk başına ölçümler](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/).

## <a name="next-steps"></a>Sonraki adımlar

- Azure depolama tarafından oluşturulan günlüklere ve ölçümlere yönelik bir başvuru için bkz. [Azure depolama izleme veri başvurusu](monitor-storage-reference.md).
- Azure kaynaklarını izleme hakkında daha fazla bilgi için bkz. Azure [izleyici Ile Azure kaynaklarını izleme](../../azure-monitor/insights/monitor-azure-resource.md).
- Ölçüm geçişi hakkında daha fazla bilgi için bkz. [Azure Storage ölçümleri geçişi](./storage-metrics-migration.md).

