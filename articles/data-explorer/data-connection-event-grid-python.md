---
title: Python kullanarak Azure Veri Gezgini için Olay Ağı veri bağlantısı oluşturma
description: Bu makalede, Python'u kullanarak Azure Veri Gezgini için Olay Izgara veri bağlantısı oluşturmayı öğrenirsiniz.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: 1439383598517f57bc77e718d4ded7f53941d3bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444205"
---
# <a name="create-an-event-grid-data-connection-for-azure-data-explorer-by-using-python"></a>Python kullanarak Azure Veri Gezgini için Olay Ağı veri bağlantısı oluşturma

> [!div class="op_single_selector"]
> * [Portal](ingest-data-event-grid.md)
> * [C #](data-connection-event-grid-csharp.md)
> * [Python](data-connection-event-grid-python.md)
> * [Azure Resource Manager şablonu](data-connection-event-grid-resource-manager.md)

Bu makalede, Python kullanarak Azure Veri Gezgini için bir Olay Izgara veri bağlantısı oluşturursunuz. Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Azure Veri Gezgini, Olay Hub'larından, IoT Hub'larından ve blob kapsayıcılarına yazılmış lekelerden yutma veya veri yükleme söner.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* [Python 3.4+](https://www.python.org/downloads/).

* [Bir küme ve veritabanı.](create-cluster-database-python.md)

* [Tablo ve sütun eşleme.](net-standard-ingest-data.md#create-a-table-on-your-test-cluster)

* [Veritabanı ve tablo ilkeleri](database-table-policies-csharp.md) (isteğe bağlı).

* [Olay Izgara aboneliği olan bir depolama hesabı.](ingest-data-event-grid.md#create-an-event-grid-subscription-in-your-storage-account)

[!INCLUDE [data-explorer-data-connection-install-package-python](../../includes/data-explorer-data-connection-install-package-python.md)]

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-an-event-grid-data-connection"></a>Olay Izgara veri bağlantısı ekleme

Aşağıdaki örnek, olay Izgara veri bağlantısının nasıl programsal olarak ekleyeceğinigösterir. Azure portalını kullanarak Olay Ağı veri bağlantısı eklemek için [Azure Veri Gezgini'nde Olay Ağı veri bağlantısı oluşturmaya](ingest-data-event-grid.md#create-an-event-grid-data-connection-in-azure-data-explorer) bakın.


```Python
from azure.mgmt.kusto import KustoManagementClient
from azure.mgmt.kusto.models import EventGridDataConnection
from azure.common.credentials import ServicePrincipalCredentials

#Directory (tenant) ID
tenant_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Application ID
client_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
#Client Secret
client_secret = "xxxxxxxxxxxxxx"
subscription_id = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx"
credentials = ServicePrincipalCredentials(
        client_id=client_id,
        secret=client_secret,
        tenant=tenant_id
    )
kusto_management_client = KustoManagementClient(credentials, subscription_id)

resource_group_name = "testrg"
#The cluster and database that are created as part of the Prerequisites
cluster_name = "mykustocluster"
database_name = "mykustodatabase"
data_connection_name = "myeventhubconnect"
#The event hub and storage account that are created as part of the Prerequisites
event_hub_resource_id = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.EventHub/namespaces/xxxxxx/eventhubs/xxxxxx"
storage_account_resource_id = "/subscriptions/xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxx/providers/Microsoft.Storage/storageAccounts/xxxxxx"
consumer_group = "$Default"
location = "Central US"
#The table and column mapping that are created as part of the Prerequisites
table_name = "StormEvents"
mapping_rule_name = "StormEvents_CSV_Mapping"
data_format = "csv"

#Returns an instance of LROPoller, check https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
poller = kusto_management_client.data_connections.create_or_update(resource_group_name=resource_group_name, cluster_name=cluster_name, database_name=database_name, data_connection_name=data_connection_name,
                                            parameters=EventGridDataConnection(storage_account_resource_id=storage_account_resource_id, event_hub_resource_id=event_hub_resource_id, 
                                                                                consumer_group=consumer_group, table_name=table_name, location=location, mapping_rule_name=mapping_rule_name, data_format=data_format))
```
|**Ayar** | **Önerilen değer** | **Alan açıklaması**|
|---|---|---|
| tenant_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracı kimliğiniz. Dizin kimliği olarak da bilinir.|
| subscription_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kaynak oluşturma için kullandığınız abonelik kimliği.|
| client_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci kimliği.|
| client_secret | *xxxxxxxxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci sırrı. |
| resource_group_name | *testrg* | Kümenizi içeren kaynak grubunun adı.|
| cluster_name | *mykustocluster* | Kümenizin adı.|
| Veritabanı_adı | *mykustoveritabanı* | Kümenizdeki hedef veritabanının adı.|
| data_connection_name | *myeventhubconnect* | Veri bağlantınızın istenilen adı.|
| Table_name | *Fırtına Etkinlikleri* | Hedef veritabanındaki hedef tablonun adı.|
| mapping_rule_name | *StormEvents_CSV_Mapping* | Hedef tabloyla ilgili sütun eşlemenizin adı.|
| data_format | *Csv* | İletinin veri biçimi.|
| event_hub_resource_id | *Kaynak kimliği* | Olay Izgarasının olayları gönderecek şekilde yapılandırıldığı Olay Hub'ınızın kaynak kimliği. |
| storage_account_resource_id | *Kaynak kimliği* | Depolama hesabınızın, alım için verileri tutan kaynak kimliği. |
| consumer_group | *$Default* | Etkinlik Hub'ınızın tüketici grubu.|
| location | *Orta ABD* | Veri bağlantısı kaynağının konumu.|

[!INCLUDE [data-explorer-data-connection-clean-resources-python](../../includes/data-explorer-data-connection-clean-resources-python.md)]