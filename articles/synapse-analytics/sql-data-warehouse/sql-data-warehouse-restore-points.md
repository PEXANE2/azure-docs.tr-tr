---
title: Kullanıcı tanımlı geri yükleme noktaları
description: SQL havuzu için geri yükleme noktası nasıl oluşturulur.
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
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745064"
---
# <a name="user-defined-restore-points"></a>Kullanıcı tanımlı geri yükleme noktaları

Bu makalede, PowerShell ve Azure portalını kullanarak Azure Synapse Analytics'teki bir SQL havuzu için kullanıcı tanımlı yeni bir geri yükleme noktası oluşturmayı öğreneceksiniz.

## <a name="create-user-defined-restore-points-through-powershell"></a>PowerShell ile kullanıcı tanımlı geri yükleme noktaları oluşturma

Kullanıcı tanımlı bir geri yükleme noktası oluşturmak için [Yeni-AzSqlDatabaseRestorePoint](/powershell/module/az.sql/new-azsqldatabaserestorepoint?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) PowerShell cmdlet'ini kullanın.

1. Başlamadan önce [Azure PowerShell'i yüklediğinizden](/powershell/azure/overview?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)emin olun.
2. PowerShell’i açın.
3. Azure hesabınıza bağlanın ve hesabınızla ilişkili tüm abonelikleri listele.
4. Geri yüklenecek veritabanını içeren aboneliği seçin.
5. Veri ambarınızın hemen bir kopyası için bir geri yükleme noktası oluşturun.

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

6. Varolan tüm geri yükleme noktalarının listesine bakın.

```Powershell
# List all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName
```

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>Azure portalı üzerinden kullanıcı tanımlı geri yükleme noktaları oluşturma

Azure portalı aracılığıyla kullanıcı tanımlı geri yükleme noktaları da oluşturulabilir.

1. [Azure portal](https://portal.azure.com/) hesabınızda oturum açın.

2. Geri yükleme noktası oluşturmak istediğiniz SQL havuzuna gidin.

3. Sol bölmeden **Genel Bakış'ı** seçin, **+ Yeni Geri Yükleme Noktası'nı**seçin. Yeni Geri Yükleme Noktası düğmesi etkin değilse, SQL havuzunun duraklatmadığından emin olun.

    ![Yeni Geri Yükleme Noktası](./media/sql-data-warehouse-restore-points/creating-restore-point-01.png)

4. Kullanıcı tanımlı geri yükleme noktanız için bir ad belirtin ve **Uygula'yı**tıklatın. Kullanıcı tanımlı geri yükleme noktalarının varsayılan bekletme süresi yedi gündür.

    ![Geri Yükleme Noktasının Adı](./media/sql-data-warehouse-restore-points/creating-restore-point-11.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Varolan bir SQL havuzunun geri yüklemesi](sql-data-warehouse-restore-active-paused-dw.md)
- [Silinen bir SQL havuzunun geri yüklemesi](sql-data-warehouse-restore-deleted-dw.md)
- [Coğrafi yedekleme SQL havuzundan geri yükleme](sql-data-warehouse-restore-from-geo-backup.md)

