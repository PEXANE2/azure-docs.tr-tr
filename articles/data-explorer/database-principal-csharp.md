---
title: 'C kullanarak Azure Veri Gezgini için veritabanı ilkeleri ekleme #'
description: Bu makalede, C# kullanarak Azure Veri Gezgini için veritabanı ilkelerini nasıl ekleyeceğinizi öğrenirsiniz.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: 797d1253d44739f2026563e3df72bc85a8ef382e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76965040"
---
# <a name="add-database-principals-for-azure-data-explorer-by-using-c"></a>C kullanarak Azure Veri Gezgini için veritabanı ilkeleri ekleme #

> [!div class="op_single_selector"]
> * [C #](database-principal-csharp.md)
> * [Python](database-principal-python.md)
> * [Azure Resource Manager şablonu](database-principal-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Bu makalede, C# kullanarak Azure Veri Gezgini için veritabanı ilkeleri ekleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2019 yüklü değilseniz, ücretsiz Visual Studio **free** [2019 Community Edition'ı](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Bir küme ve veritabanı oluşturun.](create-cluster-database-csharp.md)

## <a name="install-c-nuget"></a>C# NuGet yükle

* [Microsoft.Azure.Management.kusto'ya yükleyin.](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)
* Kimlik doğrulaması için [Microsoft.Rest.ClientRuntime.Azure.Authentication'ı](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication) yükleyin.

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-a-database-principal"></a>Veritabanı ilkesi ekleme

Aşağıdaki örnek, bir veritabanı ilkesini programlı olarak nasıl ekleyeceğinizi gösterir.

```csharp
var tenantId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Directory (tenant) ID
var clientId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";//Application ID
var clientSecret = "xxxxxxxxxxxxxx";//Client Secret
var subscriptionId = "xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx";

var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, clientSecret);
var kustoManagementClient = new KustoManagementClient(serviceCreds)
{
    SubscriptionId = subscriptionId
};

var resourceGroupName = "testrg";
//The cluster and database that are created as part of the Prerequisites
var clusterName = "mykustocluster";
var databaseName = "mykustodatabase";
string principalAssignmentName = "databasePrincipalAssignment1";
string principalId = "xxxxxxxx";//User email, application ID, or security group name
string role = "Admin";//Admin, Ingestor, Monitor, User, UnrestrictedViewers, Viewer
string tenantIdForPrincipal = tenantId;
string principalType = "App";//User, App, or Group

var databasePrincipalAssignment = new DatabasePrincipalAssignment(principalId, role, principalType, tenantId: tenantIdForPrincipal);
await kustoManagementClient.DatabasePrincipalAssignments.CreateOrUpdateAsync(resourceGroupName, clusterName, databaseName, principalAssignmentName, databasePrincipalAssignment);
```

|**Ayar** | **Önerilen değer** | **Alan açıklaması**|
|---|---|---|
| tenantId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracı kimliğiniz. Dizin kimliği olarak da bilinir.|
| subscriptionId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kaynak oluşturma için kullandığınız abonelik kimliği.|
| clientId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci kimliği.|
| clientSecret | *xxxxxxxxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci sırrı. |
| resourceGroupName | *testrg* | Kümenizi içeren kaynak grubunun adı.|
| clusterName | *mykustocluster* | Kümenizin adı.|
| Databasename | *mykustoveritabanı* | Veritabanınızın adı.|
| principalAssignmentName | *veritabanıPrincipalAssignment1* | Veritabanı ana kaynağınızın adı.|
| principalId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kullanıcı e-postası, uygulama kimliği veya güvenlik grubu adı olabilecek ana kimlik.|
| rol | *Yönetici* | 'Yönetici', 'Ingestor', 'Monitör', 'Kullanıcı', 'Sınırsız Görüntüleyenler', 'Görüntüleyici' olabilecek veritabanı yöneticinizin rolü.|
| kiracıIdForPrincipal | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Müdürün kiracı kimliği.|
| Principaltype | *Uygulama* | 'Kullanıcı', 'Uygulama' veya 'Grup' olabilecek anaparanın türü|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Düğümü kitaplığını kullanarak veri alma](node-ingest-data.md)
