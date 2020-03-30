---
title: Azure Cosmos DB sorgu dilinde tür denetimi işlevleri
description: Azure Cosmos DB'deki SQL sistem işlevlerini tür denetimi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: cac8be4c39737ca209653532d00375c8c9bee48e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71349070"
---
# <a name="type-checking-functions-azure-cosmos-db"></a>Tür denetimi işlevleri (Azure Cosmos DB)

Tür denetimi işlevleri, SQL sorgusundaki ifade türünü denetlemenize sağlar. Değişken veya bilinmeyen öğelerin içindeki özelliklerin türlerini belirlemek için tür denetleme işlevlerini kullanabilirsiniz. 

## <a name="functions"></a>İşlevler

Desteklenen yerleşik tür denetleme işlevleri tablosu aşağıda veda edilmiştir:

Aşağıdaki işlevler giriş değerlerine karşı tür denetimini destekler ve her biri boolean değerini döndürür.  
  
||||  
|-|-|-|  
|[IS_ARRAY](sql-query-is-array.md)|[IS_BOOL](sql-query-is-bool.md)|[IS_DEFINED](sql-query-is-defined.md)|  
|[IS_NULL](sql-query-is-null.md)|[IS_NUMBER](sql-query-is-number.md)|[IS_OBJECT](sql-query-is-object.md)|  
|[IS_PRIMITIVE](sql-query-is-primitive.md)|[IS_STRING](sql-query-is-string.md)||  
  

## <a name="next-steps"></a>Sonraki adımlar

- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
- [Kullanıcı Tanımlı Fonksiyonlar](sql-query-udfs.md)
- [Toplamalar](sql-query-aggregates.md)
