---
title: PowerShell örneği - Azure SQL Veritabanı'nda yönetilen bir örnek oluşturma
description: Azure SQL Veritabanı'nda yönetilen bir örnek oluşturmak için Azure PowerShell örnek komut dosyası
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
ms.openlocfilehash: 3e72a2f6754ad8e9c5bcfabe7eeee299468fa8f3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73691634"
---
# <a name="use-powershell-to-create-an-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı yönetilen bir örnek oluşturmak için PowerShell'i kullanın

Bu PowerShell komut dosyası örneği, yeni bir sanal ağ içinde özel bir alt ağda yönetilen bir Azure SQL Veritabanı örneği oluşturur. Ayrıca, sanal ağ için bir rota tablosu nu ve ağ güvenlik grubunu da yapılandırır. Komut dosyası başarıyla çalıştırıldıktan sonra, yönetilen örnek sanal ağ içinden veya şirket içi ortamdan erişilebilir. Azure [SQL Veritabanı Yönetilen Örneği'ne bağlanmak için Azure VM'yi yapılandırın](../sql-database-managed-instance-configure-vm.md) ve şirket [içinden Azure SQL Veritabanı Yönetilen Örneği'ne noktadan siteye bağlantı yapılandırın.](../sql-database-managed-instance-configure-p2s.md)

> [!IMPORTANT]
> Sınırlamalar için [desteklenen bölgelere](../sql-database-managed-instance-resource-limits.md#supported-regions) ve [desteklenen abonelik türlerine](../sql-database-managed-instance-resource-limits.md#supported-subscription-types)bakın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici AZ PowerShell 1.4.0 veya daha sonra gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/managed-instance/create-and-configure-managed-instance.ps1 "Create managed instance")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur.
| [Yeni-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur |
| [Ekle-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Add-AzVirtualNetworkSubnetConfig) | Sanal ağa alt ağ yapılandırması ekler |
| [Get-AzVirtualNetwork](/powershell/module/az.network/Get-AzVirtualNetwork) | Kaynak grubunda sanal ağ sağlar |
| [Set-AzVirtualNetwork](/powershell/module/az.network/Set-AzVirtualNetwork) | Sanal ağ için hedef durumu ayarlar |
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Get-AzVirtualNetworkSubnetConfig) | Sanal ağda bir alt ağ alır |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/Set-AzVirtualNetworkSubnetConfig) | Sanal ağdaki alt ağ yapılandırması için hedef durumunu yapılandırır |
| [Yeni-AzRouteTable](/powershell/module/az.network/New-AzRouteTable) | Rota tablosu oluşturur |
| [Get-AzRouteTable](/powershell/module/az.network/Get-AzRouteTable) | Rota tabloları alır |
| [Set-AzRouteTable](/powershell/module/az.network/Set-AzRouteTable) | Rota tablosu için hedef durumunu ayarlar |
| [Yeni-AzSqlInstance](/powershell/module/az.sql/New-AzSqlInstance) | Azure SQL Veritabanı yönetilen örneği oluşturur |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
