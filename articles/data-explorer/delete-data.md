---
title: Azure Veri Gezgini'nden veri silme
description: Bu makalede, temizleme, düşme kapsamları ve bekletme tabanlı silmeler de dahil olmak üzere Azure Veri Gezgini'ndeki silme senaryoları açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: avneraa
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: dd0f8740d148a7817bcfe2fbad591ceeb1610d0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501419"
---
# <a name="delete-data-from-azure-data-explorer"></a>Azure Veri Gezgini'nden veri silme

Azure Veri Gezgini, bu makalede açıklanan çeşitli silme senaryolarını destekler. 

## <a name="delete-data-using-the-retention-policy"></a>Bekletme ilkesini kullanarak verileri silme

Azure Veri Gezgini, [bekletme ilkesine](/azure/kusto/management/retentionpolicy)göre verileri otomatik olarak siler. Bu yöntem, verileri silmenin en verimli ve sorunsuz yoludur. Bekletme ilkesini veritabanıveya tablo düzeyinde ayarlayın.

90 günlük bekletme için ayarlanmış bir veritabanı veya tablo düşünün. Yalnızca 60 günlük veri gerekiyorsa, eski verileri aşağıdaki gibi silin:

```kusto
.alter-merge database <DatabaseName> policy retention softdelete = 60d

.alter-merge table <TableName> policy retention softdelete = 60d
```

## <a name="delete-data-by-dropping-extents"></a>Kapsamları bırakarak verileri silme

[Kapsam (veri shard)](/azure/kusto/management/extents-overview) verilerin depolandığı iç yapıdır. Her kapsamda milyonlarca kayıt tutabilir. Extents tek tek veya [damla extent(ler) komutları](/azure/kusto/management/extents-commands#drop-extents)kullanılarak bir grup olarak silinebilir. 

### <a name="examples"></a>Örnekler

Bir tablodaki tüm satırları veya belirli bir boyutu silebilirsiniz.

* Tablodaki tüm satırları silin:

    ```kusto
    .drop extents from TestTable
    ```

* Belirli bir ölçüde silin:

    ```kusto
    .drop extent e9fac0d2-b6d5-4ce3-bdb4-dea052d13b42
    ```

## <a name="delete-individual-rows-using-purge"></a>Temizleme kullanarak tek tek satırları silme

[Veri temizleme,](/azure/kusto/concepts/data-purge) bireyler satırlarını sileriçin kullanılabilir. Silme hemen değildir ve önemli sistem kaynakları gerektirir. Bu nedenle, yalnızca uyumluluk senaryoları için önerilir.  

