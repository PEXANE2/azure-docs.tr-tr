---
title: SQL yönetilen örnek sanal kümesindeki sanal ağ DNS sunucuları ayarını eşitler
description: SQL yönetilen örneği sanal kümesinde sanal ağ DNS sunucularının nasıl eşitleneceğini öğrenin.
services: sql-database
ms.service: sql-managed-instance
author: srdan-bozovic-msft
ms.author: srbozovi
ms.topic: how-to
ms.date: 01/17/2021
ms.openlocfilehash: b95afe513dba2f1da9556b27ec17bcccc9fe88e1
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102173560"
---
# <a name="synchronize-virtual-network-dns-servers-setting-on-sql-managed-instance-virtual-cluster"></a>SQL yönetilen örnek sanal kümesindeki sanal ağ DNS sunucuları ayarını eşitler
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, SQL yönetilen örnek sanal kümesinde sanal ağ DNS sunucularının ne zaman ve nasıl eşitleneceği açıklanmaktadır.

## <a name="when-to-synchronize-the-dns-setting"></a>DNS ayarı ne zaman eşitlenir?

Özel ana bilgisayar adlarının SQL Yönetilen Örneğinden çözümlenmesini gerektiren birkaç senaryo (örneğin veritabanı postası, bulutunuzda veya hibrit ortamınızda bulunan diğer SQL Server örnekleriyle bağlantılı sunucular) vardır. Böyle bir durumda Azure içinde özel DNS yapılandırmanız gerekir. Ayrıntılı bilgi için bkz. [Azure SQL Yönetilen Örneği için özel bir DNS yapılandırma](custom-dns-configure.md).

Bu değişiklik, yönetilen örnek barındıran [sanal küme](connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture) oluşturulduktan sonra uygulanırsa, sanal kümedeki DNS sunucuları ayarını sanal ağ yapılandırmasıyla eşitlemeniz gerekir.

> [!IMPORTANT]
> DNS sunucuları ayarını eşitlemek, sanal kümede barındırılan tüm yönetilen örnekleri etkileyecektir.

## <a name="how-to-synchronize-the-dns-setting"></a>DNS ayarı nasıl eşitlenir

### <a name="azure-rbac-permissions-required"></a>Azure RBAC izinleri gerekli

DNS sunucusu yapılandırmasını eşitleme Kullanıcı tarafından aşağıdaki Azure rollerinden birine sahip olmalıdır:

- Abonelik sahibi rolü veya
- Yönetilen örnek katılımcısı rolü veya
- Aşağıdaki izne sahip özel rol:
  - `Microsoft.Sql/virtualClusters/updateManagedInstanceDnsServers/action`

### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

DNS sunucuları ayarının güncelleştirildiği sanal ağı alın.

```PowerShell
$ResourceGroup = 'enter resource group of virtual network'
$VirtualNetworkName = 'enter virtual network name'
$virtualNetwork = Get-AzVirtualNetwork -ResourceGroup $ResourceGroup -Name $VirtualNetworkName
```
Alt ağdaki tüm Sanal kümeler için DNS sunucuları yapılandırmasını eşleştirmek üzere [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction) PowerShell komutunu kullanın.

```PowerShell
Get-AzSqlVirtualCluster `
    | where SubnetId -match $virtualNetwork.Id `
    | select Id `
    | Invoke-AzResourceAction -Action updateManagedInstanceDnsServers -Force
```
### <a name="use-the-azure-cli"></a>Azure CLI kullanma

DNS sunucuları ayarının güncelleştirildiği sanal ağı alın.

```Azure CLI
resourceGroup="auto-failover-group"
virtualNetworkName="vnet-fog-eastus"
virtualNetwork=$(az network vnet show -g $resourceGroup -n $virtualNetworkName --query "id" -otsv)
```

Alt ağdaki tüm Sanal kümelerin DNS sunucuları yapılandırmasını eşitlemeye yönelik [az Resource Invoke-Action](/cli/azure/resource#az_resource_invoke_action) Azure CLI komutunu kullanın.

```Azure CLI
az sql virtual-cluster list --query "[? contains(subnetId,'$virtualNetwork')].id" -o tsv \
    | az resource invoke-action --action updateManagedInstanceDnsServers --ids @-
```
## <a name="next-steps"></a>Sonraki adımlar

- Özel DNS yapılandırma hakkında daha fazla bilgi edinin [Azure SQL yönetilen örneği için özel DNS yapılandırma](custom-dns-configure.md).
- Genel bakış için bkz. [Azure SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
