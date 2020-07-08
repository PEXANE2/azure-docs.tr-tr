---
title: Azure Cosmos DB SQL sabitleri
description: Azure Cosmos DB içindeki SQL sorgu sabitlerinin belirli bir veri değerini göstermek için nasıl kullanıldığı hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: tisande
ms.openlocfilehash: cca62c358037dbe99fd16746ee081b1540161df2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74873429"
---
# <a name="azure-cosmos-db-sql-query-constants"></a>SQL sorgu sabitlerini Azure Cosmos DB  

 Sabit, sabit değer veya skaler değer olarak da bilinen, belirli bir veri değerini temsil eden bir simgedir. Bir sabit biçim, temsil ettiği değerin veri türüne bağlıdır.  
  
 **Desteklenen skaler veri türleri:**  
  
|**Tür**|**Değerler sırası**|  
|-|-|  
|**Tanımlayan**|Tek değer: **tanımsız**|  
|**Null**|Tek değer: **null**|  
|**Boole**|Değerler: **false**, **true**.|  
|**Sayı**|Bir çift duyarlıklı kayan noktalı sayı, IEEE 754 standardı.|  
|**Dize**|Sıfır veya daha fazla Unicode karakterden oluşan bir dizi. Dizeler tek veya çift tırnak içine alınmalıdır.|  
|**Dizi**|Sıfır veya daha fazla öğe dizisi. Her öğe, **tanımsız**bir değer hariç herhangi bir skaler veri türü değeri olabilir.|  
|**Nesne**|Sırasız bir sıfır veya daha fazla ad/değer çifti kümesi. Ad bir Unicode dizesidir, değer **tanımsız**olmayan herhangi bir skaler veri türünde olabilir.|  
  
## <a name="syntax"></a><a name="bk_syntax"></a>Syntax
  
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
  
##  <a name="arguments"></a><a name="bk_arguments"></a>Değişkenlerinden
  
* `<undefined_constant>; Undefined`  
  
  Tanımsız tür tanımsız değeri temsil eder.  
  
* `<null_constant>; null`  
  
  **Null**türünde **null** değeri temsil eder.  
  
* `<boolean_constant>`  
  
  Boolean türünde sabiti temsil eder.  
  
* `false`  
  
  Boolean türündeki **yanlış** değeri temsil eder.  
  
* `true`  
  
  Boolean türünün **true** değerini temsil eder.  
  
* `<number_constant>`  
  
  Bir sabiti temsil eder.  
  
* `decimal_literal`  
  
  Ondalık sabit değerleri, ondalık Gösterim ya da bilimsel gösterim kullanılarak temsil edilen sayılardır.  
  
* `hexadecimal_literal`  
  
  Onaltılık sabit değerler, ' 0x ' öneki kullanılarak ve ardından bir veya daha fazla onaltılık basamak tarafından temsil edilen sayılardır.  
  
* `<string_constant>`  
  
  String türünde bir sabiti temsil eder.  
  
* `string _literal`  
  
  Dize sabit değerleri, sıfır veya daha fazla Unicode karakter ya da kaçış dizileri tarafından temsil edilen Unicode dizeleridir. Dize sabit değerleri tek tırnak içine alınır (kesme işareti: ') veya çift tırnak işaretleri (tırnak işareti: ").  
  
  Aşağıdaki kaçış sıralarında izin verilir:  
  
|**Kaçış sırası**|**Açıklama**|**Unicode karakter**|  
|-|-|-|  
|\\'|kesme işareti (')|U + 0027|  
|\\"|tırnak işareti (")|U + 0022|  
|\\\ |ters eğik çizgi ( \\ )|U + 005C|  
|\\/|eğik çizgi (/)|U + 002F|  
|\b|geri al|U + 0008|  
|\f|Form akışı|U + 000C|  
|\n|satır besleme|U + 000A|  
|\r|satır başı|U + 000D|  
|\t|sekmesinde|U + 0009|  
|\Uxxxx içinde|4 onaltılık basamakla tanımlanan bir Unicode karakteri.|U + XXXX|  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Belge verilerini modelleme](modeling-data.md)
