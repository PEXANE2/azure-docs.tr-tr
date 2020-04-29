---
title: SYNAPSE SQL 'de sorgu etiketlerini kullanma
description: Bu makaleye dahil olmak üzere, SYNAPSE SQL 'de sorgu etiketlerini kullanmaya yönelik temel ipuçları yer almaktadır.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81430038"
---
# <a name="use-query-labels-in-synapse-sql"></a>SYNAPSE SQL 'de sorgu etiketlerini kullanma
Bu makaleye dahil olmak üzere, SYNAPSE SQL 'de sorgu etiketlerini kullanmaya yönelik temel ipuçları yer almaktadır.

> [!NOTE]
> İsteğe bağlı SQL (Önizleme) sorguları etiketlemeyi desteklemez.

## <a name="what-are-query-labels"></a>Sorgu etiketleri nedir?
SQL havuzu sorgu etiketleri adlı bir kavramı destekler. Herhangi bir derinliğe geçmeden önce bir örneğe göz atalım:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Son çizgi ' My Query Tag ' dizesini sorguya etiketleyin. Etiket, DMVs aracılığıyla sorgu özellikli olduğundan bu etiket özellikle yararlıdır. Etiketleri sorgulamak, sorun sorgularını bulmaya yönelik bir mekanizma sağlar ve ilerlemeyi bir ELT çalıştırması aracılığıyla belirlemesine yardımcı olur.

İyi adlandırma kuralı en yararlı seçenektir. Örneğin, etiketi PROJECT, PROCEDURE, DEYIMLE ve açıklama ile başlatmak, sorguyu kaynak denetimindeki tüm kodlar arasında benzersiz şekilde tanımlar.

Aşağıdaki sorgu, etikete göre aramak için dinamik bir yönetim görünümü kullanır:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Sorgulama sırasında, sözcük etiketinin çevresine köşeli ayraç veya çift tırnak koymak gereklidir. Etiket, ayrılmış bir sözcüktür ve sınırlandırılmamış bir hata oluşmasına neden olur. 
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipucu için bkz. [geliştirmeye genel bakış](develop-overview.md).


