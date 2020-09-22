---
title: sys. sp_cleanup_data_retention (Transact-SQL)-Azure SQL Edge
description: Azure SQL Edge 'de sys. sp_cleanup_data_retention (Transact-SQL) kullanma hakkında bilgi edinin
keywords: sys. sp_cleanup_data_retention (Transact-SQL), SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/22/2020
ms.openlocfilehash: 9c0a6700a476d4f7f875af5373e3c99bc4e25af2
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941367"
---
# <a name="syssp_cleanup_data_retention-transact-sql"></a>sys.sp_cleanup_data_retention (Transact-SQL)

**Uygulama hedefi:** Azure SQL Edge

Veri saklama ilkeleri etkin olan tablolardan kullanılmayan kayıtları temizlemeyi gerçekleştirir. Daha fazla bilgi için bkz. [veri saklama](data-retention-overview.md). 

## <a name="syntax"></a>Söz dizimi 

```sql
sys.sp_cleanup_data_retention   
    { [@schema_name = ] 'schema_name' },  
    { [@table_name = ] 'table_name' },   
    [ [@rowcount =] rowcount OUTPUT ]    

```

## <a name="arguments"></a>Bağımsız değişkenler  
`[ @schema_name = ] schema_name`    
 , Temizleme işleminin gerçekleştirilmesi gereken tablo için sahip olan şemanın adıdır. *schema_name* , **döndürmeli**türünde gerekli bir parametredir.
  
`[ @table_name = ] 'table_name'`    
 , Temizleme işleminin gerçekleştirilmesi gereken tablonun adıdır. *table_name* , **döndürmeli**türünde gerekli bir parametredir.

## <a name="output-parameter"></a>Çıkış parametresi  

`[ @rowcount = ] rowcount OUTPUT`   
 rowcount, tablodaki kayıt Temizleme sayısını temsil eden isteğe bağlı bir çıkış parametresidir. *rowcount* int 'tir.
  
## <a name="permissions"></a>İzinler    
 Db_owner izinleri gerektirir.

## <a name="next-steps"></a>Sonraki adımlar
- [Veri saklama ve otomatik veri temizleme](data-retention-overview.md)
- [Geçmiş verilerini bekletme ilkesiyle yönetme](data-retention-cleanup.md) 
- [Veri bekletmeyi etkinleştirme ve devre dışı bırakma](data-retention-enable-disable.md)
