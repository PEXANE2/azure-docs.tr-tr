---
title: Azure Veri Gezgini C# SDK kullanarak ilke oluşturma
description: Bu makalede, c# kullanarak ilke oluşturmayı öğreneceksiniz.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: fa2dd4993dbf70bcbc6ea97726f8cd7254123429
ms.sourcegitcommit: 9f330c3393a283faedaf9aa75b9fcfc06118b124
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "71997234"
---
# <a name="create-database-and-table-policies-for-azure-data-explorer-using-c"></a>Kullanarak Azure Veri Gezgini için veritabanı ve tablo ilkeleri oluşturmaC#

> [!div class="op_single_selector"]
> * [C#](database-table-policies-csharp.md)
> * [Python](database-table-policies-python.md)
>

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Bu makalede kullanarak C#Azure Veri Gezgini için veritabanı ve tablo ilkeleri oluşturacaksınız.

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio 2019 yüklü değilse, **ücretsiz** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/)' ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.

* [Test kümesi ve veritabanı](create-cluster-database-csharp.md)

* [Bir sınama tablosu](net-standard-ingest-data.md#create-a-table-on-your-test-cluster)

## <a name="install-c-nuget"></a>NuGet C# 'i yükler

* [Azure Veri Gezgini (kusto) NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)yükler.

* [Microsoft. Azure. kusto. Data. NETStandard NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Data.NETStandard/) (tablonun ilkelerini değiştirmek için isteğe bağlı) yükler.

* Kimlik doğrulaması için [Microsoft. IdentityModel. clients. ActiveDirectory NuGet paketini](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) yükler.

## <a name="authentication"></a>Kimlik Doğrulaması
Bu makaledeki örnekleri çalıştırmak için, kaynaklara erişebilen bir Azure AD uygulaması ve hizmet sorumlusu olması gerekir. Aynı Azure AD uygulamasını [bir test kümesinden ve veritabanından](create-cluster-database-csharp.md#authentication)kimlik doğrulaması için kullanabilirsiniz. Farklı bir Azure AD uygulaması kullanmak istiyorsanız, ücretsiz bir Azure AD uygulaması oluşturmak ve abonelik kapsamında rol ataması eklemek için bkz. [Azure AD uygulaması oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) . Ayrıca, `Directory (tenant) ID`, `Application ID` ve `Client Secret` ' nin nasıl alınacağını gösterir. Yeni Azure AD uygulamasını veritabanına asıl olarak eklemeniz gerekebilir, bkz. [azure Veri Gezgini veritabanı Izinlerini yönetme](https://docs.microsoft.com/azure/data-explorer/manage-database-permissions).   

## <a name="alter-database-retention-policy"></a>Alter database bekletme ilkesi
10 günlük geçici silme dönemi ile bir bekletme ilkesi ayarlar.
    
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
await kustoManagementClient.Databases.UpdateAsync(resourceGroupName, clusterName, databaseName, new DatabaseUpdate(softDeletePeriod: TimeSpan.FromDays(10)));
```

## <a name="alter-database-cache-policy"></a>Alter database Cache ilkesi
Veritabanı için son beş günün veri kümesi SSD 'sinde olacağı önbellek ilkesini ayarlar.

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
await kustoManagementClient.Databases.UpdateAsync(resourceGroupName, clusterName, databaseName, new DatabaseUpdate(hotCachePeriod: TimeSpan.FromDays(5)));
```

## <a name="alter-table-cache-policy"></a>Alter table Cache ilkesi
Son beş günün veri kümesi SSD 'sinde olacağı tablo için önbellek ilkesi ayarlar.

```csharp
var kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
var databaseName = "<DatabaseName>";
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
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

## <a name="add-a-new-principal-for-database"></a>Veritabanı için yeni bir sorumlu ekleyin
Veritabanı için yönetici sorumlusu olarak yeni bir Azure AD uygulaması ekleme

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
//The cluster and database that are created as part of the Prerequisites
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
