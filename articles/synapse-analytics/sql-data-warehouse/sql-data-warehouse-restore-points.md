---
title: Kullanıcı tanımlı geri yükleme noktaları
description: SQL havuzu için geri yükleme noktası oluşturma.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 07/03/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 5c9b7eb1b03b6b6e3721c13f9ebf7da25dd2e376
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80745064"
---
# <a name="user-defined-restore-points"></a>Kullanıcı tanımlı geri yükleme noktaları

Bu makalede, PowerShell ve Azure portal kullanarak Azure SYNAPSE Analytics 'te bir SQL havuzu için yeni bir Kullanıcı tanımlı geri yükleme noktası oluşturmayı öğreneceksiniz.

## <a name="create-user-defined-restore-points-through-powershell"></a>PowerShell aracılığıyla Kullanıcı tanımlı geri yükleme noktaları oluşturma

Kullanıcı tanımlı geri yükleme noktası oluşturmak için [New-AzSqlDatabaseRestorePoint](/powershell/module/az.sql/new-azsqldatabaserestorepoint?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) PowerShell cmdlet 'ini kullanın.

1. Başlamadan önce [Azure PowerShell](/powershell/azure/overview?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)' yi yüklediğinizden emin olun.
2. PowerShell’i açın.
3. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listeleyin.
4. Geri yüklenecek veritabanını içeren aboneliği seçin.
5. Veri ambarınızın anlık bir kopyası için geri yükleme noktası oluşturun.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$Label = "<YourRestorePointLabel>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Create a restore point of the original database
New-AzSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -RestorePointLabel $Label

```

6. Tüm mevcut geri yükleme noktalarının listesine bakın.

```Powershell
# List all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName
```

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>Azure portal aracılığıyla Kullanıcı tanımlı geri yükleme noktaları oluşturma

Kullanıcı tanımlı geri yükleme noktaları, Azure portal aracılığıyla da oluşturulabilir.

1. [Azure Portal](https://portal.azure.com/) hesabınızda oturum açın.

2. İçin geri yükleme noktası oluşturmak istediğiniz SQL havuzuna gidin.

3. Sol bölmeden **genel bakış** ' ı seçin, **+ Yeni geri yükleme noktası**' nı seçin. Yeni geri yükleme noktası düğmesi etkinleştirilmemişse, SQL havuzunun duraklatılmadığından emin olun.

    ![Yeni geri yükleme noktası](./media/sql-data-warehouse-restore-points/creating-restore-point-01.png)

4. Kullanıcı tanımlı geri yükleme noktanız için bir ad belirtin ve **Uygula**' ya tıklayın. Kullanıcı tanımlı geri yükleme noktalarının varsayılan saklama süresi yedi gündür.

    ![Geri yükleme noktasının adı](./media/sql-data-warehouse-restore-points/creating-restore-point-11.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Mevcut bir SQL havuzunu geri yükleme](sql-data-warehouse-restore-active-paused-dw.md)
- [Silinen bir SQL havuzunu geri yükleme](sql-data-warehouse-restore-deleted-dw.md)
- [Coğrafi yedekleme SQL havuzundan geri yükleme](sql-data-warehouse-restore-from-geo-backup.md)

