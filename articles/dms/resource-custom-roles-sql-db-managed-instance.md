---
title: "Özel roller: Çevrimiçi SQL Server'dan SQL'e yönetilen örnek geçişleri"
titleSuffix: Azure Database Migration Service
description: SQL Server ile Azure SQL Veritabanı için özel rolleri kullanmayı öğrenin örnek çevrimiçi geçişler yönetildi.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 10/25/2019
ms.openlocfilehash: e9a1024ca3ab68841474ab051c029042df4915b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78254937"
---
# <a name="custom-roles-for-sql-server-to-sql-database-managed-instance-online-migrations"></a>SQL Server'dan SQL Veritabanı'na özel roller yönetilen örnek çevrimiçi geçişler

Azure Veritabanı Geçiş Hizmeti, Azure Hizmetleri ile etkileşim kurmak için bir APP Kimliği kullanır. APP Kimliği, Abonelik düzeyinde Katılımcı rolünü (birçok Kurumsal güvenlik departmanının izin vermeyeceği) veya Azure veritabanı Geçişleri Hizmeti'nin gerektirdiği belirli izinleri veren özel roller oluşturulmasını gerektirir. Azure Etkin Dizini'nde 2.000 özel rol sınırı olduğundan, APP ID tarafından özel olarak gereken tüm izinleri bir veya iki özel rolde birleştirmek ve ardından APP ID'ye belirli nesneler veya kaynak grupları (diğer yandan abonelik düzeyi). Özel rollerin sayısı önemli değilse, aşağıda açıklandığı gibi toplamda üç özel rol oluşturmak için özel rolleri kaynak türüne göre bölebilirsiniz.

Rol tanımı json dizesinin AtamaScopes bölümü, portaldaki **Rol Atama Arabirimi Ekle'de** izinlerin nerede görünacağını denetlemenize olanak tanır. UI'yi ek rollerle darmadağın etmeyi önlemek için büyük olasılıkla kaynak grubundaki ve hatta kaynak düzeyindeki rolü tanımlamak isteyebilirsiniz. Bunun gerçek rol atamasını gerçekleştirmediğini unutmayın.

## <a name="minimum-number-of-roles"></a>Minimum rol sayısı

Şu anda APP Kimliği için biri kaynak düzeyinde, diğeri abonelik düzeyinde olmak üzere en az iki özel rol oluşturmanızı öneririz.

> [!NOTE]
> Yeni SQL Veritabanı yönetilen örnek kodu Azure'a dağıtılırken, son özel rol gereksinimi sonunda kaldırılabilir.

**APP ID için Özel Rol**. Bu *rol, kaynak* veya *kaynak grubu* düzeyindeki Azure Veritabanı Geçiş Hizmeti geçişi için gereklidir (APP Kimliği hakkında daha fazla bilgi için, [kaynaklara erişebilen bir Azure REKLAM uygulaması ve hizmet ilkesi oluşturmak için portalı kullan makalesine](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)bakın).

```json
{
  "Name": "DMS Role - App ID",
  "IsCustom": true,
  "Description": "DMS App ID access to complete MI migrations",
  "Actions": [
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageaccounts/blobservices/read",
        "Microsoft.Storage/storageaccounts/blobservices/write",
        "Microsoft.Sql/managedInstances/read",
        "Microsoft.Sql/managedInstances/write",
        "Microsoft.Sql/managedInstances/databases/read",
        "Microsoft.Sql/managedInstances/databases/write",
        "Microsoft.Sql/managedInstances/databases/delete",
        "Microsoft.Sql/managedInstances/metrics/read",
        "Microsoft.DataMigration/locations/*",
        "Microsoft.DataMigration/services/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<subscription_id>/ResourceGroups/<StorageAccount_rg_name>",
    "/subscriptions/<subscription_id>/ResourceGroups/<ManagedInstance_rg_name>",
    "/subscriptions/<subscription_id>/ResourceGroups/<DMS_rg_name>",
  ]
}
```

**APP ID için özel rol - abonelik**. Bu rol, *abonelik* düzeyinde Azure Veritabanı Geçiş Hizmeti geçişi için gereklidir.

```json
{
  "Name": "DMS Role - App ID - Sub",
  "IsCustom": true,
  "Description": "DMS App ID access at subscription level to complete MI migrations",
  "Actions": [
        "Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<subscription_id>"
  ]
}
```

Yukarıdaki json üç metin dosyasında depolanmalıdır ve **Yeni-AzureRmRoleDefinition (AzureRM)** veya **New-AzRoleDefinition (AZ)** kullanarak rolleri oluşturmak için AzureRM, AZ PowerShell cmdlets veya Azure CLI kullanabilirsiniz.

Daha fazla bilgi için Azure [kaynakları için özel roller](https://docs.microsoft.com/azure/role-based-access-control/custom-roles)makalesine bakın.

Bu özel rolleri oluşturduktan sonra, kullanıcılara rol atamaları ve APP ID(ler) uygun kaynaklara veya kaynak gruplarına eklemeniz gerekir:

* "DMS Rolü - Uygulama Kimliği" rolü, geçişler için kullanılacak APP Kimliği'ne ve ayrıca Depolama Hesabı, Azure Veritabanı Geçiş Hizmeti örneği ve SQL Veritabanı yönetilen örnek kaynak düzeylerinde verilmelidir.
* "DMS Rolü - Uygulama Kimliği - Alt" rolü, abonelik düzeyinde APP Id'ye verilmelidir (kaynak veya kaynak grubunda verilen ler başarısız olur). Bu gereksinim, bir kod güncelleştirmesi dağıtılana kadar geçicidir.

## <a name="expanded-number-of-roles"></a>Genişletilmiş rol sayısı

Azure Etkin Dizininizdeki özel rollerin sayısı önemli değilse, toplam üç rol oluşturmanızı öneririz. Yine de "DMS Rolü - App ID - Alt" rolüne ihtiyacınız olacaktır, ancak yukarıdaki "DMS Rolü - Uygulama Kimliği" rolü kaynak türüne göre iki farklı role bölünür.

**SQL Veritabanı yönetilen örnek için APP ID için özel rol**

```json
{
  "Name": "DMS Role - App ID - SQL MI",
  "IsCustom": true,
  "Description": "DMS App ID access to complete MI migrations",
  "Actions": [
        "Microsoft.Sql/managedInstances/read",
        "Microsoft.Sql/managedInstances/write",
        "Microsoft.Sql/managedInstances/databases/read",
        "Microsoft.Sql/managedInstances/databases/write",
        "Microsoft.Sql/managedInstances/databases/delete",
        "Microsoft.Sql/managedInstances/metrics/read"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<subscription_id>/resourceGroups/<ManagedInstance_rg_name>"
  ]
}
```

**Depolama için APP Kimliği için özel rol**

```json
{
  "Name": "DMS Role - App ID - Storage",
  "IsCustom": true,
  "Description": "DMS App ID storage access to complete MI migrations",
  "Actions": [
"Microsoft.Storage/storageAccounts/read",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageaccounts/blobservices/read",
        "Microsoft.Storage/storageaccounts/blobservices/write"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/<subscription_id>/resourceGroups/<StorageAccount_rg_name>"
  ]
}
```

## <a name="role-assignment"></a>Rol ataması

Kullanıcılara/APP Kimliği'ne rol atamak için Azure portalını açın ve aşağıdaki adımları gerçekleştirin:

1. Kaynak grubuna veya kaynağa gidin (abonelikte verilmesi gereken rol hariç), **Access Denetimi'ne**gidin ve ardından yeni oluşturduğunuz özel rolleri bulmak için kaydırın.

2. Uygun rolü seçin, APP Kimliğini seçin ve değişiklikleri kaydedin.

  APP ID(lar) artık **Rol atamaları** sekmesinde listelenmiş olarak görünür.

## <a name="next-steps"></a>Sonraki adımlar

* Microsoft [Veritabanı Geçiş Kılavuzu'nda](https://datamigration.microsoft.com/)senaryonuz için geçiş kılavuzunu gözden geçirin.
