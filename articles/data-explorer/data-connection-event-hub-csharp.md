---
title: 'C kullanarak Azure Veri Gezgini için Olay Hub veri bağlantısı oluşturma #'
description: Bu makalede, C# kullanarak Azure Veri Gezgini için Bir Olay Hub veri bağlantısı oluşturmayı öğrenirsiniz.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: cf2a274b4f48b31739d6abba5cf87fa2a10d4ca1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74667687"
---
# <a name="create-an-event-hub-data-connection-for-azure-data-explorer-by-using-c"></a>C kullanarak Azure Veri Gezgini için Olay Hub veri bağlantısı oluşturma #

> [!div class="op_single_selector"]
> * [Portal](ingest-data-event-hub.md)
> * [C #](data-connection-event-hub-csharp.md)
> * [Python](data-connection-event-hub-python.md)
> * [Azure Resource Manager şablonu](data-connection-event-hub-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini, Olay Hub'larından, IoT Hub'larından ve blob kaplarına yazılmış lekelerden yutma (veri yükleme) sunar. Bu makalede, C# kullanarak Azure Veri Gezgini için bir Olay Hub veri bağlantısı oluşturursunuz.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2019 yüklü değilseniz, ücretsiz Visual Studio **free** [2019 Community Edition'ı](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Küme ve veritabanı](create-cluster-database-csharp.md) oluşturma
* [Tablo ve sütun eşleme](net-standard-ingest-data.md#create-a-table-on-your-test-cluster) oluşturma
* [Veritabanı ve tablo ilkelerini](database-table-policies-csharp.md) ayarlama (isteğe bağlı)
* Yutma için veri içeren bir [Olay Hub'ı](ingest-data-event-hub.md#create-an-event-hub)oluşturun. 

[!INCLUDE [data-explorer-data-connection-install-nuget-csharp](../../includes/data-explorer-data-connection-install-nuget-csharp.md)]

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-an-event-hub-data-connection"></a>Olay Hub veri bağlantısı ekleme

Aşağıdaki örnekte, Olay Hub veri bağlantısının nasıl programsal olarak eklenilebildiğini gösterilmektedir. Azure [portalını](ingest-data-event-hub.md#connect-to-the-event-hub) kullanarak Bir Olay Hub veri bağlantısı eklemek için etkinlik merkezine bağlan'a bakın.

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
//The event hub that is created as part of the Prerequisites
var eventHubResourceId = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.EventHub/namespaces/xxxxxx/eventhubs/xxxxxx";
var consumerGroup = "$Default";
var location = "Central US";
//The table and column mapping are created as part of the Prerequisites
var tableName = "StormEvents";
var mappingRuleName = "StormEvents_CSV_Mapping";
var dataFormat = DataFormat.CSV;
await kustoManagementClient.DataConnections.CreateOrUpdateAsync(resourceGroupName, clusterName, databaseName, dataConnectionName, 
    new EventHubDataConnection(eventHubResourceId, consumerGroup, location: location, tableName: tableName, mappingRuleName: mappingRuleName, dataFormat: dataFormat));
```

|**Ayar** | **Önerilen değer** | **Alan açıklaması**|
|---|---|---|
| tenantId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracı kimliğiniz. Dizin kimliği olarak da bilinir.|
| subscriptionId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kaynak oluşturma için kullandığınız abonelik kimliği.|
| clientId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci kimliği.|
| clientSecret | *xxxxxxxxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci sırrı.|
| resourceGroupName | *testrg* | Kümenizi içeren kaynak grubunun adı.|
| clusterName | *mykustocluster* | Kümenizin adı.|
| Databasename | *mykustoveritabanı* | Kümenizdeki hedef veritabanının adı.|
| dataConnectionName | *myeventhubconnect* | Veri bağlantınızın istenilen adı.|
| tableName | *Fırtına Etkinlikleri* | Hedef veritabanındaki hedef tablonun adı.|
| haritaKuralName | *StormEvents_CSV_Mapping* | Hedef tabloyla ilgili sütun eşlemenizin adı.|
| Dataformat | *Csv* | İletinin veri biçimi.|
| olayHubResourceId | *Kaynak kimliği* | Bilgi alma için verileri tutan Olay Hub'ınızın kaynak kimliği. |
| consumerGroup | *$Default* | Etkinlik Hub'ınızın tüketici grubu.|
| location | *Orta ABD* | Veri bağlantısı kaynağının konumu.|

[!INCLUDE [data-explorer-data-connection-clean-resources-csharp](../../includes/data-explorer-data-connection-clean-resources-csharp.md)]
