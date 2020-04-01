---
title: PowerShell örneği- Failover grubu - Azure SQL Veritabanı yönetilen örnek
description: Azure Sql Veritabanı yönetilen bir örnek oluşturmak, bir başarısız lık grubuna eklemek ve başarısız olmayı test etmek için Azure PowerShell örnek komut dosyası.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 07/16/2019
ms.openlocfilehash: e50877f6f3194885b139683fe865144384716b48
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73691750"
---
# <a name="use-powershell-to-add-an-azure-sql-database-managed-instance-to-a-failover-group"></a>Bir başarısız gruba Azure SQL Veritabanı yönetilen bir örnek eklemek için PowerShell'i kullanın 

Bu PowerShell komut dosyası örneği iki yönetilen örnek oluşturur, bunları bir başarısızlık grubuna ekler ve ardından birincil yönetilen örnekten ikincil yönetilen örneğe kadar başarısız testleri uygular. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici AZ PowerShell 1.4.0 veya daha sonra gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-scripts"></a>Örnek komut dosyaları

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/failover-groups/add-managed-instance-to-failover-group-az-ps.ps1 "Add managed instance to a failover group")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın. Kaynak grubunu iki kez kaldırmanız gerekir. Kaynak grubunu ilk kez kaldırma yönetilen örnek ve sanal kümeleri kaldırır, ancak `Remove-AzResourceGroup : Long running operation failed with status 'Conflict'.`daha sonra hata iletisi ile başarısız olur. Kaynak grubunun yanı sıra kalan kaynakları kaldırmak için Remove-AzResourceGroup komutunu ikinci kez çalıştırın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Bir Azure kaynak grubu oluşturur.  |
| [Yeni-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Sanal ağ oluşturur.  |
| [Ekle-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig) | Sanal ağa bir alt ağ yapılandırması ekler. | 
| [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork) | Bir kaynak grubundaki sanal ağı alır. | 
| [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig) | Sanal ağda bir alt ağ alır. | 
| [Yeni-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup) | Ağ güvenlik grubu oluşturur. | 
| [Yeni-AzRouteTable](/powershell/module/az.network/new-azroutetable) | Bir rota tablosu oluşturur. |
| [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) | Sanal ağ için bir alt ağ yapılandırması güncelleştirir.  |
| [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork) | Sanal ağı güncelleştirir.  |
| [Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup) | Bir ağ güvenlik grubu alır. |
| [Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Ağ güvenlik grubuna ağ güvenliği kuralı yapılandırması ekler. |
| [Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup) | Ağ güvenlik grubunu güncelleştirir.  | 
| [Ekle-AzRouteConfig](/powershell/module/az.network/add-azrouteconfig) | Rota tablosuna bir rota ekler. |
| [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable) | Rota tablosunu güncelleştirir.  |
| [Yeni-AzSqlInstance](/powershell/module/az.sql/new-azsqlinstance) | Azure SQL Veritabanı yönetilen bir örnek oluşturur.  |
| [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance)| Azure SQL Yönetilen Veritabanı Örneği hakkındaki bilgileri verir. |
| [Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Genel bir IP adresi oluşturur.  | 
| [Yeni-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig) | Sanal Ağ Ağ Geçidi için IP Yapılandırması Oluşturur |
| [Yeni-AzVirtualNetworkAğ Ağ Geçidi](/powershell/module/az.network/new-azvirtualnetworkgateway) | Sanal Ağ Ağ Geçidi Oluşturur |
| [New-AzVirtualNetworkGatewayConnection](/powershell/module/az.network/new-azvirtualnetworkgatewayconnection) | İki sanal ağ ağ geçidi arasında bir bağlantı oluşturur.   |
| [Yeni-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/new-azsqldatabaseinstancefailovergroup)| Yeni bir Azure SQL Veritabanı yönetilen örnek failover grubu oluşturur.  |
| [Get-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/get-azsqldatabaseinstancefailovergroup) | Yönetilen örnek failover gruplarını alır veya listeler.| 
| [Switch-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/switch-azsqldatabaseinstancefailovergroup) | Yönetilen bir örnek failover grubunun başarısız bir yürütmesini yürütür. | 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu kaldırır. | 

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
