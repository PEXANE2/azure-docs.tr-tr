---
title: Veri Fabrikası - .NET API Değişim Günlüğü
description: Azure Veri Fabrikası için .NET API'nin belirli bir sürümünde çığır açan değişiklikleri, özellik eklemelerini, hata düzeltmelerini ve benzeri açıkları açıklar.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
robots: noindex
ms.date: 01/22/2018
ms.openlocfilehash: dbbbdebdcf1db7afe485166f5744f2291b757d50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74979011"
---
# <a name="azure-data-factory---net-api-change-log"></a>Azure Veri Fabrikası - .NET API değişiklik günlüğü
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. 

Bu makalede, belirli bir sürümde Azure Veri Fabrikası SDK değişiklikler hakkında bilgi sağlar. Azure Veri Fabrikası için en son NuGet paketini [burada](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) bulabilirsiniz

## <a name="version-4110"></a>Sürüm 4.11.0
Özellik Eklemeler:

* Aşağıdaki bağlantılı hizmet türleri eklendi:
  * [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
  * [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
  * [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
* Aşağıdaki veri kümesi türleri eklendi:
  * [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
  * [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
* Aşağıdaki kopya kaynak türleri eklendi:
  * [MongoDbKaynak](https://msdn.microsoft.com/library/mt765123.aspx)

## <a name="version-4100"></a>Sürüm 4.10.0
* TextFormat'a aşağıdaki isteğe bağlı özellikler eklendi:
  * [Atlama Hattı Sayısı](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
  * [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
  * [tedaviemptyasnull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
* Aşağıdaki bağlantılı hizmet türleri eklendi:
  * [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
  * [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
* Aşağıdaki veri kümesi türleri eklendi:
  * [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
* Aşağıdaki kopya kaynak türleri eklendi:
  * [CassandraKaynak](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
* AzureMLBatchExecutionActivity'e [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) özelliği ekleme
  * Azure Machine Learning denemesine birden çok web hizmeti girişi nin geçmesini etkinleştirme

## <a name="version-491"></a>Sürüm 4.9.1
### <a name="bug-fix"></a>Hata düzeltmesi
* [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx)için WebApi tabanlı kimlik doğrulamayı amortismana alın.

## <a name="version-490"></a>Sürüm 4.9.0
### <a name="feature-additions"></a>Özellik Eklemeler
* CopyActivity'e [Etkinleştirme](https://msdn.microsoft.com/library/mt767916.aspx) ve [Hazırlama Ayarları](https://msdn.microsoft.com/library/mt767918.aspx) özelliklerini ekleyin. Özellik hakkındaki ayrıntılar için [Aşamalı kopyaya](data-factory-copy-activity-performance.md#staged-copy) bakın.

### <a name="bug-fix"></a>Hata düzeltmesi
* [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) yöntemi, bir [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) örnek alır aşırı tanıtın.
* CopySink'te isteğe bağlı olarak [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) ve [WriteBatchTimeout'u](https://msdn.microsoft.com/library/dn884245.aspx) işaretleyin.

## <a name="version-480"></a>Sürüm 4.8.0
### <a name="feature-additions"></a>Özellik Eklemeler
* Kopyalama performansının aparatını etkinleştirmek için Kopyalama etkinlik türüne aşağıdaki isteğe bağlı özellikler eklendi:
  * [ParallelCopy](https://msdn.microsoft.com/library/mt767910.aspx)
  * [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Sürüm 4.7.0
### <a name="feature-additions"></a>Özellik Eklemeler
* Dosyaları en iyi duruma getirilmiş satır sütun (ORC) biçiminde kopyalamak için yeni StorageFormat türü [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) türü eklendi.
* SqlDWSink'e [İzin VerPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) ve PolyBaseSettings özelliklerini ekleyin.
  * Verileri SQL Veri Ambarı'na kopyalamak için PolyBase kullanımını sağlar.

## <a name="version-461"></a>Sürüm 4.6.1
### <a name="bug-fixes"></a>Hata Düzeltmeleri
* Etkinlik pencerelerini listelemek için HTTP isteğini düzeltir.
  * Kaynak grubu adını ve veri fabrikası adını istek yükünden kaldırır.

## <a name="version-460"></a>Sürüm 4.6.0
### <a name="feature-additions"></a>Özellik Eklemeler
* [PipelineProperties'e](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx)aşağıdaki özellikler eklenmiştir:
  * [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
  * [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
  * [Veri kümeleri](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
* [PipelineRuntimeInfo'ya](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx)aşağıdaki özellikler eklenmiştir:
  * [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
* Verileri JSON formatında olan veri kümelerini tanımlamak için yeni [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) türü [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) türü eklendi.

## <a name="version-450"></a>Sürüm 4.5.0
### <a name="feature-additions"></a>Özellik Eklemeler
* [Etkinlik penceresi için liste işlemleri](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx)eklendi.
  * Varlık türlerine (diğer bir şekilde veri fabrikaları, veri kümeleri, ardışık işlemler) göre filtrelerle etkinlik pencerelerini almak için yöntemler eklendi.
* Aşağıdaki bağlantılı hizmet türleri eklendi:
  * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Aşağıdaki veri kümesi türleri eklendi:
  * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Aşağıdaki kopya kaynak türleri eklendi:     
  * [Web Kaynak](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Sürüm 4.4.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Aşağıdaki bağlantılı hizmet türü, kopyalama etkinlikleri için veri kaynağı ve lavabo olarak eklenmiştir:
  * [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Kavramsal bilgiler ve örnekler için [Azure Depolama SAS Bağlantılı Hizmeti'ne](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) bakın.

## <a name="version-430"></a>Sürüm 4.3.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Aşağıdaki bağlantılı hizmet türleri, kopyalama etkinlikleri için veri kaynağı olarak eklendi:
  * [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Bkz. Kavramsal bilgi ve örnekler için [Veri Fabrikası'nı kullanarak HDFS'den veri taşı.](data-factory-hdfs-connector.md)
  * [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Kavramsal bilgi ve örnekler için [Azure Veri Fabrikası'nı kullanarak ODBC veri depolarından veri taşıma](data-factory-odbc-connector.md) yada bakın.

## <a name="version-420"></a>Sürüm 4.2.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Aşağıdaki yeni etkinlik türü eklendi: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Etkinlikle ilgili ayrıntılar için, [Kaynak Etkinliğini Güncelleştir'i kullanarak Azure ML modellerini güncelleştirme ye](data-factory-azure-ml-batch-execution-activity.md)bakın.
* [AzureMLLinkedService sınıfına](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx)yeni isteğe bağlı özellik [güncelleştirmesiResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) eklendi.
* [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) ve [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) özellikleri [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) sınıfına eklendi.
* Veri Fabrikası hizmetine istemci çağrıları için zaman eklerinin yapılandırmasına izin verin.

## <a name="version-410"></a>Sürüm 4.1.0
### <a name="feature-additions"></a>Özellik eklemeleri
* Aşağıdaki bağlantılı hizmet türleri eklendi:
  * [AzureDataLakeStoreBağlantılı Hizmeti](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
  * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Aşağıdaki etkinlik türleri eklendi:
  * [DataLakeAnalyticsUSQLAktivite](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Aşağıdaki veri kümesi türleri eklendi:
  * [AzureDataLakeStoreVeriseti](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Kopyalama Etkinliği için aşağıdaki kaynak ve lavabo türleri eklenmiştir:
  * [AzureDataLakeStoreKaynak](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
  * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)

## <a name="version-401"></a>Sürüm 4.0.1
### <a name="breaking-changes"></a>Yeni değişiklikler
Aşağıdaki sınıflar yeniden adlandırıldı. Yeni adlar 4.0.0 sürümünden önce sınıfların orijinal adları vardı.

| 4.0.0'daki ad | 4.0.1'deki ad |
|:--- |:--- |
| AzureSqlDataWarehouseVeriseti |[AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx) |
| AzureSqlDataset |[AzureSqlTableVeri Seti](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx) |
| AzureDataset |[AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx) |
| OracleDataset |[OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx) |
| İlişkiselDataset |[İlişkiselTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx) |
| SqlServerDataset |[SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx) |

## <a name="version-400"></a>Sürüm 4.0.0
### <a name="breaking-changes"></a>Yeni değişiklikler
* Aşağıdaki sınıflar/arabirimler yeniden adlandırıldı.

| Eski ad | Yeni ad |
|:--- |:--- |
| ITable İşlemleri |[IDatasetİşlemleri](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |
| Tablo |[Veri kümesi](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) |
| Tablo Özellikleri |[DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) |
| TabloTipiProprerties |[DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TabloOluşturmaVeya Güncelleme Parametreleri |[DatasetCreateOrUpdateParametreleri](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) |
| Tablo OluşturmaYanıt |[DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) |
| TableGetResponse |[DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) |
| Tablo Listesi Yanıtı |[DatasetListYanıt](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters |[DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) |

* **Liste** yöntemleri sayfalı sonuçları şimdi döndürer. Yanıt boş olmayan bir **NextLink** özelliği içeriyorsa, istemci uygulamasının tüm sayfalar döndürülene kadar bir sonraki sayfayı almaya devam etmesi gerekir.  Örnek aşağıda verilmiştir:

    ```csharp
    PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
    var pipelines = new List<Pipeline>(response.Pipelines);

    string nextLink = response.NextLink;
    while (!string.IsNullOrEmpty(nextLink))
    {
        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
        pipelines.AddRange(nextResponse.Pipelines);

        nextLink = nextResponse.NextLink;
    }
    ```
* **Liste** ardışık ardışık alan API'si, tam ayrıntılar yerine yalnızca bir ardışık ardışık ardışık ardışık ardışık alan özetini döndürür. Örneğin, bir ardışık hat lar özetindeki etkinlikler yalnızca ad ve tür içerir.

### <a name="feature-additions"></a>Özellik eklemeleri
* [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) sınıfı, Azure SQL Veri Ambarı'nın idempotent kopyasını desteklemek için **sliceIdentifierColumnName** ve **SqlWriterCleanupScript**olmak üzere iki yeni özelliği destekler. Bu özelliklerle ilgili ayrıntılar için [Azure SQL Veri Ambarı](data-factory-azure-sql-data-warehouse-connector.md) makalesine bakın.
* Artık Kopyalama Etkinliği'nin bir parçası olarak Azure SQL Veritabanı ve Azure SQL Veri Ambarı kaynaklarına karşı depolanan yordamı desteklemeyi destekliyoruz. [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) ve [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) sınıfları aşağıdaki özelliklere sahiptir: **SqlReaderStoredProcedureName** ve **StoredProcedureParameters**. Bu özelliklerle ilgili ayrıntılar için Azure.com Azure [SQL Veritabanı](data-factory-azure-sql-connector.md#sqlsource) ve Azure SQL [Veri Ambarı](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) makalelerini inceleyin.  
