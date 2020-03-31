---
title: Azure Veri Gezgini C# SDK'yı kullanarak ilkeler oluşturun
description: Bu makalede, C# kullanarak ilkeleri oluşturmak için öğreneceksiniz.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 17312840b0081056ad04723f2b2c241c47902021
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74667300"
---
# <a name="create-database-and-table-policies-for-azure-data-explorer-by-using-c"></a>C kullanarak Azure Veri Gezgini için veritabanı ve tablo ilkeleri oluşturun #

> [!div class="op_single_selector"]
> * [C #](database-table-policies-csharp.md)
> * [Python](database-table-policies-python.md)
>

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Bu makalede, C# kullanarak Azure Veri Gezgini için veritabanı ve tablo ilkeleri oluşturacaksınız.

## <a name="prerequisites"></a>Ön koşullar

* Görsel Stüdyo 2019. Visual Studio 2019'unuzun yoksa *ücretsiz* [Visual Studio Community 2019'u](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure geliştirmeyi** seçtiğinizden emin olun.
* Azure aboneliği. Gerekirse, başlamadan önce ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/) oluşturabilirsiniz.
* [Bir test kümesi ve veritabanı.](create-cluster-database-csharp.md)
* [Bir test tablosu](net-standard-ingest-data.md#create-a-table-on-your-test-cluster).

## <a name="install-c-nuget"></a>C# NuGet yükle

* Azure [Veri Gezgini (Kusto) NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)yükleyin.
* [Microsoft.Azure.Kusto.Data.NETStandard NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Data.NETStandard/)yükleyin. (Tablo ilkelerini değiştirmek için isteğe bağlıdır.)
* Kimlik doğrulama için [Microsoft.IdentityModel.Clients.ActiveDirectory NuGet paketini](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)yükleyin.

## <a name="authentication"></a>Kimlik doğrulaması
Bu makaledeki örnekleri çalıştırmak için kaynaklara erişebilen bir Azure Etkin Dizin (Azure AD) uygulaması na ve hizmet ilkesine ihtiyacınız vardır. Bir [test kümesinden ve veritabanından](create-cluster-database-csharp.md#authentication)kimlik doğrulama için aynı Azure AD uygulamasını kullanabilirsiniz. Farklı bir Azure AD uygulaması kullanmak istiyorsanız, ücretsiz bir Azure AD uygulaması oluşturmak ve abonelik kapsamında rol ataması eklemek için [bir Azure AD uygulaması oluşturmaya](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) bakın. Bu makalede ayrıca nasıl `Directory (tenant) ID`almak `Application ID`için `Client secret`gösterir , , ve . Yeni Azure AD uygulamasını veritabanına ana para olarak eklemeniz gerekebilir. Daha fazla bilgi için azure [veri gezgini veritabanı izinlerini yönet'e](https://docs.microsoft.com/azure/data-explorer/manage-database-permissions)bakın.

## <a name="alter-database-retention-policy"></a>Veritabanı tutma ilkesini değiştirme
10 günlük yumuşak silme süresiyle bekletme ilkesi ayarlar.
    
```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client secret
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
//The cluster and database that are created as part of the prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
await kustoManagementClient.Databases.UpdateAsync(resourceGroupName, clusterName, databaseName, new DatabaseUpdate(softDeletePeriod: TimeSpan.FromDays(10)));
```

## <a name="alter-database-cache-policy"></a>Veritabanı önbelleği ilkesini değiştirme
Veritabanı için bir önbellek ilkesi ayarlar. Önceki beş günlük veri SSD kümesinde olacaktır.

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client secret
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
//The cluster and database that are created as part of the prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
await kustoManagementClient.Databases.UpdateAsync(resourceGroupName, clusterName, databaseName, new DatabaseUpdate(hotCachePeriod: TimeSpan.FromDays(5)));
```

## <a name="alter-table-cache-policy"></a>Tablo önbelleği ilkesini değiştir
Tablo için bir önbellek ilkesi ayarlar. Önceki beş günlük veri SSD kümesinde olacaktır.

```csharp
var kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
var databaseName = "<DatabaseName>";
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client secret
var tableName = "<TableName>"

var kustoConnectionStringBuilder =
    new KustoConnectionStringBuilder(kustoUri)
    {
        FederatedSecurity = true,
        InitialCatalog = databaseName,
        ApplicationClientId = clientId,
        ApplicationKey = clientSecret,
        Authority = tenantId
    };

using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    //dataHotSpan and indexHotSpan should have the same value
    var hotSpan = TimeSpan.FromDays(5);
    var command1 = CslCommandGenerator.GenerateAlterTableCachingPolicyCommand(tableName: tableName,
                    dataHotSpan: hotSpan, indexHotSpan: hotSpan);

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="add-a-new-principal-for-the-database"></a>Veritabanı için yeni bir müdür ekleme
Veritabanı için yönetici anabilim da yeni bir Azure AD uygulaması ekler.

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var clientIdToAdd = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
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
//The cluster and database that are created as part of the prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
await kustoManagementClient.Databases.AddPrincipalsAsync(resourceGroupName, clusterName, databaseName,
                new DatabasePrincipalListRequest()
                {
                    Value = new List<DatabasePrincipal>()
                    {
                        new DatabasePrincipal("Admin", "<database_principle_name>", "App", appId: clientIdToAdd, tenantName:tenantId)
                    }
                });
```
## <a name="next-steps"></a>Sonraki adımlar

* [Veritabanı ve tablo ilkeleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/kusto/management/policies)
