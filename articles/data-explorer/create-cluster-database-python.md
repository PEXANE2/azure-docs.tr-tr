---
title: Python kullanarak Bir Azure Veri Gezgini kümesi & DB oluşturma
description: Python'u kullanarak Azure Veri Gezgini kümesi ni ve veritabanını nasıl oluşturup oluşturabilirsiniz öğrenin.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 8425058c9f6ac5b90c37a99f749a810672b406fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560516"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-python"></a>Python'u kullanarak Bir Azure Veri Gezgini kümesi ve veritabanı oluşturma

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [Powershell](create-cluster-database-powershell.md)
> * [C #](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [ARM şablonu](create-cluster-database-resource-manager.md)

Bu makalede, Python kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturursunuz. Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini'ni kullanmak için önce bir küme oluşturun ve bu kümede bir veya daha fazla veritabanı oluşturun. Ardından, sorguları veritabanına çalıştırabilmeniz için verileri bir veritabanına yükleyin veya yükleyin.

## <a name="prerequisites"></a>Ön koşullar

* Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir tane oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)

* [Python 3.4+](https://www.python.org/downloads/).

* [Kaynaklara erişebilen bir Azure AD Uygulaması ve hizmet ilkesi.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) `Directory (tenant) ID`Için `Application ID`' ve `Client Secret`.

## <a name="install-python-package"></a>Python paketini yükleme

Azure Veri Gezgini (Kusto) için Python paketini yüklemek için, Python'un yoluna çıkan bir komut istemini açın. Şu komutu çalıştırın:

```
pip install azure-common
pip install azure-mgmt-kusto
```
## <a name="authentication"></a>Kimlik doğrulaması
Bu makaledeki örnekleri çalıştırmak için, kaynaklara erişebilen bir Azure AD Uygulaması ve hizmet ilkesine ihtiyacımız var. Ücretsiz bir Azure AD Uygulaması oluşturmak ve abonelik kapsamında rol ataması eklemek için [bir Azure AD uygulaması oluşturun'u](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) denetleyin. Ayrıca nasıl almak için `Directory (tenant) ID` `Application ID`gösterir `Client Secret`, ve .

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesini oluşturma

1. Aşağıdaki komutu kullanarak kümeoluşturun:

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
   | cluster_name | *mykustocluster* | Kümenizin istenilen adı.|
   | sku_name | *Standard_D13_v2* | Kümeniz için kullanılacak SKU. |
   | tier | *Standart* | SKU kademesi. |
   | capacity | *number* | Kümeörneklerinin sayısı. |
   | resource_group_name | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    > [!NOTE]
    > **Bir küme oluşturma** uzun süren bir işlemdir. Yöntem **create_or_update** LROPoller bir örnek döndürür, daha fazla bilgi almak için [LROPoller sınıf](/python/api/msrest/msrest.polling.lropoller?view=azure-python) bakın.

1. Kümenizin başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```Python
    cluster_operations.get(resource_group_name = resource_group_name, cluster_name= clusterName, custom_headers=None, raw=False)
    ```

Sonuç değeri `provisioningState` içeriyorsa, `Succeeded` küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesinde veritabanıoluşturma

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
   | cluster_name | *mykustocluster* | Veritabanının oluşturulacağı kümenizin adı.|
   | Veritabanı_adı | *mykustoveritabanı* | Veritabanınızın adı.|
   | resource_group_name | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | soft_delete_period | *3650 gün, 0:00:00* | Verilerin sorgu için kullanılabilir tutulacağı süre. |
   | hot_cache_period | *3650 gün, 0:00:00* | Verilerin önbellekte tutulacağı süre. |

1. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```Python
    database_operations.get(resource_group_name = resource_group_name, cluster_name = clusterName, database_name = databaseName)
    ```

Artık bir kümeniz ve veritabanınız var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer makalelerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları koruyun.
* Kaynakları temizlemek için kümeyi silin. Bir kümeyi sildiğinizde, aynı zamanda tüm veritabanlarını da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```Python
    cluster_operations.delete(resource_group_name = resource_group_name, cluster_name = clusterName)
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Python kitaplığını kullanarak veri alma](python-ingest-data.md)
