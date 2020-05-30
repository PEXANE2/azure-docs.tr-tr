---
title: PowerShell örneği-izleme-ölçeklendirme-elastik havuz-Azure SQL veritabanı
description: Azure SQL veritabanında esnek havuzu izlemek ve ölçeklendirmek için örnek betik Azure PowerShell
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
ms.openlocfilehash: c0ce0a1019554592f769d88f4168da7dc6188586
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84196833"
---
# <a name="use-powershell-to-monitor-and-scale-an-elastic-pool-in-azure-sql-database"></a>Azure SQL veritabanında esnek havuzu izlemek ve ölçeklendirmek için PowerShell 'i kullanma
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Bu PowerShell betiği örneği, bir elastik havuzun performans ölçümlerini izler, elastik havuzu daha yüksek bir işlem boyutuna ölçeklendirir ve performans ölçümlerinden birinde bir uyarı kuralı oluşturur.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici az PowerShell 1.4.0 veya üstünü gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=17-18 "Monitor and scale an elastic pool in SQL Database")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
 [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Veritabanlarını veya elastik havuzları barındıran bir sunucu oluşturur. |
| [New-Azsqtalaçıkartma havuzu](/powershell/module/az.sql/new-azsqlelasticpool) | Elastik havuz oluşturur. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Sunucuda bir veritabanı oluşturur. |
| [Get-AzMetric](/powershell/module/az.monitor/get-azmetric) | Veritabanı için boyut kullanım bilgilerini gösterir.|
| [Add-AzMetricAlertRule](/powershell/module/az.monitor/add-azmetricalertrule) | Ölçüm tabanlı bir uyarı kuralı ekler veya güncelleştirir. |
| [Set-Azsqtalaçıkartma havuzu](/powershell/module/az.sql/set-azsqlelasticpool) | Elastik havuz özelliklerini güncelleştirir. |
| [Add-AzMetricAlertRule](/powershell/module/az.monitor/add-azmetricalertrule) | Gelecekte ölçümleri otomatik olarak izlemek için bir uyarı kuralı ayarlar. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek PowerShell betiği örnekleri, [Azure PowerShell betiklerine](../powershell-script-content-guide.md)bulunabilir.
