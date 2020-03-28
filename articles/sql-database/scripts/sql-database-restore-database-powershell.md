---
title: PowerShell örnek-geri yükleme-yedekleme-Azure SQL veritabanı
description: Azure SQL tek veritabanını otomatik yedeklemelerden daha önceki bir noktaya geri yüklemek için Azure PowerShell örnek komut dosyası
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 03/27/2019
ms.openlocfilehash: da4236e138bd75237ca10b85dc1586fecd1cece4
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73691562"
---
# <a name="use-powershell-to-restore-an-azure-sql-single-database-to-an-earlier-point-in-time"></a>Azure SQL tek veritabanını daha önceki bir noktaya geri yüklemek için PowerShell'i kullanın

Bu PowerShell komut dosyası örneği, bir Azure SQL veritabanını belirli bir noktaya geri yükler.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici AZ PowerShell 1.4.0 veya daha sonra gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/restore-database/restore-database.ps1?highlight=17-18 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek bir veritabanı veya elastik havuz barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase) | SQL Veritabanı sunucusunda bağımsız veya havuzlu bir veritabanı olarak bir veritabanı oluşturur. |
[Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) | Bağımsız veya havuzlu bir veritabanının coğrafi yedekli yedeklemesini alır. |
| [Geri Yükleme-AzSqlVeritabanı](/powershell/module/az.sql/restore-azsqldatabase) | SQL bağımsız veya havuzlu veritabanını geri yükler. |
|[Remove-AzSqlDatabase](/powershell/module/az.sql/remove-azsqldatabase) | Azure SQL bağımsız veya havuzlu veritabanını kaldırır. |
| [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Geri yükleyebileceğiniz silinmiş bağımsız veya havuzlu bir veritabanı alır. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
