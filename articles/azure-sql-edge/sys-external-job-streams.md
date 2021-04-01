---
title: sys.external_job_streams (Transact-SQL)-Azure SQL Edge
description: Azure SQL Edge 'de sys.external_job_streams kullanma hakkında bilgi edinin
keywords: sys.external_job_streams, SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2019
ms.openlocfilehash: 35010d3aba7f6d5ee3185291c917ff7726ba8bd7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "90900363"
---
# <a name="sysexternal_job_streams-transact-sql"></a>sys.external_job_streams (Transact-SQL)

Bir dış akış işiyle eşlenen giriş veya çıkış dış akış nesnesi için bir satır döndürür.

|Sütun adı|Veri türü|Açıklama|  
|-----------------|---------------|-----------------|
|**job_id**|**int**| Akış işi nesnesi için nesne kimlik numarası. Bu sütun sys.external_streaming_jobs object_id sütunuyla eşlenir.|
|**stream_id**|**int**| Stream nesnesi için nesne kimlik numarası. Bu sütun sys.external_streams object_id sütunuyla eşlenir. |
|**is_input**|**bit**| 1 Stream nesnesi akış işi için bir giriş kullandıysanız, aksi takdirde 0.|
|**is_output**|**bit**| 1 Stream nesnesi akış işi için bir çıktı kullandıysanız, aksi takdirde 0.|

## <a name="example"></a>Örnek

Bu Katalog görünümü `sys.external_streams` ve `sys.external_streaming_jobs` Katalog görünümleriyle birlikte kullanılır. Örnek sorgu aşağıda gösterilmektedir

```sql
Select
    sj.Name as Job_Name,
    sj.Create_date as Job_Create_date,
    sj.modify_date as Job_Modify_date,
    sj.statement as Stream_Job_Query,
    Input_Stream_Name =
        Case js.is_input
            when 1 then s.Name
            else null
        END,
    output_Stream_Name =
        case js.is_output
            when 1 then s.Name
            else null
        END,
    s.location as Stream_Location
from sys.external_job_streams js
inner join sys.external_streams s on s.object_id = js.stream_id
inner join sys.external_streaming_jobs sj on sj.object_id = js.job_id
```

## <a name="permissions"></a>İzinler

Katalog görünümlerindeki meta verilerin görünürlüğü, kullanıcının sahip olduğu veya kullanıcıya bir izin verildiği securables ile sınırlıdır. Daha fazla bilgi için bkz. [meta veri görünürlük yapılandırması](/sql/relational-databases/security/metadata-visibility-configuration/).

## <a name="see-also"></a>Ayrıca bkz.

- [Katalog görünümleri (Transact-SQL)](/sql/relational-databases/system-catalog-views/catalog-views-transact-sql/)
- [Sistem görünümleri (Transact-SQL)](/sql/t-sql/language-reference/)
