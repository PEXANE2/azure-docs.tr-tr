---
title: Araç sorgularında etiketleri kullanma
description: Çözümler geliştirmek için Azure SQL Veri Ambarı'ndaki araç sorgularına etiketler kullanma ipuçları.
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
ms.openlocfilehash: 828e4a406cd0fb12877af44263ab1f338c20850c
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351670"
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı'nda araç sorguları için etiketleri kullanma
Çözümler geliştirmek için Azure SQL Veri Ambarı'ndaki araç sorgularına etiketler kullanma ipuçları.


## <a name="what-are-labels"></a>Etiketler nedir?
SQL Veri Ambarı sorgu etiketleri adlı bir kavramı destekler. Herhangi bir derinliğe girmeden önce, bir örneğe bakalım:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Son satır, sorguya 'Sorgu Etiketim' dizesini etiketler. Etiket DMV'ler aracılığıyla sorgulanabilir olduğundan, bu etiket özellikle yararlıdır. Etiketler için sorgulama, sorun sorgularını bulmak ve ELT çalışmasında ilerlemeyi belirlemeye yardımcı olmak için bir mekanizma sağlar.

İyi bir adlandırma kongresi gerçekten yardımcı olur. Örneğin, etiketi PROJECT, PROCEDURE, STATEMENT veya COMMENT ile başlatmak, kaynak denetimindeki tüm kodlar arasında sorguyu benzersiz olarak tanımlamaya yardımcı olur.

Aşağıdaki sorgu, etikete göre arama yapmak için dinamik bir yönetim görünümü kullanır.

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


