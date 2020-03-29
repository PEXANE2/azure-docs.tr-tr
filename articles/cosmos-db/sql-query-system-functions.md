---
title: Azure Cosmos DB sorgu dilinde sistem işlevleri
description: Azure Cosmos DB'de yerleşik ve kullanıcı tanımlı SQL sistem işlevleri hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 6f41adbb726313ef095084d079dc7852736e0c06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74870539"
---
# <a name="system-functions-azure-cosmos-db"></a>Sistem fonksiyonları (Azure Cosmos DB)

 Cosmos DB birçok yerleşik SQL işlevi sağlar. Yerleşik işlevler kategorileri aşağıda listelenmiştir.  
  
|Fonksiyon grubu|Açıklama|İşlemler|  
|--------------|-----------------|-----------------| 
|[Dizi işlevleri](sql-query-array-functions.md)|Dizi işlevleri bir dizi giriş değeri üzerinde bir işlem gerçekleştirir ve sayısal, Boolean veya dizi değeri döndürün. | [ARRAY_CONCAT](sql-query-array-concat.md), [ARRAY_CONTAINS](sql-query-array-contains.md), [ARRAY_LENGTH](sql-query-array-length.md), [ARRAY_SLICE](sql-query-array-slice.md) |
|[Tarih ve Saat fonksiyonları](sql-query-date-time-functions.md)|Tarih ve saat fonksiyonları geçerli UTC tarih ve saatini iki şekilde elde etmenizi sağlar; değeri milisaniye cinsinden veya ISO 8601 biçimine uygun bir dize olarak Unix çağı olan sayısal bir zaman damgası. | [GetCurrentDateTime](sql-query-getcurrentdatetime.md), [GetCurrentTimestamp](sql-query-getcurrenttimestamp.md) |
|[Matematik işlevleri](sql-query-mathematical-functions.md)|Matematiksel işlevlerin her biri, genellikle bağımsız değişken olarak sağlanan giriş değerlerini temel alan bir hesaplama yapar ve sayısal bir değer döndürür. | [ABS](sql-query-abs.md), [ACOS](sql-query-acos.md), [ASIN](sql-query-asin.md), [ATAN](sql-query-atan.md), [ATN2](sql-query-atn2.md), [TAVAN](sql-query-ceiling.md), [COS](sql-query-cos.md), [KARYOLA](sql-query-cot.md), [DERECE](sql-query-degrees.md), [EXP](sql-query-exp.md), [TABAN](sql-query-floor.md), LOG , [LOG10](sql-query-log10.md), [PI](sql-query-pi.md), [GÜÇ](sql-query-power.md), [RADYANS](sql-query-radians.md), [RAND](sql-query-rand.md), [YUVARLAK](sql-query-round.md), [İşaret](sql-query-sign.md), [SIN](sql-query-sin.md), [SQRT](sql-query-sqrt.md), [KARE](sql-query-square.md), [TAN](sql-query-tan.md), [TRUNC](sql-query-trunc.md) [LOG](sql-query-log.md) |
|[Uzamsal işlevler](sql-query-spatial-functions.md)|Uzamsal işlevler uzamsal nesne giriş değeri üzerinde bir işlem gerçekleştirir ve sayısal veya Boolean değerini döndürer. | [ST_DISTANCE](sql-query-st-distance.md), [ST_INTERSECTS](sql-query-st-intersects.md), [ST_ISVALID](sql-query-st-isvalid.md), [ST_ISVALIDDETAILED](sql-query-st-isvaliddetailed.md), [ST_WITHIN](sql-query-st-within.md) |
|[Dize işlevleri](sql-query-string-functions.md)|Dize işlevleri bir dize giriş değeri üzerinde bir işlem gerçekleştirir ve bir dize, sayısal veya Boolean değeri döndürün. | [CONCAT](sql-query-concat.md), [İçERİr](sql-query-contains.md), [ENDSWITH](sql-query-endswith.md), [INDEX_OF](sql-query-index-of.md), [SOL](sql-query-left.md), [UZUNLUK](sql-query-length.md), [ALT](sql-query-lower.md), [LTRIM](sql-query-ltrim.md), [DEĞİşTİr ,](sql-query-replace.md) [ÇOĞALTMA](sql-query-replicate.md), [TERS](sql-query-reverse.md), [SAĞ](sql-query-right.md), [RTRIM](sql-query-rtrim.md), [STARTSWITH](sql-query-startswith.md), [StringToArray](sql-query-stringtoarray.md), [StringToBoolean](sql-query-stringtoboolean.md), [StringToNull](sql-query-stringtonull.md), [StringToNumber](sql-query-stringtonumber.md), [StringToObject](sql-query-stringtoobject.md), ALT STRING , [TOString](sql-query-substring.md), [TRIM](sql-query-trim.md), [ÜST](sql-query-upper.md) [ToString](sql-query-tostring.md) |
|[Tür denetimi işlevleri](sql-query-type-checking-functions.md)|Tür denetimi işlevleri, SQL sorgularında bir ifade türünü denetlemenize olanak sağlar. | [IS_ARRAY](sql-query-is-array.md), [IS_BOOL](sql-query-is-bool.md), [IS_DEFINED](sql-query-is-defined.md), [IS_NULL](sql-query-is-null.md), [IS_NUMBER](sql-query-is-number.md), [IS_OBJECT](sql-query-is-object.md), [IS_PRIMITIVE](sql-query-is-primitive.md), [IS_STRING](sql-query-is-string.md) |

## <a name="built-in-versus-user-defined-functions-udfs"></a>Dahili ve Kullanıcı Tanımlı Fonksiyonlar (UDFs)

Şu anda yerleşik bir işlevin kullanılabildiği kullanıcı tanımlı bir işlev (UDF) kullanıyorsanız, ilgili yerleşik işlevin çalışması daha hızlı ve daha verimli olacaktır.

## <a name="built-in-versus-ansi-sql-functions"></a>Dahili ve ANSI SQL işlevleri

Cosmos DB işlevleri ile ANSI SQL işlevleri arasındaki temel fark, Cosmos DB işlevlerinin şemasız ve karma şema verileriyle iyi çalışacak şekilde tasarlanmış olmasıdır. Örneğin, bir özellik eksikse veya sayısal olmayan bir `unknown`değeri varsa, öğe hata döndürmek yerine atlanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye Giriş](introduction.md)
- [Dizi işlevleri](sql-query-array-functions.md)
- [Tarih ve saat işlevleri](sql-query-date-time-functions.md)
- [Matematik işlevleri](sql-query-mathematical-functions.md)
- [Uzamsal işlevler](sql-query-spatial-functions.md)
- [Dize işlevleri](sql-query-string-functions.md)
- [Tür denetimi işlevleri](sql-query-type-checking-functions.md)
- [Kullanıcı Tanımlı Fonksiyonlar](sql-query-udfs.md)
- [Toplamalar](sql-query-aggregates.md)
