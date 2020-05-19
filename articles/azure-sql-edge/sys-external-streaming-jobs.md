---
title: sys. external_streaming_jobs (Transact-SQL)-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de sys. external_streaming_jobs kullanma hakkında bilgi edinin (Önizleme)
keywords: sys. external_streaming_jobs, SQL Edge
services: sql-database-edge
ms.service: sql-database-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2019
ms.openlocfilehash: 7d4f66375c5490906e773c6f105a029c3a88465c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597278"
---
# <a name="sysexternal_streaming_jobs-transact-sql"></a>sys. external_streaming_jobs (Transact-SQL)

Veritabanı kapsamında oluşturulan her bir dış akış işi için bir satır döndürür.

|Sütun adı|Veri türü|Açıklama|  
|-----------------|---------------|-----------------|
|**ada**|**döndürmeli**|Akışın adı. Veritabanı içinde benzersizdir.|
|**object_id**|**int**|Stream nesnesi için nesne kimlik numarası. Veritabanı içinde benzersizdir.|
|**principal_id**|**int**|Bu derlemeye sahip olan sorumlunun KIMLIĞI|
|**schema_id**|**int**| Nesneyi içeren şemanın KIMLIĞI.|
|**parent_object_id**|**numarasını**| Bu akış için üst nesne için nesne kimlik numarası. Geçerli uygulamada, bu değer her zaman null olur|
|**türüyle**|**karakter (2)**|Nesne türü. Stream nesneleri için, türü her zaman ' EJ ' olur|
|**type_desc**|**nvarchar (60)**| Nesne türünün açıklaması. Stream nesneleri için, türü her zaman ' EXTERNAL_STREAMING_JOB ' olur|
|**create_date**|**datetime**| Nesnenin oluşturulduğu tarih.|
|**modify_date**|**datetime**| Geçerli uygulamada, bu değer Stream nesnesi için create_date aynıdır |
|**is_ms_shipped**|**bit**| Bir iç bileşen tarafından oluşturulan nesne.|  
|**is_published**|**bit**| Nesne yayımlandı.|  
|**is_schema_published**|**bit**|Yalnızca nesnenin şeması yayımlanır.|
|**uses_ansi_nulls**|**bit**| Stream nesnesi, ÜZERINDE ANSI_NULLS veritabanı ayarla seçeneği ile oluşturuldu|
|**Ekstre**|**varchar(maks.)**| Akış Analizi, akış işi için sorgu metni. Daha fazla bilgi için bkz. [sp_create_streaming_job](overview.md) |
|**durumlarına**|**int**| Akış işinin geçerli durumu. Olası değerler şunlardır <br /><br /> **Oluşturulan** = 0. Akış işi oluşturuldu, ancak henüz başlatılmadı. <br /><br /> **Başlangıç** = 1. Akış işi başlangıç aşamasındadır. <br /><br /> **Başarısız** = 6. Akış işi başarısız oldu. Bu genellikle işlem sırasında önemli bir hatanın göstergesidir. <br /><br /> **Durduruldu** = 4. Akış işi durduruldu. <br /><br /> **Boşta** = 7. Akış işi çalışıyor, ancak işlenecek giriş yok. <br /><br /> **İşleme** = 8. Akış işi çalışıyor ve girişleri işliyor. Bu durum, akış işi için sağlıklı bir durumu gösterir. <br /><br /> **Düşürülmüş** = 9. Akış işi çalışıyor, ancak giriş işleme sırasında önemli olmayan giriş/çıkış serileştirme/seri hale getirme hataları vardı. Giriş işi çalışmaya devam eder, ancak hatalarla karşılaşan girdileri de bırakacak.|

## <a name="permissions"></a>İzinler

Katalog görünümlerindeki meta verilerin görünürlüğü, kullanıcının sahip olduğu veya kullanıcıya bir izin verildiği securables ile sınırlıdır. Daha fazla bilgi için bkz. [meta veri görünürlük yapılandırması](/sql/relational-databases/security/metadata-visibility-configuration/).

## <a name="see-also"></a>Ayrıca bkz.

- [T-SQL akış Kataloğu görünümleri](overview.md)
- [Katalog görünümleri (Transact-SQL)](/sql/relational-databases/system-catalog-views/catalog-views-transact-sql/)
- [Sistem görünümleri (Transact-SQL)](/sql/t-sql/language-reference/)

