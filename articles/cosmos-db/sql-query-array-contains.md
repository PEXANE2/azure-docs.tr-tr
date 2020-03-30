---
title: Azure Cosmos DB sorgu dilinde ARRAY_CONTAINS
description: Azure Cosmos DB'deki Dizi İçeren SQL sisteminin işlevinin, dizinin belirtilen değeri içerip içermediğini belirten bir Boolean'ı nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 47fe20888aa546e414b268b30c2e03580750a040
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303486"
---
# <a name="array_contains-azure-cosmos-db"></a>ARRAY_CONTAINS (Azure Cosmos DB)
Dizinin belirtilen değeri bulunup içermediğini belirten bir Boolean döndürür. Komut içinde boolean ifadesi kullanarak bir nesnenin kısmi veya tam eşleşmesini denetleyebilirsiniz. 

## <a name="syntax"></a>Sözdizimi
  
```sql
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*arr_expr*  
   Aranacak dizi ifadesidir.  
  
*expr*  
   Bulunacak ifadedir.  

*bool_expr*  
   Boolean bir ifadedir. 'True' olarak değerlendirirse ve belirtilen arama değeri bir nesneyse, komut kısmi eşleşmeyi denetler (arama nesnesi nesnelerden birinin alt kümesidir). 'False' olarak değerlendirirse, komut dizi içindeki tüm nesnelerin tam eşleşmesini denetler. Belirtilmemişse varsayılan değer yanlıştır. 
  
## <a name="return-types"></a>İade türleri
  
  Boolean değerini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek kullanarak bir dizi üyelik `ARRAY_CONTAINS`için kontrol etmek için .  
  
```sql
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples") AS b1,  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes") AS b2  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"b1": true, "b2": false}]  
```  

Aşağıdaki örnek, ARRAY_CONTAINS kullanarak bir dizideki JSON'un kısmi eşleşmesini nasıl denetleriz?  
  
```sql
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true) AS b1, 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}) AS b2,
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) AS b3 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{
  "b1": true,
  "b2": false,
  "b3": false
}]
```

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
