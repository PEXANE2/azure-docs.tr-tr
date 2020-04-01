---
title: PowerShell örnek içe bacpac dosyası Azure SQL veritabanı
description: BACPAC dosyasını bir SQL veritabanına içeri aktarmak için Azure PowerShell örnek betiği
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 05/24/2019
ms.openlocfilehash: 1cd7b94ac2717bbfa06d093d13fc201514cdc575
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73691694"
---
# <a name="use-powershell-to-import-a-bacpac-file-into-an-azure-sql-database"></a>PowerShell kullanarak BACPAC dosyasını bir Azure SQL veritabanına içeri aktarma

Bu PowerShell betiği örneği, BACPAC dosyasındaki bir veritabanını Azure SQL veritabanına içeri aktarır.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici AZ PowerShell 1.4.0 veya daha sonra gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=20-21 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Girilen IP adresi aralığından SQL Veritabanı sunucusundaki tüm tek veritabanlarına ve havuzlu veritabanına erişim sağlamak için bir SQL Database sunucusu güvenlik duvarı kuralı oluşturur. |
| [Yeni-AzSqlDatabaseImport](/powershell/module/az.sql/new-azsqldatabaseimport) | Bir BACPAC dosyasını içeri aktarır ve sunucuda yeni bir veritabanı oluşturur. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
