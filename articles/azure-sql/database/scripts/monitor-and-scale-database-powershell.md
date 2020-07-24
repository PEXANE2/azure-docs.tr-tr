---
title: Azure SQL veritabanı 'nda tek bir veritabanını izlemek ve ölçeklendirmek için PowerShell 'i kullanma
description: Azure SQL veritabanı 'nda tek bir veritabanını izlemek ve ölçeklendirmek için Azure PowerShell örnek betiği kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: 3c4695bb9dfeb9f0122d39dd335c9b733fe039b2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87085070"
---
# <a name="use-powershell-to-monitor-and-scale-a-single-database-in-azure-sql-database"></a>Azure SQL veritabanı 'nda tek bir veritabanını izlemek ve ölçeklendirmek için PowerShell 'i kullanma

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Bu PowerShell betiği örneği, tek bir veritabanının performans ölçümlerini izler, daha yüksek bir işlem boyutuna ölçeklendirir ve performans ölçümlerinden birinde bir uyarı kuralı oluşturur.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici az PowerShell 1.4.0 veya üstünü gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1?highlight=15-16 "Monitor and scale single database")]

> [!NOTE]
> Ölçümlerin tam listesi için bkz. [ölçüm destekleniyor](../../../azure-monitor/platform/metrics-supported.md#microsoftsqlserversdatabases).
> [!TIP]
> Veritabanı işlemlerinin durumunu almak için [Get-azsqldatabaseactivity](/powershell/module/az.sql/get-azsqldatabaseactivity) komutunu kullanın ve bir veritabanı güncelleştirme işlemini iptal etmek için [stop-azsqldatabaseactivity](/powershell/module/az.sql/stop-azsqldatabaseactivity) kullanın.

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
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek bir veritabanı veya elastik havuz barındıran bir sunucu oluşturur. |
| [Get-AzMetric](/powershell/module/az.monitor/get-azmetric) | Veritabanı için boyut kullanım bilgilerini gösterir.|
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | Veritabanı özelliklerini güncelleştirir veya veritabanlarını elastik havuzların içine veya dışına taşıdıkça. |
| [Add-AzMetricAlertRule](/powershell/module/az.monitor/add-azmetricalertrule) | Gelecekte ölçümleri otomatik olarak izlemek için bir uyarı kuralı ayarlar. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek PowerShell betiği örnekleri, [Azure PowerShell betiklerine](../powershell-script-content-guide.md)bulunabilir.
