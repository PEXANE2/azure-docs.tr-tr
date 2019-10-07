---
title: Kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturmaC#
description: Şunu kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturmayı öğreninC#
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 35f11ee9bce4dc7c68e12749f69d2f2e4253d4bc
ms.sourcegitcommit: 9f330c3393a283faedaf9aa75b9fcfc06118b124
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "71996254"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-c"></a>Kullanarak bir Azure Veri Gezgini kümesi ve veritabanı oluşturmaC#

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
> * [ARM şablonu](create-cluster-database-resource-manager.md)

Azure Veri Gezgini, uygulamalardan, Web sitelerinden, IoT cihazlarından ve daha pek çok büyük miktarlarda veri akışı için gerçek zamanlı analizler için hızlı ve tam olarak yönetilen bir veri analizi hizmetidir. Azure Veri Gezgini kullanmak için, önce bir küme oluşturun ve bu kümede bir veya daha fazla veritabanı oluşturursunuz. Daha sonra sorguları bu verilere karşı çalıştırmak için bir veritabanına (yükleme) sahip olursunuz. Bu makalede, kullanarak C#bir küme ve veritabanı oluşturursunuz.

## <a name="prerequisites"></a>Prerequisites

* Visual Studio 2019 yüklü değilse, **ücretsiz** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/)' ı indirip kullanabilirsiniz. Visual Studio Kurulumu sırasında **Azure geliştirmeyi** etkinleştirdiğinizden emin olun.

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

## <a name="install-c-nuget"></a>NuGet C# 'i yükler

1. [Azure Veri Gezgini (kusto) NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)yükler.

1. Kimlik doğrulaması için [Microsoft. IdentityModel. clients. ActiveDirectory NuGet paketini](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) yükler.

## <a name="authentication"></a>Kimlik doğrulaması
Bu makaledeki örnekleri çalıştırmak için, kaynaklara erişebilen bir Azure AD uygulaması ve hizmet sorumlusu olması gerekir. [Azure AD uygulaması oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) ' yı denetleyerek ücretsiz BIR Azure AD uygulaması oluşturun ve abonelik kapsamında rol ataması ekleyin. Ayrıca, `Directory (tenant) ID`, `Application ID` ve `Client Secret` ' nin nasıl alınacağını gösterir.

## <a name="create-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesi oluşturma

1. Aşağıdaki kodu kullanarak kümenizi oluşturun:

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

   |**Ayarlanmasını** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | :/ | *mykustocluster* | Kümenizin istenen adı.|
   | skuName | *Standard_D13_v2* | Kümeniz için kullanılacak SKU. |
   | katman | *Stand* | SKU katmanı. |
   | kü | *sayısından* | Küme örneklerinin sayısı. |
   | resourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |

    > [!NOTE]
    > **Küme oluşturma** uzun süren bir işlemdir, bu yüzden CreateOrUpdate yerine CreateOrUpdateAsync kullanılması önemle önerilir. 

1. Kümenizin başarıyla oluşturulup oluşturulmayacağını denetlemek için şu komutu çalıştırın:

    ```csharp
    kustoManagementClient.Clusters.Get(resourceGroupName, clusterName);
    ```

Sonuç `Succeeded` değeriyle `ProvisioningState` içeriyorsa, küme başarıyla oluşturuldu.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>Azure Veri Gezgini kümesinde veritabanı oluşturma

1. Aşağıdaki kodu kullanarak veritabanınızı oluşturun:

    ```csharp
    var hotCachePeriod = new TimeSpan(3650, 0, 0, 0);
    var softDeletePeriod = new TimeSpan(3650, 0, 0, 0);
    var databaseName = "mykustodatabase";
    var database = new Database(location: location, softDeletePeriod: softDeletePeriod, hotCachePeriod: hotCachePeriod);

    await kustoManagementClient.Databases.CreateOrUpdateAsync(resourceGroupName, clusterName, databaseName, database);
    ```

   |**Ayarlanmasını** | **Önerilen değer** | **Alan açıklaması**|
   |---|---|---|
   | :/ | *mykustocluster* | Veritabanının oluşturulacağı Kümenizin adı.|
   | Dosyasında | *mykustodatabase* | Veritabanınızın adı.|
   | resourceGroupName | *testrg* | Kümenin oluşturulacağı kaynak grubu adı. |
   | softDeletePeriod | *3650:00:00:00* | Verilerin sorgu için kullanılabilir kalacağı zaman miktarı. |
   | hotCachePeriod | *3650:00:00:00* | Verilerin önbellekte tutulacağı zaman miktarı. |

2. Oluşturduğunuz veritabanını görmek için aşağıdaki komutu çalıştırın:

    ```csharp
    kustoManagementClient.Databases.Get(resourceGroupName, clusterName, databaseName);
    ```

Artık bir kümeniz ve veritabanınız var.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

* Diğer makalelerimizi izlemeyi planlıyorsanız oluşturduğunuz kaynakları saklayın.
* Kaynakları temizlemek için kümeyi silin. Bir kümeyi sildiğinizde, içindeki tüm veritabanlarını da siler. Kümenizi silmek için aşağıdaki komutu kullanın:

    ```csharp
    kustoManagementClient.Clusters.Delete(resourceGroupName, clusterName);
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini .NET Standard SDK 'sını kullanarak verileri alma (Önizleme)](net-standard-ingest-data.md)
