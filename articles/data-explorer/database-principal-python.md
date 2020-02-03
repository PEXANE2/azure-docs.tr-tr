---
title: Python kullanarak Azure Veri Gezgini yönelik veritabanı sorumlularını ekleme
description: Bu makalede, Python kullanarak Azure Veri Gezgini için veritabanı sorumlularını eklemeyi öğreneceksiniz.
author: lucygoldbergmicrosoft
ms.author: lugoldbe
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: 8b9c4f4d5427b326c273558db0bff808068b192a
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76965014"
---
# <a name="add-database-principals-for-azure-data-explorer-by-using-python"></a>Python kullanarak Azure Veri Gezgini yönelik veritabanı sorumlularını ekleme

> [!div class="op_single_selector"]
> * [C#](database-principal-csharp.md)
> * [Python](database-principal-python.md)
> * [Azure Resource Manager şablonu](database-principal-resource-manager.md)

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Bu makalede, Python kullanarak Azure Veri Gezgini için veritabanı sorumlularını eklersiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Küme ve veritabanı oluşturma](create-cluster-database-python.md)

## <a name="install-python-package"></a>Python paketini yükle

Azure Veri Gezgini Python paketini yüklemek için (kusto), yolunda Python içeren bir komut istemi açın. Şu komutu çalıştırın:

```
pip install azure-common
pip install azure-mgmt-kusto
```

[!INCLUDE [data-explorer-authentication](../../includes/data-explorer-authentication.md)]

## <a name="add-a-database-principal"></a>Asıl veritabanı Ekle

Aşağıdaki örnek, programlı olarak bir veritabanı sorumlusunu nasıl ekleneceğini gösterir.

```Python
from azure.mgmt.kusto import KustoManagementClient
from azure.mgmt.kusto.models import DatabasePrincipalAssignment
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
kusto_management_client = KustoManagementClient(credentials, subscription_id)

resource_group_name = "testrg"
#The cluster and database that is created as part of the Prerequisites
cluster_name = "mykustocluster"
database_name = "mykustodatabase"
principal_assignment_name = "clusterPrincipalAssignment1"
#User email, application ID, or security group name
principal_id = "xxxxxxxx"
#AllDatabasesAdmin or AllDatabasesViewer
role = "AllDatabasesAdmin"
tenant_id_for_principal = tenantId
#User, App, or Group
principal_type = "App"

#Returns an instance of LROPoller, check https://docs.microsoft.com/python/api/msrest/msrest.polling.lropoller?view=azure-python
poller = kusto_management_client.database_principal_assignments.create_or_update(resource_group_name=resource_group_name, cluster_name=cluster_name, database_name=database_name, principal_assignment_name= principal_assignment_name, parameters=DatabasePrincipalAssignment(principal_id=principal_id, role=role, tenant_id=tenant_id_for_principal, principal_type=principal_type))
```

|**Ayar** | **Önerilen değer** | **Alan açıklaması**|
|---|---|---|
| tenant_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracı KIMLIĞINIZ. Dizin KIMLIĞI olarak da bilinir.|
| subscription_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kaynak oluşturma için kullandığınız abonelik KIMLIĞI.|
| client_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci KIMLIĞI.|
| client_secret | *xxxxxxxxxxxxxx* | Kiracınızdaki kaynaklara erişebilen uygulamanın istemci gizli anahtarı. |
| resource_group_name | *testrg* | Kümenizi içeren kaynak grubunun adı.|
| cluster_name | *mykustocluster* | Kümenizin adı.|
| database_name | *mykustodatabase* | Veritabanınızın adı.|
| principal_assignment_name | *databasePrincipalAssignment1* | Veritabanı asıl kaynağınızın adı.|
| principal_id | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Kullanıcı e-postası, uygulama KIMLIĞI veya güvenlik grubu adı olabilen sorumlu KIMLIĞI.|
| role | *Yöneticileri* | Asıl veritabanı rolü ' Admin', ' Ingestor ', ' Monitor ', ' user ', ' Unkısıttedgörüntüleyiciler ', ' Viewer ' olabilir.|
| tenant_id_for_principal | *xxxxxxxx-xxxxx-xxxx-xxxx-xxxxxxxxx* | Sorumlunun kiracı KIMLIĞI.|
| principal_type | *Uygulama* | ' User ', ' App ' veya ' Group ' olabilen asıl öğe türü|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Python kitaplığını kullanarak verileri alma](python-ingest-data.md)
