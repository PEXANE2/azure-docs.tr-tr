---
title: Synapse SQL'de sorgu etiketlerini kullanma
description: Bu makalede Synapse SQL sorgu etiketleri kullanmak için gerekli ipuçları dahildir.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 47b476cbc6997ca5ec63968bdc269e2273662100
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430038"
---
# <a name="use-query-labels-in-synapse-sql"></a>Synapse SQL'de sorgu etiketlerini kullanma
Bu makalede Synapse SQL sorgu etiketleri kullanmak için gerekli ipuçları dahildir.

> [!NOTE]
> İsteğe bağlı SQL (önizleme) etiketleme sorgularını desteklemez.

## <a name="what-are-query-labels"></a>Sorgu etiketleri nedir
SQL havuzu sorgu etiketleri adlı bir kavramı destekler. Herhangi bir derinliğe girmeden önce, bir örneğe bakalım:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Son satır, sorguya 'Sorgu Etiketim' dizesini etiketler. Etiket DMV'ler aracılığıyla sorgulanabilir olduğundan, bu etiket özellikle yararlıdır. Etiketler için sorgulama, sorun sorgularını bulmak için bir mekanizma sağlar ve ELT çalışmasında ilerlemeyi belirlemeye yardımcı olur.

İyi adlandırma kuralı en yararlıdır. Örneğin, etiketin PROJECT, PROCEDURE, STATEMENT veya COMMENT ile başlatılması, sorguyu kaynak denetimindeki tüm kodlar arasında benzersiz olarak tanımlar.

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
Daha fazla geliştirme ipucu için [geliştirme genel bakış](develop-overview.md)ına bakın.


