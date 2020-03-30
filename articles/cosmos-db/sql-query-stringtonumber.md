---
title: Azure Cosmos DB sorgu dilinde StringToNumber
description: Azure Cosmos DB'de SQL sistem fonksiyonu StringToNumber hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 5ca8d0c4a6d244823dda6f0f79a3cf5c743a12a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78296431"
---
# <a name="stringtonumber-azure-cosmos-db"></a>StringToNumber (Azure Cosmos DB)
 Bir Sayıya çevrilmiş ifadeyi döndürür. İfade çevrilemiyorsa, tanımsız döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
StringToNumber(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   JSON Numarası ifadesi olarak ayrıştırılması gereken bir dize ifadesidir. JSON'daki sayılar bir tamsayı veya kayan nokta olmalıdır. JSON formatı hakkında ayrıntılı bilgi için [json.org](https://json.org/)  
  
## <a name="return-types"></a>İade türleri
  
  Bir Sayı ifadesini veya tanımlanmamış bir ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `StringToNumber` farklı türlerde nasıl nasıl bir şekilde nasıl hissettiğini gösterir. 

Beyaz uzaya yalnızca Sayı'dan önce veya sonra izin verilir.

```sql
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

JSON'da geçerli bir Numara tamsayı veya kayan nokta numarası olmalıdır.

```sql
SELECT   
    StringToNumber("0xF")
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
{{}}
```  

Geçirilen ifade sayı ifadesi olarak ayrıştırılır; bu girdiler Sayı türü için değerlendirme yapmazlar ve böylece tanımsız döner. 

```sql
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
{{}}
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
