---
title: 'C kullanarak DB & bir Azure Veri Gezgini kümesi oluşturma #'
description: "C'yi kullanarak Azure Veri Gezgini kümesi ni ve veritabanını nasıl oluşturabilirsiniz öğrenin #"
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 0c32d438ac8551f061343edb747e9fc035b498e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246415"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-c"></a>C kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturma #

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [Powershell](create-cluster-database-powershell.md)
> * [C #](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [Azure Resource Manager şablonu](create-cluster-database-resource-manager.md)

Azure Veri Gezgini uygulamalar, web siteleri, IoT cihazları ve daha fazlasından akışı yapılan büyük miktarda veri üzerinde gerçek zamanlı analiz yapmaya yönelik hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini'ni kullanmak için öncelikle bir küme ve bu kümenin içinde bir veya daha fazla veritabanı oluşturmanız gerekir. Ardından veritabanına veri alarak (yükleyerek) sorgu çalıştırabilirsiniz. Bu makalede, C# kullanarak bir küme ve veritabanı oluşturursunuz.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2019 yüklü değilseniz, ücretsiz Visual Studio **free** [2019 Community Edition'ı](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

[!INCLUDE [data-explorer-data-connection-install-nuget-csharp](../../includes/data-explorer-data-connection-install-nuget-csharp.md)]

## <a name="authentication"></a>Kimlik doğrulaması
Bu makaledeki örnekleri çalıştırmak için, kaynaklara erişebilen bir Azure AD Uygulaması ve hizmet ilkesine ihtiyacımız var. Ücretsiz bir Azure AD Uygulaması oluşturmak ve abonelik kapsamında rol ataması eklemek için [bir Azure AD uygulaması oluşturun'u](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) denetleyin. Ayrıca nasıl almak için `Directory (tenant) ID` `Application ID`gösterir `Client Secret`, ve .

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesini oluşturma

1. Aşağıdaki kodu kullanarak kümeoluşturun:

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
    var clusterName = "mykustocluster";
    var location = "Central US";
    var skuName = "Standard_D13_v2";
    var tier = "Standard";
    var capacity = 5;
    var sku = new AzureSku(skuName, tier, capacity);
    var cluster = new Cluster(location, sku);
    await kustoManagementClient.Clusters.CreateOrUpdateAsync(resourceGroupName, clusterName, cluster);
    ```

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | clusterName | *mykustocluster* | Kümenizin istenilen adı.|
   | skuName | *Standard_D13_v2* | Kümeniz için kullanılacak SKU. |
   | tier | *Standart* | SKU kademesi. |
   | capacity | *number* | Kümeörneklerinin sayısı. |
   | resourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    > [!NOTE]
    > **Bir küme oluşturma** uzun süren bir işlemdir, bu nedenle CreateOrUpdate yerine CreateOrUpdateAsync kullanılması önerilir. 

1. Kümenizin başarıyla oluşturulup oluşturulmadığını kontrol etmek için aşağıdaki komutu çalıştırın:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

Sonuç değeri `ProvisioningState` içeriyorsa, `Succeeded` küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesinde veritabanıoluşturma

1. Aşağıdaki kodu kullanarak veritabanınızı oluşturun:

    ```csharp
    var hotCachePeriod = new TimeSpan(3650, 0, 0, 0);
    var softDeletePeriod = new TimeSpan(3650, 0, 0, 0);
    var databaseName = "mykustodatabase";
    var database = new ReadWriteDatabase(location: location, softDeletePeriod: softDeletePeriod, hotCachePeriod: hotCachePeriod);

    await kustoManagementClient.Databases.CreateOrUpdateAsync(resourceGroupName, clusterName, databaseName, database);
    ```

        [!NOTE]
        If you are using C# version 2.0.0 or below, use Database instead of ReadWriteDatabase.

   |**Ayar** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | clusterName | *mykustocluster* | Veritabanının oluşturulacağı kümenizin adı.|
   | Databasename | *mykustoveritabanı* | Veritabanınızın adı.|
   | resourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | softDeletePeriod | *3650:00:00:00* | Verilerin sorgu için kullanılabilir tutulacağı süre. |
   | hotÖnbellekDönemi | *3650:00:00:00* | Verilerin önbellekte tutulacağı süre. |

2. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```csharp
    kustoManagementClient.Databases.Get(resourceGroupName, clusterName, databaseName) as ReadWriteDatabase;
    ```

Artık bir kümeniz ve veritabanınız var.

## <a name="clean-up-resources"></a>Kaynakları temizleme

* Diğer makalelerimizi izlemeyi planlıyorsanız, oluşturduğunuz kaynakları koruyun.
* Kaynakları temizlemek için kümeyi silin. Bir kümeyi sildiğinizde, aynı zamanda tüm veritabanlarını da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```csharp
    kustoManagementClient.Clusters.Delete(resourceGroupName, clusterName);
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini .NET Standart SDK'yı kullanarak veri alma (Önizleme)](net-standard-ingest-data.md)
