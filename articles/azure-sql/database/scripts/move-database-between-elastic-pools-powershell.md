---
title: 'PowerShell: bir veritabanını elastik havuzlar arasında taşıma'
description: SQL veritabanındaki bir veritabanını iki elastik havuz arasında taşımak için Azure PowerShell örnek betiği kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1
ms.devlang: PowerShell
ms.topic: sample
author: stevestein
ms.reviewer: ''
ms.author: sstein
ms.date: 03/12/2019
ms.openlocfilehash: de26542c56eeddefc560c17a9d7b03f18f7c15a3
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84196788"
---
# <a name="use-powershell-to-create-elastic-pools-and-move-a-database-between-them"></a>PowerShell kullanarak elastik havuzlar oluşturun ve bir veritabanını aralarında taşıyın

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Bu PowerShell betiği örneği, iki elastik havuz oluşturur, SQL veritabanında havuza alınmış bir veritabanını bir SQL elastik havuzundan başka bir SQL elastik havuzuna taşımaktadır ve sonra havuza alınmış veritabanını SQL elastik havuzundan Azure SQL veritabanı 'nda tek bir veritabanı olacak şekilde taşımaktadır.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici az PowerShell 1.4.0 veya üstünü gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="sample-script"></a>Örnek betik

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/move-database-between-pools-and-standalone/move-database-between-pools-and-standalone.ps1?highlight=18-19 "Move a database between pools")]

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
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Veritabanları ve elastik havuzlar barındıran bir sunucu oluşturur. |
| [New-Azsqtalaçıkartma havuzu](/powershell/module/az.sql/new-azsqlelasticpool) | Elastik havuz oluşturur. |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | Sunucuda bir veritabanı oluşturur. |
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | Veritabanı özelliklerini güncelleştirir veya bir veritabanını elastik havuzun içine veya dışına ya da elastik havuzlar arasında taşır. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../powershell-script-content-guide.md) içinde bulunabilir.
