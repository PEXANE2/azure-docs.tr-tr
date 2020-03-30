---
title: Azure Cosmos DB sorgu dilinde StringToBoolean
description: Azure Cosmos DB'de SQL sistem fonksiyonu StringToBoolean hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: ce11db91eff51e669f0917fbf34b1d560d0e9f07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78296550"
---
# <a name="stringtoboolean-azure-cosmos-db"></a>StringToBoolean (Azure Cosmos DB)
 Boolean'a çevrilmiş ifadeyi döndürür. İfade çevrilemiyorsa, tanımsız döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
StringToBoolean(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Boolean ifadesi olarak ayrıştırılması gereken bir dize ifadesidir.  
  
## <a name="return-types"></a>İade türleri
  
  Boolean ifadesini veya tanımsız ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `StringToBoolean` farklı türlerde nasıl nasıl bir şekilde nasıl hissettiğini gösterir. 
 
 Aşağıda geçerli girişli örnekler verilmiştir.

Whitespace'e yalnızca "true"/"false"dan önce veya sonra izin verilir.

```sql
SELECT 
    StringToBoolean("true") AS b1, 
    StringToBoolean("    false") AS b2,
    StringToBoolean("false    ") AS b3
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"b1": true, "b2": false, "b3": false}]
```  

Aşağıda geçersiz girişli örnekler verilmiştir.

 Booleans büyük/küçük harf duyarlı ve tüm küçük karakterler yani "doğru" ve "yanlış" ile yazılmalıdır.

```sql
SELECT 
    StringToBoolean("TRUE"),
    StringToBoolean("False")
```  

Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{}]
``` 

Geçirilen ifade boolean ifadesi olarak ayrıştırılacaktır; bu girdiler Boolean yazımı değerlendirmez ve böylece tanımsız döner.

```sql
SELECT 
    StringToBoolean("null"),
    StringToBoolean(undefined),
    StringToBoolean(NaN), 
    StringToBoolean(false), 
    StringToBoolean(true)
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
