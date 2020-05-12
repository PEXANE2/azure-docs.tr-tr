---
title: PowerShell örneği-Azure SQL veritabanı 'nda yönetilen bir örnek oluşturma
description: Azure SQL veritabanı 'nda yönetilen bir örnek oluşturmak için örnek betik Azure PowerShell
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/25/2019
ms.openlocfilehash: a4ba4d69b560307e52c277f5c695bbba718cd1d3
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2020
ms.locfileid: "83005838"
---
# <a name="use-powershell-to-create-an-azure-sql-database-managed-instance"></a>PowerShell kullanarak Azure SQL veritabanı yönetilen örneği oluşturma

Bu PowerShell betiği örneği, yeni bir sanal ağ içindeki ayrılmış bir alt ağda Azure SQL veritabanı yönetilen örneği oluşturur. Ayrıca, bir yol tablosu ve sanal ağ için bir ağ güvenlik grubu yapılandırır. Betik başarıyla çalıştırıldığında, yönetilen örneğe sanal ağ içinden veya şirket içi bir ortamdan erişilebilir. Azure [SQL veritabanı yönetilen örneğine bağlanmak için](../sql-database-managed-instance-configure-vm.md) bkz. Azure VM 'yi yapılandırma ve [ŞIRKET Içi Azure SQL veritabanı yönetilen örneği ile noktadan siteye bağlantı yapılandırma](../sql-database-managed-instance-configure-p2s.md).

> [!IMPORTANT]
> Sınırlamalar için bkz. [Desteklenen bölgeler](../sql-database-managed-instance-resource-limits.md#supported-regions) ve [desteklenen Abonelik türleri](../sql-database-managed-instance-resource-limits.md#supported-subscription-types).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici AZ PowerShell 1.4.0 veya üstünü gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/managed-instance/create-and-configure-managed-instance.ps1 "Create managed instance")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutlardan bazılarını kullanır. Aşağıdaki tabloda kullanılan ve diğer komutlar hakkında daha fazla bilgi için, komuta özgü belgelere yönelik bağlantılara tıklayın.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur.
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Bir sanal ağ oluşturur |
| [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Add-AzVirtualNetworkSubnetConfig) | Bir sanal ağa alt ağ yapılandırması ekler |
| [Get-AzVirtualNetwork](/powershell/module/az.network/Get-AzVirtualNetwork) | Bir kaynak grubundaki sanal ağı alır |
| [Set-AzVirtualNetwork](/powershell/module/az.network/Set-AzVirtualNetwork) | Bir sanal ağın hedef durumunu ayarlar |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Get-AzVirtualNetworkSubnetConfig) | Bir sanal ağ içinde bir alt ağ alır |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Set-AzVirtualNetworkSubnetConfig) | Bir sanal ağdaki alt ağ yapılandırması için hedef durumu yapılandırır |
| [New-AzRouteTable](/powershell/module/az.network/New-AzRouteTable) | Yol tablosu oluşturur |
| [Get-AzRouteTable](/powershell/module/az.network/Get-AzRouteTable) | Rota tablolarını alır |
| [Set-AzRouteTable](/powershell/module/az.network/Set-AzRouteTable) | Bir yol tablosu için hedef durumunu ayarlar |
| [New-Azsqlınstance](/powershell/module/az.sql/New-AzSqlInstance) | Azure SQL veritabanı yönetilen örneği oluşturur |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
