---
title: Python kullanarak Azure Veri Gezgini kümesi ve veritabanı oluşturma
description: Python kullanarak Azure Veri Gezgini kümesi ve veritabanı oluşturmayı öğrenin.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 8d43965e87ab57d9f0c79c6661a761b06ccb7073
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76902109"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-python"></a>Python kullanarak Azure Veri Gezgini kümesi ve veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [ARM şablonu](create-cluster-database-resource-manager.md)

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini kullanmak için, önce bir küme oluşturun ve bu kümede bir veya daha fazla veritabanı oluşturursunuz. Daha sonra sorguları bu verilere karşı çalıştırmak için bir veritabanına (yükleme) sahip olursunuz. Bu makalede, Python kullanarak bir küme ve veritabanı oluşturursunuz.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="install-python-package"></a>Python paketini yükle

Azure Veri Gezgini Python paketini yüklemek için (kusto), yolunda Python içeren bir komut istemi açın. Şu komutu çalıştırın:

```
pip install azure-common
pip install azure-mgmt-kusto
```
## <a name="authentication"></a>Kimlik Doğrulaması
Bu makaledeki örnekleri çalıştırmak için, kaynaklara erişebilen bir Azure AD uygulaması ve hizmet sorumlusu olması gerekir. [Azure AD uygulaması oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) ' yı denetleyerek ücretsiz BIR Azure AD uygulaması oluşturun ve abonelik kapsamında rol ataması ekleyin. Ayrıca, `Directory (tenant) ID`, `Application ID`ve `Client Secret`nasıl alınacağını da gösterir.

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi oluşturma

1. Aşağıdaki komutu kullanarak kümenizi oluşturun:

    ```Python
    from azure.mgmt.kusto import KustoManagementClient
    from azure.mgmt.kusto.models import Cluster, AzureSku
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

    location = 'Central US'
    sku_name = 'Standard_D13_v2'
    capacity = 5
    tier = "Standard"
    resource_group_name = 'testrg'
    cluster_name = 'mykustocluster'
    cluster = Cluster(location=location, sku=AzureSku(name=sku_name, capacity=capacity, tier=tier))
    
    kustoManagementClient = KustoManagementClient(credentials, subscription_id)
    
    cluster_operations = kustoManagementClient.clusters
    
    poller = cluster_operations.create_or_update(resource_group_name, cluster_name, cluster)
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Kümenizin istenen adı.|
   | sku_name | *Standard_D13_v2* | Kümeniz için kullanılacak SKU. |
   | tier | *Standart* | SKU katmanı. |
   | capacity | *sayısından* | Küme örneklerinin sayısı. |
   | resource_group_name | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    > [!NOTE]
    > **Küme oluşturma** işlemi uzun süredir çalışır. Yöntem **Create_or_update** lropoller 'ın bir örneğini döndürür, daha fazla bilgi almak için bkz. [lropoller sınıfı](/python/api/msrest/msrest.polling.lropoller?view=azure-python) .

1. Kümenizin başarıyla oluşturulup oluşturulmayacağını denetlemek için şu komutu çalıştırın:

    ```Python
    cluster_operations.get(resource_group_name = resource_group_name, cluster_name= clusterName, custom_headers=None, raw=False)
    ```

Sonuç `Succeeded` değerine sahip `provisioningState` içeriyorsa, küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesinde veritabanı oluşturma

1. Aşağıdaki komutu kullanarak veritabanınızı oluşturun:

    ```Python
    from azure.mgmt.kusto.models import Database
    from datetime import timedelta
    
    softDeletePeriod = timedelta(days=3650)
    hotCachePeriod = timedelta(days=3650)
    databaseName="mykustodatabase"
    
    database_operations = kusto_management_client.databases 
    _database = ReadWriteDatabase(location=location,
                        soft_delete_period=softDeletePeriod,
                        hot_cache_period=hotCachePeriod)
    
    #Returns an instance of LROPoller, see https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
    poller =database_operations.create_or_update(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName, parameters = _database)
    ```

        [!NOTE]
        If you are using Python version 0.4.0 or below, use Database instead of ReadWriteDatabase.

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | cluster_name | *mykustocluster* | Veritabanının oluşturulacağı Kümenizin adı.|
   | database_name | *mykustodatabase* | Veritabanınızın adı.|
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
