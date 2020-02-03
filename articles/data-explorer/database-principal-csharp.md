---
title: Kullanarak Azure Veri Gezgini için veritabanı sorumlularını eklemeC#
description: Bu makalede kullanarak C#Azure Veri Gezgini için veritabanı sorumlularını eklemeyi öğreneceksiniz.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: 797d1253d44739f2026563e3df72bc85a8ef382e
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76965040"
---
# <a name="add-database-principals-for-azure-data-explorer-by-using-c"></a>Kullanarak Azure Veri Gezgini için veritabanı sorumlularını eklemeC#

> [!div class="op_single_selector"]
> * [C#](database-principal-csharp.md)
> * [Python](database-principal-python.md)
> * [Azure Resource Manager şablonu](database-principal-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Bu makalede kullanarak C#Azure Veri Gezgini için veritabanı sorumlularını eklersiniz.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2019 yüklü değilse, **ücretsiz** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/)' ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Bir küme ve veritabanı oluşturun](create-cluster-database-csharp.md).

## <a name="install-c-nuget"></a>NuGet C# 'i yükler

* [Microsoft. Azure. Management. kusto](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)uygulamasını yükler.
* Kimlik doğrulaması için [Microsoft. Rest. ClientRuntime. Azure. Authentication](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication) 'ı yükler.

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-a-database-principal"></a>Asıl veritabanı Ekle

Aşağıdaki örnek, programlı olarak bir veritabanı sorumlusunu nasıl ekleneceğini gösterir.

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
| tenantId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracı KIMLIĞINIZ. Dizin KIMLIĞI olarak da bilinir.|
| subscriptionId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kaynak oluşturma için kullandığınız abonelik KIMLIĞI.|
| clientID | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci KIMLIĞI.|
| clientSecret | *xxxxxxxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci gizli anahtarı. |
| resourceGroupName | *testrg* | Kümenizi içeren kaynak grubunun adı.|
| clusterName | *mykustocluster* | Kümenizin adı.|
| Dosyasında | *mykustodatabase* | Veritabanınızın adı.|
| Princıpalassignmentname | *databasePrincipalAssignment1* | Veritabanı asıl kaynağınızın adı.|
| PrincipalId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kullanıcı e-postası, uygulama KIMLIĞI veya güvenlik grubu adı olabilen sorumlu KIMLIĞI.|
| role | *Yöneticileri* | Asıl veritabanı rolü ' Admin', ' Ingestor ', ' Monitor ', ' user ', ' Unkısıttedgörüntüleyiciler ', ' Viewer ' olabilir.|
| tenantIdForPrincipal | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Sorumlunun kiracı KIMLIĞI.|
| principalType | *Uygulama* | ' User ', ' App ' veya ' Group ' olabilen asıl öğe türü|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini node kitaplığını kullanarak verileri alma](node-ingest-data.md)
