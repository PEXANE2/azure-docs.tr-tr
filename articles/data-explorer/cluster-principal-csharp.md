---
title: Kullanarak Azure Veri Gezgini için küme sorumlularını eklemeC#
description: Bu makalede kullanarak C#Azure Veri Gezgini için küme sorumlularını eklemeyi öğreneceksiniz.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: e6c3970890dfe2c669dee1acf631e9dd45ab1085
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76965066"
---
# <a name="add-cluster-principals-for-azure-data-explorer-by-using-c"></a>Kullanarak Azure Veri Gezgini için küme sorumlularını eklemeC#

> [!div class="op_single_selector"]
> * [C#](cluster-principal-csharp.md)
> * [Python](cluster-principal-python.md)
> * [Azure Resource Manager şablonu](cluster-principal-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Bu makalede kullanarak C#Azure Veri Gezgini için küme sorumlularını eklersiniz.

## <a name="prerequisites"></a>Ön koşullar

* Visual Studio 2019 yüklü değilse, **ücretsiz** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/)' ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.
* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Bir küme oluşturun](create-cluster-database-csharp.md).

## <a name="install-c-nuget"></a>NuGet C# 'i yükler

* [Microsoft. Azure. Management. kusto](https://www.nuget.org/packages/Microsoft.Azure.Management.Kusto/)uygulamasını yükler.
* Kimlik doğrulaması için [Microsoft. Rest. ClientRuntime. Azure. Authentication](https://www.nuget.org/packages/Microsoft.Rest.ClientRuntime.Azure.Authentication) 'ı yükler.

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-a-cluster-principal"></a>Küme sorumlusu ekleme

Aşağıdaki örnek, programlı olarak bir küme sorumlusu nasıl ekleneceğini gösterir.

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
//The cluster that is created as part of the Prerequisites
var clusterName = "mykustocluster";
string principalAssignmentName = "clusterPrincipalAssignment1";
string principalId = "xxxxxxxx";//User email, application ID, or security group name
string role = "AllDatabasesAdmin";//AllDatabasesAdmin or AllDatabasesViewer
string tenantIdForPrincipal = tenantId;
string principalType = "App";//User, App, or Group

var clusterPrincipalAssignment = new ClusterPrincipalAssignment(principalId, role, principalType, tenantId: tenantIdForPrincipal);
await kustoManagementClient.ClusterPrincipalAssignments.CreateOrUpdateAsync(resourceGroupName, clusterName, principalAssignmentName, clusterPrincipalAssignment);
```

|**Ayar** | **Önerilen değer** | **Alan açıklaması**|
|---|---|---|
| tenantId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracı KIMLIĞINIZ. Dizin KIMLIĞI olarak da bilinir.|
| subscriptionId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kaynak oluşturma için kullandığınız abonelik KIMLIĞI.|
| clientID | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci KIMLIĞI.|
| clientSecret | *xxxxxxxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci gizli anahtarı. |
| resourceGroupName | *testrg* | Kümenizi içeren kaynak grubunun adı.|
| clusterName | *mykustocluster* | Kümenizin adı.|
| Princıpalassignmentname | *clusterPrincipalAssignment1* | Küme asıl kaynağınızın adı.|
| PrincipalId | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kullanıcı e-postası, uygulama KIMLIĞI veya güvenlik grubu adı olabilen sorumlu KIMLIĞI.|
| role | *AllDatabasesAdmin* | ' AllDatabasesAdmin' veya ' AllDatabasesViewer ' olabilen kümeniz için asıl rol rolü.|
| tenantIdForPrincipal | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Sorumlunun kiracı KIMLIĞI.|
| principalType | *Uygulama* | ' User ', ' App ' veya ' Group ' olabilen asıl öğe türü|

## <a name="next-steps"></a>Sonraki adımlar

* [Veritabanı sorumluları Ekle](database-principal-csharp.md)
