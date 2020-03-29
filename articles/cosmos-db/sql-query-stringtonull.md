---
title: Azure Cosmos DB sorgu dilinde StringToNull
description: Azure Cosmos DB'de SQL sistem fonksiyonu StringToNull hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 7e17547f88465103b61eabec04978ea806ffa2c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78296448"
---
# <a name="stringtonull-azure-cosmos-db"></a>StringToNull (Azure Cosmos DB)
 Null'a çevrilmiş ifadeyi döndürür. İfade çevrilemiyorsa, tanımsız döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
StringToNull(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Null ifadesi olarak ayrıştırılması gereken bir dize ifadesidir.
  
## <a name="return-types"></a>İade türleri
  
  Null ifadesini veya tanımsız bir ifadeyi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `StringToNull` farklı türlerde nasıl nasıl bir şekilde nasıl hissettiğini gösterir. 

Aşağıda geçerli girişli örnekler verilmiştir.

 Whitespace'e yalnızca "null"dan önce veya sonra izin verilir.

```sql
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"n1": null, "n2": null, "n3": true}]
```  

Aşağıda geçersiz girişli örnekler verilmiştir.

Null büyük/küçük harf duyarlıdır ve tüm küçük karakterlerle yani "null" ile yazılmalıdır.

```sql
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{}]
```  

Geçirilen ifade null bir ifade olarak ayrıştırılır; bu girdiler null türü ne kadar değerlendirilmeyerek tanımsız olarak döndürülmez.

```sql
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{}]
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
