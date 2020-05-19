---
title: sys. external_streams (Transact-SQL)-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de sys. external_streams kullanma hakkında bilgi edinin (Önizleme)
keywords: sys. external_streams, SQL Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2019
ms.openlocfilehash: d47139b0b2a20ecfcf92bfc55a8b47c8e3cf7dab
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597285"
---
# <a name="sysexternal_streams-transact-sql"></a>sys. external_streams (Transact-SQL)

Veritabanı kapsamında oluşturulan her dış akış nesnesi için bir satır döndürür.

|Sütun adı|Veri türü|Açıklama|  
|-----------------|---------------|-----------------|
|**ada**|**döndürmeli**|Akışın adı. Veritabanı içinde benzersizdir.|
|**object_id**|**int**|Stream nesnesi için nesne kimlik numarası. Veritabanı içinde benzersizdir.|
|**principal_id**|**int**|Bu derlemeye sahip olan sorumlunun KIMLIĞI|
|**schema_id**|**int**| Nesneyi içeren şemanın KIMLIĞI.|
|**parent_object_id**|**numarasını**| Bu akış için üst nesne için nesne kimlik numarası. Geçerli uygulamada, bu değer her zaman null olur|
|**türüyle**|**karakter (2)**|Nesne türü. Stream nesneleri için, tür her zaman ' ES ' olur|
|**type_desc**|**nvarchar (60)**| Nesne türünün açıklaması. Stream nesneleri için, türü her zaman ' EXTERNAL_STREAM ' olur|
|**create_date**|**hem**| Nesnenin oluşturulduğu tarih.|
|**modify_date**|**hem**| Nesnenin ALTER ifadesi kullanılarak son değiştirildiği tarih.|
|**is_ms_shipped**|**bit**| Bir iç bileşen tarafından oluşturulan nesne.|  
|**is_published**|**bit**|Nesne yayımlandı.|  
|**is_schema_published**|**bit**|Yalnızca nesnenin şeması yayımlanır.|
|**max_column_id_used**|**bit**| Bu sütun, iç amaçlar için kullanılır ve gelecekte kaldırılacaktır|  
|**uses_ansi_nulls**|**bit**| Stream nesnesi, ÜZERINDE ANSI_NULLS veritabanı ayarla seçeneği ile oluşturuldu|
|**data_source_id**|**int**| Stream nesnesi tarafından temsil edilen dış veri kaynağının nesne KIMLIĞI |  
|**file_format_id**|**int**| Stream nesnesi tarafından kullanılan dış dosya biçiminin nesne KIMLIĞI. Dış dosya biçimi, bir dış akış tarafından başvurulan verilerin gerçek yerleşimini belirtmek için gereklidir| 
|**konumuna**|**varchar(maks.)**| Dış akış nesnesi için hedef. Daha fazla bilgi için [dış akış oluşturma](overview.md) konusuna bakın |
|**input_option**|**varchar(maks.)**| Dış akışın oluşturulması sırasında kullanılan giriş seçenekleri. Daha fazla bilgi için [dış akış oluşturma](overview.md) konusuna bakın |
|**output_option**|**varchar(maks.)**| Dış akışın oluşturulması sırasında kullanılan çıkış seçenekleri. Daha fazla bilgi için [dış akış oluşturma](overview.md) konusuna bakın | 

## <a name="permissions"></a>İzinler

Katalog görünümlerindeki meta verilerin görünürlüğü, kullanıcının sahip olduğu veya kullanıcıya bir izin verildiği securables ile sınırlıdır. Daha fazla bilgi için bkz. [meta veri görünürlük yapılandırması](/sql/relational-databases/security/metadata-visibility-configuration/).

## <a name="see-also"></a>Ayrıca bkz.

- [Katalog görünümleri (Transact-SQL)](/sql/relational-databases/system-catalog-views/catalog-views-transact-sql/)
- [Sistem görünümleri (Transact-SQL)](/sql/t-sql/language-reference/)
