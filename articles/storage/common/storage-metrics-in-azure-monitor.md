---
title: Azure Monitör'de Azure Depolama ölçümleri | Microsoft Dokümanlar
description: Azure Monitor'dan sunulan yeni ölçümler hakkında bilgi edinin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 09/05/2017
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 73181222bf3f15bbbac24fc253eddfea1c57bc6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247104"
---
# <a name="azure-storage-metrics-in-azure-monitor"></a>Azure İzleyici’de Azure Depolama ölçümleri

Azure Depolama'daki ölçümlerle, kullanım eğilimlerini analiz edebilir, istekleri ni izleyebilir ve depolama hesabınızla ilgili sorunları tanılayabilirsiniz.

Azure Monitor, farklı Azure hizmetleri arasında izleme için birleşik kullanıcı arabirimleri sağlar. Daha fazla bilgi için Azure [Monitor'a](../../monitoring-and-diagnostics/monitoring-overview.md)bakın. Azure Depolama, azure monitor platformuna metrik veri göndererek Azure Monitor'u entegre eder.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure Monitor ölçümlere erişmek için birden çok yol sağlar. Bunlara [Azure portalından,](https://portal.azure.com)Azure Monitör API'lerinden (REST ve .NET) ve Olay Hub'ları gibi çözüm çözümlerinden erişebilirsiniz. Daha fazla bilgi için Azure [Monitör Ölçümleri'ne](../../monitoring-and-diagnostics/monitoring-overview-metrics.md)bakın.

Ölçümler varsayılan olarak etkinleştirilir ve son 93 günlük verilere erişebilirsiniz. Verileri daha uzun süre saklamanız gerekiyorsa, metrik verileri bir Azure Depolama hesabına arşivleyebilirsiniz. Bu, Azure Monitor'daki [tanılama ayarlarında](../../azure-monitor/platform/platform-logs-overview.md) yapılandırılır.

### <a name="access-metrics-in-the-azure-portal"></a>Azure portalında erişim ölçümleri

Azure portalında ölçümleri zaman içinde izleyebilirsiniz. Aşağıdaki örnekte, **Hareketler'in** hesap düzeyinde nasıl görüntülenebildiğini gösterilmektedir.

![Azure portalında ölçümlere erişim ekran görüntüsü](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal.png)

Boyutları destekleyen ölçümler için metrik'i istenilen boyut değeriyle filtreleyebilirsiniz. Aşağıdaki örnek, **API Adı** boyutu için değerler seçerek belirli bir işlemde hesap düzeyinde **Hareketleri** nasıl görüntüleyeceğimi gösterir.

![Azure portalında boyutiçeren ölçümlere erişim ekran görüntüsü](./media/storage-metrics-in-azure-monitor/access-metrics-in-portal-with-dimension.png)

### <a name="access-metrics-with-the-rest-api"></a>REST API ile erişim ölçümleri

Azure Monitor, metrik tanımı ve değerlerini okumak için [REST API'leri](/rest/api/monitor/) sağlar. Bu bölümde, depolama ölçümlerini nasıl okuyabileceğiniz gösterilmektedir. Kaynak kimliği tüm REST APIS'lerinde kullanılır. Daha fazla bilgi için lütfen Depolama'daki hizmetler için kaynak kimliğini anlama'yı okuyun.

Aşağıdaki örnek, REST API ile testi basitleştirmek için komut satırında [ArmClient'ın](https://github.com/projectkudu/ARMClient) nasıl kullanılacağını gösterir.

#### <a name="list-account-level-metric-definition-with-the-rest-api"></a>REST API ile hesap düzeyi metrik tanımını listele

Aşağıdaki örnekte, metrik tanımın hesap düzeyinde nasıl listelenebildiğini gösterilmektedir:

```
# Login to Azure and enter your credentials when prompted.
> armclient login

> armclient GET /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions?api-version=2018-01-01

```

Blob, tablo, dosya veya sıra için metrik tanımları listelemek istiyorsanız, API ile her hizmet için farklı kaynak kimlikleri belirtmeniz gerekir.

Yanıt, JSON biçimindemetrik tanımı içerir:

```Json
{
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metricdefinitions/UsedCapacity",
      "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}",
      "category": "Capacity",
      "name": {
        "value": "UsedCapacity",
        "localizedValue": "Used capacity"
      },
      "isDimensionRequired": false,
      "unit": "Bytes",
      "primaryAggregationType": "Average",
      "metricAvailabilities": [
        {
          "timeGrain": "PT1M",
          "retention": "P30D"
        },
        {
          "timeGrain": "PT1H",
          "retention": "P30D"
        }
      ]
    },
    ... next metric definition
  ]
}

```

#### <a name="read-account-level-metric-values-with-the-rest-api"></a>REST API ile hesap düzeyindemetrik değerleri okuyun

Aşağıdaki örnek, hesap düzeyinde metrik verilerin nasıl okunacağını gösterir:

```
> armclient GET "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/microsoft.insights/metrics?metricnames=Availability&api-version=2018-01-01&aggregation=Average&interval=PT1H"

```

Yukarıdaki örnekte, blob, tablo, dosya veya sıra için metrik değerleri okumak istiyorsanız, API ile her hizmet için farklı kaynak kimlikleri belirtmeniz gerekir.

Aşağıdaki yanıt JSON biçiminde metrik değerleri içerir:

```Json
{
  "cost": 0,
  "timespan": "2017-09-07T17:27:41Z/2017-09-07T18:27:41Z",
  "interval": "PT1H",
  "value": [
    {
      "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/providers/Microsoft.Insights/metrics/Availability",
      "type": "Microsoft.Insights/metrics",
      "name": {
        "value": "Availability",
        "localizedValue": "Availability"
      },
      "unit": "Percent",
      "timeseries": [
        {
          "metadatavalues": [],
          "data": [
            {
              "timeStamp": "2017-09-07T17:27:00Z",
              "average": 100.0
            }
          ]
        }
      ]
    }
  ]
}

```

### <a name="access-metrics-with-the-net-sdk"></a>.NET SDK ile erişim ölçümleri

Azure Monitor metrik tanımı ve değerlerini okumak için [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Monitor/) sağlar. [Örnek kod,](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/) SDK'nın farklı parametrelerle nasıl kullanılacağını gösterir. Depolama ölçümleri `0.18.0-preview` için daha sonra sürüm kullanmanız gerekir. Kaynak Kimliği .NET SDK'da kullanılır. Daha fazla bilgi için lütfen Depolama'daki hizmetler için kaynak kimliğini anlama'yı okuyun.

Aşağıdaki örnekte, depolama ölçümlerini okumak için Azure Monitor .NET SDK'nın nasıl kullanılacağı gösterilmektedir.

#### <a name="list-account-level-metric-definition-with-the-net-sdk"></a>.NET SDK ile hesap düzeyi metrik tanımını listele

Aşağıdaki örnekte, metrik tanımın hesap düzeyinde nasıl listelenebildiğini gösterilmektedir:

```csharp
    public static async Task ListStorageMetricDefinition()
    {
        // Resource ID for storage account
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

        // Using metrics in Azure Monitor is currently free. However, if you use additional solutions ingesting metrics data, you may be billed by these solutions. For example, you are billed by Azure Storage if you archive metrics data to an Azure Storage account. Or you are billed by Operation Management Suite (OMS) if you stream metrics data to OMS for advanced analysis.
        MonitorManagementClient readOnlyClient = AuthenticateWithReadOnlyClient(tenantId, applicationId, accessKey, subscriptionId).Result;
        IEnumerable<MetricDefinition> metricDefinitions = await readOnlyClient.MetricDefinitions.ListAsync(resourceUri: resourceId, cancellationToken: new CancellationToken());

        foreach (var metricDefinition in metricDefinitions)
        {
            //Enumrate metric definition:
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

Blob, tablo, dosya veya sıra için metrik tanımları listelemek istiyorsanız, API ile her hizmet için farklı kaynak kimlikleri belirtmeniz gerekir.

#### <a name="read-metric-values-with-the-net-sdk"></a>.NET SDK ile metrik değerleri okuyun

Aşağıdaki örnek, hesap `UsedCapacity` düzeyinde verilerin nasıl okunduğunu gösterir:

```csharp
    public static async Task ReadStorageMetricValue()
    {
        // Resource ID for storage account
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

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

Yukarıdaki örnekte, blob, tablo, dosya veya sıra için metrik değerleri okumak istiyorsanız, API ile her hizmet için farklı kaynak kimlikleri belirtmeniz gerekir.

#### <a name="read-multi-dimensional-metric-values-with-the-net-sdk"></a>.NET SDK ile çok boyutlu metrik değerleri okuyun

Çok boyutlu ölçümler için, belirli boyut değeri ne olursa olup olmadığını metrik verileri okumak istiyorsanız meta veri filtresini tanımlamanız gerekir.

Aşağıdaki örnek, metrik destekleyici çoklu boyut üzerindeki metrik verilerin nasıl okunacağını gösterir:

```csharp
    public static async Task ReadStorageMetricValueTest()
    {
        // Resource ID for blob storage
        var resourceId = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default";
        var subscriptionId = "{SubscriptionID}";
        // How to identify Tenant ID, Application ID and Access Key: https://azure.microsoft.com/documentation/articles/resource-group-create-service-principal-portal/
        var tenantId = "{TenantID}";
        var applicationId = "{ApplicationID}";
        var accessKey = "{AccessKey}";

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

## <a name="understanding-resource-id-for-services-in-azure-storage"></a>Azure Depolama'daki hizmetler için kaynak kimliğini anlama

Kaynak Kimliği, Azure'daki bir kaynağın benzersiz bir tanımlayıcısIdır. Metrik tanımlarını veya değerlerini okumak için Azure Monitor REST API'sini kullandığınızda, çalıştırmak istediğiniz kaynak için kaynak kimliği kullanmanız gerekir. Kaynak kimliği şablonu aşağıdaki biçimi izler:

```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

Depolama, Azure Monitor ile hem depolama hesabı düzeyinde hem de hizmet düzeyinde ölçümler sağlar. Örneğin, sadece Blob depolama için ölçümleri alabilirsiniz. Her düzeyin, ölçümleri sadece bu düzey için almak için kullanılan kendi kaynak kimliği vardır.

### <a name="resource-id-for-a-storage-account"></a>Depolama hesabı için kaynak kimliği

Aşağıda, bir depolama hesabıiçin Kaynak Kimliği'ni belirtme biçimi gösterilmektedir.

```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}
```

### <a name="resource-id-for-the-storage-services"></a>Depolama hizmetleri için kaynak kimliği

Aşağıda, depolama hizmetlerinin her biri için Kaynak Kimliği'ni belirtme biçimi gösterilmektedir.

* Blob hizmet kaynak kimliği
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/blobServices/default
```
* Tablo hizmeti kaynak kimliği
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/tableServices/default
```
* Sıra hizmeti kaynak kimliği
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/queueServices/default
```
* Dosya hizmeti kaynak kimliği
```
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}/fileServices/default
```

### <a name="resource-id-in-azure-monitor-rest-api"></a>Azure Monitor REST API'sinde kaynak kimliği

Aşağıda Azure Monitor REST API'yi ararken kullanılan desen gösterilmektedir.

```
GET {resourceId}/providers/microsoft.insights/metrics?{parameters}
```

## <a name="capacity-metrics"></a>Kapasite ölçümleri
Kapasite ölçümleri değerleri her saat başı Azure Monitor'a gönderilir. Değerler her gün yenilenir. Zaman tanesi, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm kapasite ölçümleri için desteklenen zaman tanesi bir saattir (PT1H).

Azure Depolama, Azure Monitor'da aşağıdaki kapasite ölçümlerini sağlar.

### <a name="account-level"></a>Hesap Düzeyi

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
| Kullanılmış Kapasite | Depolama hesabı tarafından kullanılan depolama miktarı. Standart depolama hesapları için blob, tablo, dosya ve kuyruk tarafından kullanılan kapasitenin toplamıdır. Premium depolama ve Blob Depolama hesapları için BlobCapacity ile aynıdır. <br/><br/> Birim: Bayt <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |

### <a name="blob-storage"></a>Blob depolama

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
| BlobKapasite | Depolama hesabında kullanılan Blob depolama alanının toplamı. <br/><br/> Birim: Bayt <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 <br/> Boyutlar: **BlobType**ve **BlobTier** ([Tanım](#metrics-dimensions)) |
| BlobCount    | Depolama hesabında depolanan blob nesnelerinin sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 <br/> Boyutlar: **BlobType**ve **BlobTier** ([Tanım](#metrics-dimensions)) |
| Konteyner Sayısı    | Depolama hesabındaki kapsayıcı sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |
| Dizin Kapasitesi     | ADLS Gen2 Hiyerarşik Endeksi tarafından kullanılan depolama miktarı <br/><br/> Birim: Bayt <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |

### <a name="table-storage"></a>Table Storage

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
| Tablo Kapasitesi | Depolama hesabı tarafından kullanılan Tablo depolama miktarı. <br/><br/> Birim: Bayt <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |
| Tablo Sayısı   | Depolama hesabındaki tablo sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |
| TabloEntityCount | Depolama hesabındaki tablo varlıklarının sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |

### <a name="queue-storage"></a>Kuyruk depolama

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
| Sıra Kapasitesi | Depolama hesabı tarafından kullanılan Sıra depolama miktarı. <br/><br/> Birim: Bayt <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |
| Sıra Sayısı   | Depolama hesabındaki sıraların sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |
| QueueMessageCount | Depolama hesabında süresi dolmamış sıra iletilerinin sayısı. <br/><br/>Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |

### <a name="file-storage"></a>Dosya depolama

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
| Dosya Kapasitesi | Depolama hesabı tarafından kullanılan Dosya depolama miktarı. <br/><br/> Birim: Bayt <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |
| Dosya Sayısı   | Depolama hesabındaki dosya sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |
| FileShareCount | Depolama hesabındaki dosya paylaşımlarının sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Değer örneği: 1024 |

## <a name="transaction-metrics"></a>İşlem ölçümleri

İşlem ölçümleri, Azure Depolama'dan Azure Monitor'a kadar bir depolama hesabına yapılan her istekte yayımlanır. Depolama hesabınızda herhangi bir etkinlik olmaması durumunda, dönem içinde işlem ölçümleri hakkında veri bulunmaz. Tüm işlem ölçümleri hem hesap hem de hizmet düzeyinde (Blob depolama, Tablo depolama, Azure Dosyaları ve Sıra depolama) kullanılabilir. Zaman tanesi, metrik değerlerin sunulduğu zaman aralığını tanımlar. Tüm işlem ölçümleri için desteklenen zaman taneleri PT1H ve PT1M'dir.

Azure Depolama, Azure Monitor'da aşağıdaki işlem ölçümlerini sağlar.

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
| İşlemler | Bir depolama hizmetine yapılan isteklerin veya belirtilen API işlemi sayısı. Bu sayı, başarılı ve başarısız istekleri ve hata üreten istekleri içerir. <br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Geçerli boyutlar: ResponseType, GeoType, ApiName ve Authentication ([Tanım](#metrics-dimensions))<br/> Değer örneği: 1024 |
| Giriş | Giriş verileri miktarı. Bu sayı, dış istemciden Azure Depolama'ya giren ve Azure içinde giren verileri içerir. <br/><br/> Birim: Bayt <br/> Toplama Türü: Toplam <br/> Geçerli boyutlar: GeoType, ApiName ve Kimlik Doğrulama ([Tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Çıkış | Çıkış verileri miktarı. Bu sayı, Azure Depolama'dan harici bir istemciye çıkış ın yanı sıra Azure içindeki çıkışları da içerir. Sonuç olarak bu sayı, faturalanabilir çıkışı yansıtmaz. <br/><br/> Birim: Bayt <br/> Toplama Türü: Toplam <br/> Geçerli boyutlar: GeoType, ApiName ve Kimlik Doğrulama ([Tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| SuccessServerLatency | Azure Depolama tarafından gerçekleştirilen başarılı bir isteği işlemek için kullanılan ortalama süre. Bu değer, Başarı E2E Gecikme Süresi’nde belirtilen ağ gecikme süresini içermez. <br/><br/> Birim: Milisaniye <br/> Toplama Türü: Ortalama <br/> Geçerli boyutlar: GeoType, ApiName ve Kimlik Doğrulama ([Tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| SuccessE2ELatency | Bir depolama hizmetine yapılan başarılı isteklerin veya belirtilen API işleminin ortalama uçtan uca gecikme süresi. Bu değer, isteği okumak, yanıtı göndermek ve yanıtın onayını almak için Azure Depolama içinde gerekli işleme süresini içerir. <br/><br/> Birim: Milisaniye <br/> Toplama Türü: Ortalama <br/> Geçerli boyutlar: GeoType, ApiName ve Kimlik Doğrulama ([Tanım](#metrics-dimensions)) <br/> Değer örneği: 1024 |
| Kullanılabilirlik | Depolama hizmeti veya belirtilen API işlemi için kullanılabilirlik yüzdesi. Kullanılabilirlik, toplam faturalandırılabilir istek değerinin beklenmeyen hata üreten istekler dahil olmak üzere ilgili istek sayısına bölünmesiyle hesaplanır. Beklenmeyen tüm hatalar, depolama hizmeti veya belirtilen API işlemi için kullanılabilirliğin azalmasıyla sonuçlanır. <br/><br/> Birim: Yüzde <br/> Toplama Türü: Ortalama <br/> Geçerli boyutlar: GeoType, ApiName ve Kimlik Doğrulama ([Tanım](#metrics-dimensions)) <br/> Değer örneği: 99,99 |

## <a name="metrics-dimensions"></a>Metrik boyutları

Azure Depolama, Azure Monitor'da ölçümler için aşağıdaki boyutları destekler.

| Boyut Adı | Açıklama |
| ------------------- | ----------------- |
| **BlobType** | Yalnızca Blob ölçümleri için blob türü. Desteklenen değerler **BlockBlob**, **PageBlob**ve **Azure Veri Gölü Depolama'dır.** Ek Blob BlockBlob dahildir. |
| **BlobTier** | Azure depolama, blob nesne verilerini en uygun maliyetli şekilde depolamanıza olanak tanıyan farklı erişim katmanları sunar. [Azure Depolama blob katmanında](../blobs/storage-blob-storage-tiers.md)daha fazla sınayın. Desteklenen değerler şunlardır: <br/> <li>**Sıcak**: Sıcak katman</li> <li>**Cool**: Cool tier</li> <li>**Arşiv**: Arşiv katmanı</li> <li>**Premium**: Blok blob için premium katman</li> <li>**P4/P6/P10/P15/P20/P30/P40/P50/P60**: Premium sayfa blob için seviye türleri</li> <li>**Standart**: Standart sayfa Blob için katman türü</li> <li>**Tiered**: Genel amaçlı v1 depolama hesabı için kademe türü</li> |
| **GeoType** | Birincil veya İkincil kümeden hareket. Kullanılabilir değerler **Birincil** ve **İkincil'i**içerir. İkincil kiracıdan nesneleri okurken Access Geo Redundant Storage (RA-GRS) oku için geçerlidir. |
| **Yanıt Türü** | Hareket yanıt türü. Kullanılabilir değerler şunlardır: <br/><br/> <li>**ServerOtherError**: Açıklananlar dışında tüm diğer sunucu tarafı hataları </li> <li>**ServerBusyError**: HTTP 503 durum kodunu döndüren kimlik doğrulaması isteği. </li> <li>**ServerTimeoutError**: HTTP 500 durum kodunu döndüren zamanlanmış kimlik doğrulama isteği. Zaman aşımı bir sunucu hatası nedeniyle gerçekleşti. </li> <li>**YetkilendirmeHatası**: Verilerin yetkisiz erişimi veya yetkilendirme hatası nedeniyle başarısız olan doğrulanmış istek. </li> <li>**NetworkError**: Ağ hataları nedeniyle başarısız olan doğrulanmış istek. Çoğunlukla bir istemci, zaman aşımı süre sonundan önce bağlantıyı erkenden kapattığında gerçekleşir. </li>  <li>**ClientAccountBandwidthThrottlingError**: [İstek, depolama hesabı ölçeklenebilirlik sınırlarını](scalability-targets-standard-account.md)aştığı için bant genişliğinde daraltır.</li><li>**ClientAccountRequestThrottlingError**: İstek, [depolama hesabı ölçeklenebilirlik limitlerini](scalability-targets-standard-account.md)aştığı için istek oranında azaltılır.<li>**ClientThrottlingError**: Diğer istemci tarafı azaltma hatası. ClientAccountBandwidthThrottlingError ve ClientAccountRequestThrottlingError hariçtir.</li> <li>**ClientTimeoutError**: HTTP 500 durum kodunu döndüren zamanlanmış kimlik doğrulama isteği. İstemcinin ağ zaman aşımı veya istek zaman aşımı depolama hizmetinin beklediğinden düşük bir değere ayarlanmışsa beklenen bir zaman aşımıdır. Aksi takdirde, bir ServerTimeoutError olarak bildirilir.</li> </li> <li>**ClientOtherError**: Açıklananlar dışındaki diğer tüm istemci tarafı hataları. </li> <li>**Başarı**: Başarılı istek</li> <li> **SuccessWithThrottling**: Bir Kobİ istemcisi ilk denemede (ler) daraldığında ancak yeniden denemelerden sonra başarılı olduğunda başarılı istek.</li> |
| **ApiName** | Operasyonun adı. Örnek: <br/> <li>**Oluşturma Kapsayıcısı**</li> <li>**Blob'u Sil**</li> <li>**GetBlob**</li> Tüm işlem adları için [belgeye](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)bakın. |
| **Kimlik doğrulaması** | Hareketlerde kullanılan kimlik doğrulama türü. Kullanılabilir değerler şunlardır: <br/> <li>**AccountKey**: Hareket, depolama hesabı anahtarı yla doğrulanır.</li> <li>**SAS**: Hareket, paylaşılan erişim imzalarıyla doğrulanır.</li> <li>**OAuth**: İşlem OAuth erişim belirteçleri ile doğrulanır.</li> <li>**Anonim**: İşlem anonim olarak istenir. Uçuş öncesi istekleri içermez.</li> <li>**AnonymousPreflight**: İşlem ön kontrol talebidir.</li> |

Ölçüleri destekleyen ölçümler için, karşılık gelen ölçüm değerlerini görmek için boyut değerini belirtmeniz gerekir. Örneğin, başarılı yanıtlar için **Hareketler** değerine bakarsanız, **YanıtTürü** boyutunu **Başarı**ile filtrelemeniz gerekir. Veya BloB Blob için **BlobCount** değerine bakarsanız, **BlobType** boyutunu **BlockBlob**ile filtrelemeniz gerekir.

## <a name="service-continuity-of-legacy-metrics"></a>Eski ölçümlerin hizmet sürekliliği

Eski ölçümler, Azure Monitor yönetilen ölçümlere paralel olarak kullanılabilir. Azure Depolama hizmeti eski ölçümlerle sona erdirene kadar destek aynı tutar.

## <a name="faq"></a>SSS

**Yeni ölçümler Klasik Depolama hesabını destekliyor mu?**

Hayır, Azure Monitor'daki yeni ölçümler yalnızca Azure Kaynak Yöneticisi depolama hesaplarını destekler. Depolama hesaplarındaki ölçümleri kullanmak istiyorsanız, Azure Kaynak Yöneticisi Depolama hesabına geçiş yapmanız gerekir. Bkz. [Azure Kaynak Yöneticisine Geçir](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview).

**Azure Depolama Yönetilen Diskler veya Yönetilmeyen Diskler için ölçümleri destekliyor mu?**

Hayır, Azure İşlemi disklerde ölçümleri destekler. Daha fazla ayrıntı için [makaleye](https://azure.microsoft.com/blog/per-disk-metrics-managed-disks/) bakın.

**Klasik ölçümleri yeni ölçümlerle nasıl eşler ve geçişler?**

[Azure Depolama ölçümleri geçişinde](./storage-metrics-migration.md)klasik ölçümler ve yeni ölçümler arasında ayrıntılı eşleme bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md)
