---
title: Azure Cosmos DB'de SQL sabitleri
description: Azure Cosmos DB'deki SQL sorgu sabitlerinin belirli bir veri değerini temsil etmek için nasıl kullanıldığı hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: tisande
ms.openlocfilehash: cca62c358037dbe99fd16746ee081b1540161df2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873429"
---
# <a name="azure-cosmos-db-sql-query-constants"></a>Azure Cosmos DB SQL sorgu sabitleri  

 Gerçek veya skaler değer olarak da bilinen sabit, belirli bir veri değerini temsil eden bir simgedir. Bir kıymetin biçimi temsil eder değerin veri türüne bağlıdır.  
  
 **Desteklenen skaler veri türleri:**  
  
|**Tür**|**Değerler sırası**|  
|-|-|  
|**Tanımsız**|Tek değer: **tanımlanmamış**|  
|**Null**|Tek değer: **null**|  
|**Boole**|Değerler: **false**, **true**.|  
|**Numarası**|Çift duyarlıklı kayan nokta sayısı, IEEE 754 standardı.|  
|**Dize**|Sıfır veya daha fazla Unicode karakter dizisi. Dizeleri tek veya çift tırnak içinde eklenmelidir.|  
|**Dizi**|Sıfır veya daha fazla öğeden oluşan bir dizi. **Tanımsız**dışında her öğe, herhangi bir skaler veri türünün bir değeri olabilir.|  
|**Nesne**|Sıralanmamış sıfır veya daha fazla ad/değer çifti kümesi. Adı unicode dize, değer **tanımsız**dışında, herhangi bir skaler veri türü olabilir.|  
  
## <a name="syntax"></a><a name="bk_syntax"></a>Sözdizimi
  
```sql  
<constant> ::=  
   <undefined_constant>  
     | <null_constant>   
     | <boolean_constant>   
     | <number_constant>   
     | <string_constant>   
     | <array_constant>   
     | <object_constant>   
  
<undefined_constant> ::= undefined  
  
<null_constant> ::= null  
  
<boolean_constant> ::= false | true  
  
<number_constant> ::= decimal_literal | hexadecimal_literal  
  
<string_constant> ::= string_literal  
  
<array_constant> ::=  
    '[' [<constant>][,...n] ']'  
  
<object_constant> ::=   
   '{' [{property_name | "property_name"} : <constant>][,...n] '}'  
  
```  
  
##  <a name="arguments"></a><a name="bk_arguments"></a>Bağımsız değişken
  
* `<undefined_constant>; Undefined`  
  
  Tanımsız türün tanımlanmamış değerini temsil eder.  
  
* `<null_constant>; null`  
  
  **Null**türünün **null** değerini temsil eder.  
  
* `<boolean_constant>`  
  
  Boolean tipisabit temsil eder.  
  
* `false`  
  
  Boolean tipinin **yanlış** değerini temsil eder.  
  
* `true`  
  
  Boolean türünün **gerçek** değerini temsil eder.  
  
* `<number_constant>`  
  
  Sabiti temsil eder.  
  
* `decimal_literal`  
  
  Ondalık literalsayılar, ondalık gösterim veya bilimsel gösterim kullanılarak temsil edilen sayılardır.  
  
* `hexadecimal_literal`  
  
  Hexadecimal literals bir veya daha fazla hexadecimal basamak takip öneki '0x' kullanılarak temsil numaralarıdır.  
  
* `<string_constant>`  
  
  String türünde bir sabiti temsil eder.  
  
* `string _literal`  
  
  String literals, sıfır veya daha fazla Unicode karakter veya kaçış dizileri dizisi yle temsil edilen Unicode dizeleridir. Dize literals tek tırnak (apostrophe: ' ) veya çift tırnak (tırnak işareti: ") kapalıdır.  
  
  Aşağıdaki kaçış sıralarına izin verilir:  
  
|**Kaçış sırası**|**Açıklama**|**Unicode karakter**|  
|-|-|-|  
|\\'|keseli sıçan (')|U+0027|  
|\\"|tırnak işareti (")|U+0022|  
|\\\ |ters solidus\\( )|U+005C|  
|\\/|solidus (/)|U+002F|  
|\b|Backspace|U+0008|  
|\f|form beslemesi|U+000C|  
|\n|satır beslemesi|U+000A|  
|\r|satır başı|U+000D|  
|\t|Sekme|U+0009|  
|\uXXXX|4 hexadecimal basamakla tanımlanan Bir Unicode karakteri.|U+XXXX|  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Belge verilerini modelleme](modeling-data.md)
