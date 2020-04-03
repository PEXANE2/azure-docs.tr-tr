---
title: Araç sorgularında etiketleri kullanma
description: Çözümler geliştirmek için Synapse SQL havuzundaki araç sorgularına etiketler kullanma ipuçları.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: c1a4ffcab3d10f1dc91ce036e995ae0026a0d718
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619026"
---
# <a name="using-labels-to-instrument-queries-in-synapse-sql-pool"></a>Synapse SQL havuzunda enstrüman sorguları için etiketleri kullanma
Bu makalede, SQL havuzunda araç sorguları için etiketleri kullanarak çözümler geliştirmek için ipuçları verilmiştir.


## <a name="what-are-labels"></a>Etiketler nedir?
SQL havuzu sorgu etiketleri adlı bir kavramı destekler. Herhangi bir derinliğe girmeden önce, bir örneğe bakalım:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Son satır, sorguya 'Sorgu Etiketim' dizesini etiketler. Etiket DMV'ler aracılığıyla sorgulanabilir olduğundan bu etiket yararlıdır. 

Etiketler için sorgulama, sorun sorgularını bulmak ve ELT çalışmasında ilerlemeyi belirlemeye yardımcı olmak için bir mekanizma sağlar.

İyi bir adlandırma kongresi gerçekten yardımcı olur. Örneğin, etiketin PROJECT, PROCEDURE, STATEMENT veya COMMENT ile başlatılması, sorguyu kaynak denetimindeki tüm kodlar arasında benzersiz olarak tanımlar.

Aşağıdaki sorgu etikete göre arama yapmak için dinamik bir yönetim görünümü kullanır:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Sorgu yaparken sözcük etiketinin etrafına kare ayraçlar veya çift tırnak lar koymak önemlidir. Etiket ayrılmış bir sözcükve sınırlandırılamadığını zaman hataya neden olur. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.


