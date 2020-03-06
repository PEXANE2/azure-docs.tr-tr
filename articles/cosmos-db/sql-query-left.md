---
title: Azure Cosmos DB sorgu dilinde kaldı
description: Azure Cosmos DB 'de SOLDAKI SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 0eac35a91e4d5158335d6797d49a09f8f6f391e3
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303758"
---
# <a name="left-azure-cosmos-db"></a>Sol (Azure Cosmos DB)
 Belirtilen sayıda karakteri içeren bir dize sol bölümünü döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
LEFT(<str_expr>, <num_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   , Öğesinden karakter Ayıklanacak dize ifadesidir.  
  
*num_expr*  
   , Karakter sayısını belirten sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, çeşitli uzunluğu değerler için sol "abc" bölümünü döndürür.  
  
```sql
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"l1": "a", "l2": "ab"}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
