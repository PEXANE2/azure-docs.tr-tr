---
title: Yönetilen örneğinvarı olan bir sanal ağı yapılandırma
description: Bu makalede, Azure SQL Veritabanı Yönetilen Örneği'ni dağıtabileceğiniz varolan bir sanal ağ ve alt ağ nasıl yapılandırılabildiğiniz açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 03/17/2020
ms.openlocfilehash: 50b832baa9253f47b5f10980ae1764c9425ed4d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476958"
---
# <a name="configure-an-existing-virtual-network-for-azure-sql-database-managed-instance"></a>Mevcut sanal ağı Azure SQL Veritabanı Yönetilen Örneği için yapılandırma

Azure SQL Veritabanı Yönetilen Örnek, bir Azure [sanal ağında](../virtual-network/virtual-networks-overview.md) ve yalnızca Yönetilen Örnekler için ayrılmış alt ağ da dağıtılmalıdır. [Yönetilen Örnek sanal ağ gereksinimlerine](sql-database-managed-instance-connectivity-architecture.md#network-requirements)göre yapılandırılırsa varolan sanal ağı ve alt ağı kullanabilirsiniz.

Aşağıdaki durumlardan biri sizin için geçerliyse, bu makalede açıklanan komut dosyasını kullanarak ağınızı doğrulayabilir ve değiştirebilirsiniz:

- Hala yapılandırılmamış yeni bir alt ağınız var.
- Alt ağın [gereksinimlerle](sql-database-managed-instance-connectivity-architecture.md#network-requirements)uyumlu olduğundan emin değilseniz.
- Değişiklik yaptıktan sonra alt ağın ağ [gereksinimlerine](sql-database-managed-instance-connectivity-architecture.md#network-requirements) uygun olup olmadığını denetlemek istiyorsunuz.

> [!Note]
> Yönetilen Örnek'i yalnızca Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla oluşturulan sanal ağlarda oluşturabilirsiniz. Klasik dağıtım modeli yle oluşturulan Azure sanal ağları desteklenmez. Yönetilen Örnekler makalesi için alt [net boyutunu](sql-database-managed-instance-determine-size-vnet-subnet.md) belirleyin'deki yönergeleri izleyerek alt net boyutunu hesaplayın. İçerideki kaynakları dağıttıktan sonra alt ağı yeniden boyutlandıramazsınız.
>
> Yönetilen bir örnek oluşturulduktan sonra, yönetilen örneğin veya VNet'in başka bir kaynak grubuna taşınması veya aboneliği desteklenmez.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Varolan bir sanal ağı doğrulama ve değiştirme

Varolan bir alt ağ içinde Yönetilen Örnek oluşturmak istiyorsanız, alt ağı hazırlamak için aşağıdaki PowerShell komut dosyasını öneririz:

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

Komut dosyası alt ağı üç adımda hazırlar:

1. Doğrulama: Yönetilen Örnek ağ gereksinimleri için seçilen sanal ağı ve alt ağı doğrular.
2. Onayla: Kullanıcıya, alt ağı Yönetilen Örnek dağıtımına hazırlamak için yapılması gereken bir dizi değişiklik gösterir. Ayrıca rıza da ister.
3. Hazırla: Sanal ağı ve alt ağı düzgün bir şekilde yapılandırır.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için [bkz.](sql-database-managed-instance.md)
- Sanal ağ oluşturmayı, Yönetilen Örnek'i nasıl oluşturup veritabanı yedeklemesinden veritabanını geri yükleyin, [bkz.](sql-database-managed-instance-get-started.md)
- DNS sorunları için [bkz.](sql-database-managed-instance-custom-dns.md)
