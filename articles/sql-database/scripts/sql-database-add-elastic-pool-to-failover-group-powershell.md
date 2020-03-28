---
title: PowerShell örneği- Failover grubu - Azure SQL Veritabanı elastik havuzu
description: Azure SQL Veritabanı elastik havuzu oluşturmak, bir başarısız lık grubuna eklemek ve başarısızlığı test etmek için Azure PowerShell örnek komut dosyası.
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
ms.openlocfilehash: a8c9b7dbddba59408808e1c33de045ad0b7ffd7a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73691797"
---
# <a name="use-powershell-to-add-an-azure-sql-database-elastic-pool-to-a-failover-group"></a>Bir başarısız gruba Azure SQL Veritabanı elastik havuzu eklemek için PowerShell'i kullanın 

Bu PowerShell komut dosyası örneği tek bir veritabanı oluşturur, elastik bir havuza ekler, bir başarısız lık grubu oluşturur ve başarısız lığı sınar. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici AZ PowerShell 1.4.0 veya daha sonra gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-scripts"></a>Örnek komut dosyaları

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/failover-groups/add-elastic-pool-to-failover-group-az-ps.ps1 "Add elastic pool to a failover group")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Tek veritabanları ve elastik havuzlar barındıran bir SQL Veritabanı sunucusu oluşturur. |
| [Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Mantıksal bir sunucu için bir güvenlik duvarı kuralı oluşturur. | 
| [Yeni-AzSqlVeritabanı](/powershell/module/az.sql/new-azsqldatabase) | Yeni bir Azure SQL Veritabanı tek veritabanı oluşturur. | 
| [Yeni-Azsqlelasticpool](/powershell/module/az.sql/new-azsqlelasticpool) | Azure SQL Veritabanı için esnek bir veritabanı havuzu oluşturur.| 
| [Set-AzSqlVeritabanı](/powershell/module/az.sql/set-azsqldatabase) | Veritabanı nın özelliklerini ayarlar veya varolan bir veritabanını elastik bir havuza taşır. | 
| [Yeni-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | Yeni bir başarısız grup oluşturur. |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | Bir veya daha fazla SQL veritabanı alır. |
| [Ekle-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | Bir başarısız gruba bir veya daha fazla Azure SQL Veritabanı ekler. |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Azure SQL Veritabanı başarısız grupları alır veya listeler. |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| Azure SQL Veritabanı failover grubunun başarısız olduğunu yürüter. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu kaldırır | 

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
