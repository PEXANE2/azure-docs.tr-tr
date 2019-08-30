---
title: Python kullanarak Azure Veri Gezgini kümesi ve veritabanı oluşturma
description: Python kullanarak Azure Veri Gezgini kümesi ve veritabanı oluşturmayı öğrenin.
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: e3f58e596db26c04a8f3be4f87eb129fadf5e328
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70141749"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-python"></a>Python kullanarak Azure Veri Gezgini kümesi ve veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini kullanmak için, önce bir küme oluşturun ve bu kümede bir veya daha fazla veritabanı oluşturursunuz. Daha sonra sorguları bu verilere karşı çalıştırmak için bir veritabanına (yükleme) sahip olursunuz. Bu makalede, Python kullanarak bir küme ve veritabanı oluşturursunuz.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="install-python-package"></a>Python paketini yükle

Azure Veri Gezgini Python paketini yüklemek için (kusto), yolunda Python içeren bir komut istemi açın. Şu komutu çalıştırın:

```
pip install azure-mgmt-kusto
```

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi oluşturma

1. Aşağıdaki komutu kullanarak kümenizi oluşturun:

    ```Python
    from azure.mgmt.kusto.kusto_management_client import KustoManagementClient
    from azure.mgmt.kusto.models import Cluster, AzureSku

    credentials = xxxxxxxxxxxxxxx
    
    subscription_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx'
    location = 'Central US'
    sku = 'D13_v2'
    capacity = 5
    resource_group_name = 'testrg'
    cluster_name = 'mykustocluster'
    cluster = Cluster(location=location, sku=AzureSku(name=sku, capacity=capacity))
    
    kustoManagementClient = KustoManagementClient(credentials, subscription_id)
    
    cluster_operations = kustoManagementClient.clusters
    
    cluster_operations.create_or_update(resource_group_name, cluster_name, cluster)
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Kümenizin istenen adı.|
   | sku | *D13_v2* | Kümeniz için kullanılacak SKU. |
   | resource_group_name | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    Küme kapasitesi gibi kullanabileceğiniz ek isteğe bağlı parametreler vardır.
    
1. [ *Kimlik bilgilerinizi* ayarlama](/azure/python/python-sdk-azure-authenticate)

1. Kümenizin başarıyla oluşturulup oluşturulmayacağını denetlemek için şu komutu çalıştırın:

    ```Python
    cluster_operations.get(resource_group_name = resource_group_name, cluster_name= clusterName, custom_headers=None, raw=False)
    ```

Sonuç `provisioningState` değeri`Succeeded` ile içeriyorsa, küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesinde veritabanı oluşturma

1. Aşağıdaki komutu kullanarak veritabanınızı oluşturun:

    ```Python
    from azure.mgmt.kusto.models import Database
    from datetime import timedelta
    
    softDeletePeriod = timedelta(days=3650)
    hotCachePeriod = timedelta(days=3650)
    databaseName="mykustodatabase"
    
    database_operations = kusto_management_client.databases 
    _database = Database(location=location,
                        soft_delete_period=softDeletePeriod,
                        hot_cache_period=hotCachePeriod)
    
    database_operations.create_or_update(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName, parameters = _database)
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Veritabanının oluşturulacağı Kümenizin adı.|
   | veritabanı | *mykustodatabase* | Veritabanınızın adı.|
   | resource_group_name | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | soft_delete_period | *3650 gün, 0:00:00* | Verilerin sorgu için kullanılabilir kalacağı zaman miktarı. |
   | hot_cache_period | *3650 gün, 0:00:00* | Verilerin önbellekte tutulacağı zaman miktarı. |

1. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```Python
    database_operations.get(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName)
    ```

Artık bir kümeniz ve veritabanınız var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer makalelerimizi izlemeyi planlıyorsanız oluşturduğunuz kaynakları saklayın.
* Kaynakları temizlemek için kümeyi silin. Bir kümeyi sildiğinizde, içindeki tüm veritabanlarını da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```Python
    cluster_operations.delete(resource_group_name = resource_group_name, cluster_name = clusterName)
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Python kitaplığını kullanarak verileri alma](python-ingest-data.md)
