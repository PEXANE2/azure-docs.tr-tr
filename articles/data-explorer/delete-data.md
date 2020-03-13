---
title: Azure Veri Gezgini verileri silme
description: Bu makalede Temizleme, kapsamları bırakma ve bekletme tabanlı silme dahil olmak üzere Azure Veri Gezgini 'da silme senaryoları açıklanmaktadır.
author: orspod
ms.author: orspodek
ms.reviewer: avneraa
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: 681cfd71d2666630b192935d66ba32eaf16c92de
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79204622"
---
# <a name="delete-data-from-azure-data-explorer"></a>Azure Veri Gezgini verileri silme

Azure Veri Gezgini, bu makalede açıklanan çeşitli silme senaryolarını destekler. 

## <a name="delete-data-using-the-retention-policy"></a>Bekletme ilkesini kullanarak verileri silme

Azure Veri Gezgini, [bekletme ilkesine](/azure/kusto/management/retentionpolicy)göre verileri otomatik olarak siler. Bu yöntem, verileri silmenin en etkili ve sorunsuz yoludur. Veritabanı veya tablo düzeyinde bekletme ilkesini ayarlayın.

90 gün bekletme için ayarlanan bir veritabanını veya tabloyu düşünün. Yalnızca 60 gün veri gerekiyorsa, eski verileri aşağıdaki gibi silin:

```kusto
.alter-merge database <DatabaseName> policy retention softdelete = 60d

.alter-merge table <TableName> policy retention softdelete = 60d
```

## <a name="delete-data-by-dropping-extents"></a>Kapsamları bırakarak verileri silme

[Kapsam (veri parça)](/azure/kusto/management/extents-overview) , verilerin depolandığı iç yapıdır. Her uzantı milyonlarca kayıt tutabilir. Kapsamlar tek tek veya [bırakma uzantıları](/azure/kusto/management/extents-commands#drop-extents)kullanılarak grup olarak silinebilir. 

### <a name="examples"></a>Örnekler

Bir tablodaki tüm satırları veya yalnızca belirli bir kapsamı silebilirsiniz.

* Bir tablodaki tüm satırları sil:

    ```kusto
    .drop extents from TestTable
    ```

* Belirli bir kapsamı Sil:

    ```kusto
    .drop extent e9fac0d2-b6d5-4ce3-bdb4-dea052d13b42
    ```

## <a name="delete-individual-rows-using-purge"></a>Temizleme kullanarak ayrı satırları silme

[Veri temizleme](/azure/kusto/management/data-purge) , bireyler satırlarını silmek için kullanılabilir. Silme işlemi anında değildir ve önemli sistem kaynakları gerektirir. Bu nedenle, yalnızca Uyumluluk senaryoları önerilir.  

