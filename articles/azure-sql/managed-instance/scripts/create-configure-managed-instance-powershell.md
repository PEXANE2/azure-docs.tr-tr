---
title: 'PowerShell: örnek oluşturma'
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örneği oluşturmak için örnek betik Azure PowerShell
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 03/25/2019
ms.openlocfilehash: 9dfe6177ed46f133772a46930d1e56c6007b33f3
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84053978"
---
# <a name="use-powershell-to-create-an-azure-sql-managed-instance"></a>PowerShell kullanarak Azure SQL yönetilen örneği oluşturma
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqlmi.md)]

Bu PowerShell betiği örneği, yeni bir sanal ağ içindeki ayrılmış bir alt ağda Azure SQL yönetilen örneği oluşturur. Ayrıca, bir yol tablosu ve sanal ağ için bir ağ güvenlik grubu yapılandırır. Betik başarıyla çalıştırıldığında, SQL yönetilen örneğine sanal ağ içinden veya şirket içi bir ortamdan erişilebilir. Azure [SQL veritabanı yönetilen örneğine bağlanmak için](../connect-vm-instance-configure.md) bkz. Azure VM 'yi yapılandırma ve [ŞIRKET Içinden Azure SQL yönetilen örneği ile noktadan siteye bağlantı yapılandırma](../point-to-site-p2s-configure.md).

> [!IMPORTANT]
> Sınırlamalar için bkz. [Desteklenen bölgeler](../resource-limits.md#supported-regions) ve [desteklenen Abonelik türleri](../resource-limits.md#supported-subscription-types).

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici AZ PowerShell 1.4.0 veya üstünü gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/managed-instance/create-and-configure-managed-instance.ps1 "Create managed instance")]

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
| [New-Azsqlınstance](/powershell/module/az.sql/New-AzSqlInstance) | Azure SQL yönetilen örneği oluşturur |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL yönetilen örnek PowerShell betiği örnekleri, [Azure SQL yönetilen örnek PowerShell betikleri](../../database/powershell-script-content-guide.md)içinde bulunabilir.
