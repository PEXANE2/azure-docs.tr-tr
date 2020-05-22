---
title: Yönetilen örnek için var olan bir sanal ağı yapılandırma
description: Bu makalede, Azure SQL veritabanı yönetilen örneğini dağıtabileceğiniz mevcut bir sanal ağın ve alt ağın nasıl yapılandırılacağı açıklanır.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: 4e678edad2c59205e76598991b36d296404a3163
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773634"
---
# <a name="configure-an-existing-virtual-network-for-azure-sql-database-managed-instance"></a>Mevcut sanal ağı Azure SQL Veritabanı Yönetilen Örneği için yapılandırma

Azure SQL Veritabanı Yönetilen Örneği bir Azure [sanal ağında](../virtual-network/virtual-networks-overview.md) ve yalnızca Yönetilen Örneklere ayrılmış bir alt ağda dağıtılmalıdır. [Yönetilen Örnek sanal ağ gereksinimlerine](sql-database-managed-instance-connectivity-architecture.md#network-requirements) uygun şekilde yapılandırılması halinde mevcut sanal ağı ve alt ağı kullanabilirsiniz.

Aşağıdaki durumlardan biri sizin için geçerliyse, bu makalede açıklanan betiği kullanarak ağınızı doğrulayabilir ve değiştirebilirsiniz:

- Hala yapılandırılmamış yeni bir alt ağa sahipsiniz.
- Alt ağın [gereksinimlere](sql-database-managed-instance-connectivity-architecture.md#network-requirements)göre hizalandığından emin değilsiniz.
- Değişiklikleri yaptıktan sonra alt ağın [ağ gereksinimleriyle](sql-database-managed-instance-connectivity-architecture.md#network-requirements) uyumlu olup olmadığını kontrol etmek istiyorsunuz.

> [!Note]
> Yönetilen bir örneği yalnızca Azure Resource Manager dağıtım modeliyle oluşturulan sanal ağlarda oluşturabilirsiniz. Klasik dağıtım modeli aracılığıyla oluşturulan Azure sanal ağları desteklenmez. [Yönetilen örnekler için alt ağın boyutunu belirleme](sql-database-managed-instance-determine-size-vnet-subnet.md) makalesindeki yönergeleri izleyerek alt ağ boyutunu hesaplayın. İçindeki kaynakları dağıttıktan sonra alt ağı yeniden boyutlandıramazsınız.
>
> Yönetilen bir örnek oluşturulduktan sonra, yönetilen örneği veya VNet 'i başka bir kaynak grubuna veya aboneliğe taşımak desteklenmez.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Var olan sanal ağı doğrulama ve değiştirme

Var olan bir alt ağ içinde Yönetilen Örnek oluşturmak isterseniz alt ağı hazırlamak için aşağıdaki PowerShell betiğini kullanmanız önerilir:

```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/delegate-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/delegateSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```

Betik, alt ağı üç adımda hazır duruma getirir:

1. Doğrula: yönetilen örnek ağ gereksinimleri için seçili sanal ağı ve alt ağı doğrular.
2. Onayla: kullanıcıya, yönetilen örnek dağıtımı için alt ağı hazırlamak üzere yapılması gereken bir değişiklik kümesi gösterilir. Ayrıca onay ister.
3. Hazırlama: sanal ağı ve alt ağı düzgün şekilde yapılandırır.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md).
- Bir sanal ağ oluşturmayı, yönetilen bir örnek oluşturmayı ve bir veritabanını bir veritabanı yedeklemesinden geri yüklemeyi gösteren bir öğretici için bkz. [Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).
- DNS sorunları için bkz. [Özel BIR DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
