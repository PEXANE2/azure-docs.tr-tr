---
title: Sorguları işaretlemek için etiketleri kullanma
description: Çözümleri geliştirmeye yönelik SYNAPSE SQL havuzundaki sorguları işaretlemek için etiketleri kullanma ipuçları.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 91b6dac5fba4bb8dfd8cf4a3bb4e5952f8388bb5
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89459156"
---
# <a name="using-labels-to-instrument-queries-in-synapse-sql-pool"></a>SYNAPSE SQL havuzundaki sorguları işaretlemek için etiketleri kullanma

Bu makaleye dahil olmak üzere, SQL havuzundaki sorguları işaretlemek için Etiketler kullanarak çözüm geliştirmeye yönelik ipuçları yer almaktadır.

Azure SYNAPSE Analytics 'te çözüm geliştirmeye yönelik sorguları işaretlemek için Etiketler kullanma ipuçları.

## <a name="what-are-labels"></a>Etiketler nedir?

SQL havuzu sorgu etiketleri adlı bir kavramı destekler. Herhangi bir derinliğe geçmeden önce bir örneğe göz atalım:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Son çizgi ' My Query Tag ' dizesini sorguya etiketleyin. Etiket, DMVs aracılığıyla sorgu özellikli olduğundan bu etiket yararlıdır.

Etiketleri sorgulamak, sorun sorgularını bulmaya ve bir ELT çalıştırması aracılığıyla ilerlemeyi belirlemesine yardımcı olmaya yönelik bir mekanizma sağlar.

İyi bir adlandırma kuralı aslında yardımcı olur. Örneğin, etiketi PROJECT, PROCEDURE, DEYIMLE ve açıklama ile başlatmak, sorguyu kaynak denetimindeki tüm kodlar arasında benzersiz şekilde tanımlar.

Aşağıdaki sorgu, etikete göre aramak için dinamik bir yönetim görünümü kullanır:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Sorgulama sırasında, sözcük etiketinin çevresine köşeli ayraç veya çift tırnak koymak gereklidir. Etiket, ayrılmış bir sözcüktür ve sınırlandırılmamış bir hata oluşmasına neden olur.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için bkz. [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).
