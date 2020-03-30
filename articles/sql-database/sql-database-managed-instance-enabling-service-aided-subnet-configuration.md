---
title: Azure SQL Veritabanı Yönetilen Örneği için hizmet destekli alt ağ yapılandırmasını etkinleştirme
description: Azure SQL Veritabanı Yönetilen Örneği için hizmet destekli alt ağ yapılandırmasını etkinleştirme
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.date: 03/12/2020
ms.openlocfilehash: efc2b8578651f68d052f227694f85348853e191f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79533275"
---
# <a name="enabling-service-aided-subnet-configuration-for-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği için hizmet destekli alt ağ yapılandırmasını etkinleştirme
Hizmet destekli subnet yapılandırması, yönetilen örnekleri barındıran alt ağlar için otomatik ağ yapılandırma yönetimi sağlar. Servis destekli subnet yapılandırması ile kullanıcı verilere erişimin tam kontrolünde kalır (TDS trafik akışları) yönetilen örnek, SLA'yı yerine getirmek için yönetim trafiğinin kesintisiz akışını sağlamak için sorumluluk alır.

Otomatik olarak yapılandırılan ağ güvenlik grupları ve rota tablosu kuralları müşteri tarafından görülebilir ve _Microsoft.Sql-managedInstances_UseOnly__öneki yle açıklama yapılır.

Hizmet destekli yapılandırma, kaynak sağlayıcısı için `Microsoft.Sql/managedInstances` [alt ağ delegasyonuna](../virtual-network/subnet-delegation-overview.md) açtığınızda otomatik olarak etkinleştirilir.

> [!IMPORTANT] 
> Alt ağ delegasyonu açık olduğunda, alt ağdan son sanal kümeyi kaldırana kadar kapatamazsınız. Sanal kümenin nasıl silinir hakkında daha fazla bilgi için aşağıdaki [makaleye](sql-database-managed-instance-delete-virtual-cluster.md#delete-virtual-cluster-from-the-azure-portal)bakın.

> [!NOTE] 
> Hizmet destekli alt ağ yapılandırması, 1 Mayıs 2020'den itibaren SLA'yı korumak için temel bir özellik olduğundan, yönetilen örnek kaynak sağlayıcısına devredilmedi. 1 Temmuz 2020'de yönetilen örnekleri içeren tüm alt ağlar otomatik olarak yönetilen örnek kaynak sağlayıcısına devredilecektir. 

## <a name="enabling-subnet-delegation-for-new-deployments"></a>Yeni dağıtımlar için alt ağ delegasyonuetkinleştirme
Yönetilen örneği boş alt ağa dağıtmak için aşağıdaki `Microsoft.Sql/managedInstances` [makalede](../virtual-network/manage-subnet-delegation.md)açıklandığı gibi kaynak sağlayıcısına temsilci göndermeniz gerekir. _Lütfen başvurulan makalenin `Microsoft.DBforPostgreSQL/serversv2` kaynak sağlayıcısını kullandığını lütfen unutmayın. Bunun yerine kaynak `Microsoft.Sql/managedInstances` sağlayıcısı kullanmanız gerekir._

## <a name="enabling-subnet-delegation-for-existing-deployments"></a>Varolan dağıtımlar için alt ağ delegasyonuetkinleştirme

Mevcut yönetilen örnek dağıtımınız için alt ağ delegasyonu etkinleştirmek için sanal ağ alt netinin nereye yerleştirildiğini bulmanız gerekir. 

Bunu öğrenmek için `Virtual network/subnet` yönetilen `Overview` örneğinportal bıçak kontrol edebilirsiniz.

Alternatif olarak, bunu öğrenmek için aşağıdaki PowerShell komutlarını çalıştırabilirsiniz. **Abonelik kimliğinizi** abonelik kimliğinizle değiştirin. Ayrıca, yönetilen örneğiniz için **rg adını** kaynak grubuyla değiştirin ve **mi-adı** yönetilen örneğinizin adıyla değiştirin.

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi.SubnetId
```

Yönetilen örnek alt netini bulduğunuzda, aşağıdaki `Microsoft.Sql/managedInstances` [makalede](../virtual-network/manage-subnet-delegation.md)açıklandığı gibi kaynak sağlayıcısına temsilci vermeniz gerekir. _Lütfen başvurulan makalenin `Microsoft.DBforPostgreSQL/serversv2` kaynak sağlayıcısını kullandığını lütfen unutmayın. Bunun yerine kaynak `Microsoft.Sql/managedInstances` sağlayıcısı kullanmanız gerekir._


> [!IMPORTANT]
> Hizmet destekli yapılandırmayı etkinleştirmek, alt ağda zaten bulunan yönetilen örnekler için bağlantıda başarısızlığa veya kesintiye neden olmaz.
