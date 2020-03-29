---
title: 'C kullanarak Azure Veri Gezgini için Olay Izgara veri bağlantısı oluşturma #'
description: Bu makalede, C# kullanarak Azure Veri Gezgini için Olay Izgara veri bağlantısı oluşturmayı öğrenirsiniz.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: 03963f60cc364dd36ad55c0a28e92e3b585bb38d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255071"
---
# <a name="create-an-event-grid-data-connection-for-azure-data-explorer-by-using-c"></a>C kullanarak Azure Veri Gezgini için Olay Izgara veri bağlantısı oluşturma #

> [!div class="op_single_selector"]
> * [Portal](ingest-data-event-grid.md)
> * [C #](data-connection-event-grid-csharp.md)
> * [Python](data-connection-event-grid-python.md)
> * [Azure Resource Manager şablonu](data-connection-event-grid-resource-manager.md)


Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini, Olay Hub'larından, IoT Hub'larından ve blob kaplarına yazılmış lekelerden yutma (veri yükleme) sunar. Bu makalede, C# kullanarak Azure Veri Gezgini için bir Olay Izgara veri bağlantısı oluşturursunuz.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2019 yüklü değilseniz, ücretsiz Visual Studio **free** [2019 Community Edition'ı](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Küme ve veritabanı](create-cluster-database-csharp.md) oluşturma
* [Tablo ve sütun eşleme](net-standard-ingest-data.md#create-a-table-on-your-test-cluster) oluşturma
* [Veritabanı ve tablo ilkelerini](database-table-policies-csharp.md) ayarlama (isteğe bağlı)
* Olay [Izgara aboneliği olan bir depolama hesabı](ingest-data-event-grid.md#create-an-event-grid-subscription-in-your-storage-account)oluşturun.

[!INCLUDE [data-explorer-data-connection-install-nuget-csharp](../../includes/data-explorer-data-connection-install-nuget-csharp.md)]

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-an-event-grid-data-connection"></a>Olay Izgara veri bağlantısı ekleme

Aşağıdaki örnek, olay Izgara veri bağlantısının nasıl programsal olarak ekleyeceğinigösterir. Azure portalını kullanarak Olay Ağı veri bağlantısı eklemek için [Azure Veri Gezgini'nde Olay Ağı veri bağlantısı oluşturmaya](ingest-data-event-grid.md#create-an-event-grid-data-connection-in-azure-data-explorer) bakın.

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";
var authenticationContext = new AuthenticationContext($"https://login.windows.net/{tenantId}");
var credential = new ClientCredential(clientId, clientSecret);
var result = await authenticationContext.AcquireTokenAsync(resource: "https://management.core.windows.net/", clientCredential: credential);

var credentials = new TokenCredentials(result.AccessToken, result.AccessTokenType);

var kustoManagementClient = new KustoManagementClient(credentials)
{
    SubscriptionId = subscriptionId
};

var resourceGroupName = "testrg";
//The cluster and database that are created as part of the Prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
var dataConnectionName = "myeventhubconnect";
//The event hub and storage account that are created as part of the Prerequisites
var eventHubResourceId = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.EventHub/namespaces/xxxxxx/eventhubs/xxxxxx";
var storageAccountResourceId = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.Storage/storageAccounts/xxxxxx";
var consumerGroup = "$Default";
var location = "Central US";
//The table and column mapping are created as part of the Prerequisites
var tableName = "StormEvents";
var mappingRuleName = "StormEvents_CSV_Mapping";
var dataFormat = DataFormat.CSV;

await kustoManagementClient.DataConnections.CreateOrUpdateAsync(resourceGroupName, clusterName, databaseName, dataConnectionName,
    new EventGridDataConnection(storageAccountResourceId, eventHubResourceId, consumerGroup, tableName: tableName, location: location, mappingRuleName: mappingRuleName, dataFormat: dataFormat));
```

|**Ayar** | **Önerilen değer** | **Alan açıklaması**|
|---|---|---|
| tenantId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracı kimliğiniz. Dizin kimliği olarak da bilinir.|
| subscriptionId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kaynak oluşturma için kullandığınız abonelik kimliği.|
| clientId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci kimliği.|
| clientSecret | *xxxxxxxxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci sırrı. |
| resourceGroupName | *testrg* | Kümenizi içeren kaynak grubunun adı.|
| clusterName | *mykustocluster* | Kümenizin adı.|
| Databasename | *mykustoveritabanı* | Kümenizdeki hedef veritabanının adı.|
| dataConnectionName | *myeventhubconnect* | Veri bağlantınızın istenilen adı.|
| tableName | *Fırtına Etkinlikleri* | Hedef veritabanındaki hedef tablonun adı.|
| haritaKuralName | *StormEvents_CSV_Mapping* | Hedef tabloyla ilgili sütun eşlemenizin adı.|
| Dataformat | *Csv* | İletinin veri biçimi.|
| olayHubResourceId | *Kaynak kimliği* | Olay Izgarasının olayları gönderecek şekilde yapılandırıldığı Olay Hub'ınızın kaynak kimliği. |
| depolamaAccountResourceId | *Kaynak kimliği* | Depolama hesabınızın, alım için verileri tutan kaynak kimliği. |
| consumerGroup | *$Default* | Etkinlik Hub'ınızın tüketici grubu.|
| location | *Orta ABD* | Veri bağlantısı kaynağının konumu.|

## <a name="generate-sample-data"></a>Örnek veri oluşturma

Azure Veri Gezgini ve depolama hesabı bağlı olduğuna göre, örnek veriler oluşturabilir ve blob depolama alanına yükleyebilirsiniz.

Bu komut dosyası, depolama hesabınızda yeni bir kapsayıcı oluşturur, varolan bir dosyayı (blob olarak) bu kapsayıcıya yükler ve sonra kapsayıcıdaki lekeleri listeler.

```csharp
var azureStorageAccountConnectionString=<storage_account_connection_string>;

var containerName=<container_name>;
var blobName=<blob_name>;
var localFileName=<file_to_upload>;

// Creating the container
var azureStorageAccount = CloudStorageAccount.Parse(azureStorageAccountConnectionString);
var blobClient = azureStorageAccount.CreateCloudBlobClient();
var container = blobClient.GetContainerReference(containerName);
container.CreateIfNotExists();

// Set metadata and upload file to blob
var blob = container.GetBlockBlobReference(blobName);
blob.Metadata.Add("rawSizeBytes", "4096‬"); // the uncompressed size is 4096 bytes
blob.Metadata.Add("kustoIngestionMappingReference", "mapping_v2‬");
blob.UploadFromFile(localFileName);

// List blobs
var blobs = container.ListBlobs();
```

> [!NOTE]
> Azure Veri Gezgini, posta daki lekeleri silmez. Blob silme işlemini yönetmek için [Azure Blob depolama yaşam döngüsünü](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts?tabs=azure-portal) kullanarak lekeleri üç ila beş gün boyunca saklar.

[!INCLUDE [data-explorer-data-connection-clean-resources-csharp](../../includes/data-explorer-data-connection-clean-resources-csharp.md)]
