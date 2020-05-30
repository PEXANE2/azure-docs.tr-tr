---
title: 'PowerShell: Azure SQL yönetilen örneği için coğrafi yedeklemeyi geri yükleme'
description: Azure SQL yönetilen örnek veritabanını coğrafi olarak yedekli bir yedekten geri yüklemek için örnek betik Azure PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 07/03/2019
ms.openlocfilehash: eeb20f51b22805775ed491267b69d0ba60c90bc3
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220861"
---
# <a name="use-powershell-to-restore-an-azure-sql-managed-instance-database-to-another-geo-region"></a>Azure SQL yönetilen örnek veritabanını başka bir coğrafi bölgeye geri yüklemek için PowerShell 'i kullanma

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqlmi.md)]

Bu PowerShell betiği örneği, bir Azure SQL yönetilen örnek veritabanını uzak bir coğrafi bölgede (coğrafi geri yükleme) geri yükler.  

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure PowerShell 1.4.0 veya üzeri bir sürüm gerekir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

```azurepowershell-interactive
# Connect-AzAccount
# The SubscriptionId in which to create these objects
$SubscriptionId = '<put subscription_id here>'
# Set the information for your managed instance
$SourceResourceGroupName = "myResourceGroup-$(Get-Random)"
$SourceInstanceName = "myManagedInstance-$(Get-Random)"
$SourceDatabaseName = "myInstanceDatabase-$(Get-Random)"

# Set the information for your destination managed instance
$TargetResourceGroupName = "myTargetResourceGroup-$(Get-Random)"
$TargetInstanceName = "myTargetManagedInstance-$(Get-Random)"
$TargetDatabaseName = "myTargetInstanceDatabase-$(Get-Random)"

Connect-AzAccount
Set-AzContext -SubscriptionId $SubscriptionId

$backup = Get-AzSqlInstanceDatabaseGeoBackup `
-ResourceGroupName $SourceResourceGroupName `
-InstanceName $SourceInstanceName `
-Name $SourceDatabaseName

$backup | Restore-AzSqlInstanceDatabase -FromGeoBackup `
-TargetInstanceDatabaseName $TargetDatabaseName `
-TargetInstanceName $TargetInstanceName `
-TargetResourceGroupName $TargetResourceGroupName

```

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```azurepowershell-interactive
Remove-AzResourceGroup -ResourceGroupName $TargetResourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/New-AzResourceGroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Get-Azsqlınstancedatabasegeobackup](/powershell/module/az.sql/Get-AzSqlInstanceDatabaseGeoBackup) | SQL yönetilen örnek veritabanının coğrafi olarak yedekli bir yedeklemesini oluşturur. |
| [Restore-Azsqlınstancedatabase](/powershell/module/az.sql/Restore-AzSqlInstanceDatabase) | Coğrafi yedeklemeden SQL yönetilen örneği üzerinde bir veritabanı oluşturur. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure SQL veritabanı için ek PowerShell betiği örnekleri, [Azure SQL veritabanı PowerShell betikleri](../../database/powershell-script-content-guide.md)içinde bulunabilir.
