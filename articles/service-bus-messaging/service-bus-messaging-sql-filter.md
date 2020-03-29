---
title: Azure Hizmet Veri Hizmetleri SQLFilter sözdizimi başvurusu | Microsoft Dokümanlar
description: Bu makalede, SQLFilter dilbilgisi hakkında ayrıntılar verilmektedir. SqlFilter, SQL-92 standardının bir alt kümesini destekler.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/05/2018
ms.author: spelluru
ms.openlocfilehash: d5a8e165fcee23c5feecd5935983dd77d3ec6c30
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759672"
---
# <a name="sqlfilter-syntax"></a>SQLFilter söz dizimi

*SqlFilter* nesnesi [SqlFilter sınıfının](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)bir örneğidir ve [BrokeredMessage'a](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)karşı değerlendirilen SQL dil tabanlı filtre ifadesini temsil eder. SqlFilter, SQL-92 standardının bir alt kümesini destekler.  
  
 Bu konu, SqlFilter dilbilgisi ile ilgili ayrıntıları listeler.  
  
```  
<predicate ::=  
      { NOT <predicate> }  
      | <predicate> AND <predicate>  
      | <predicate> OR <predicate>  
      | <expression> { = | <> | != | > | >= | < | <= } <expression>  
      | <property> IS [NOT] NULL  
      | <expression> [NOT] IN ( <expression> [, ...n] )  
      | <expression> [NOT] LIKE <pattern> [ESCAPE <escape_char>]  
      | EXISTS ( <property> )  
      | ( <predicate> )  
  
```  
  
```  
<expression> ::=  
      <constant>   
      | <function>  
      | <property>  
      | <expression> { + | - | * | / | % } <expression>  
      | { + | - } <expression>  
      | ( <expression> )  
  
```  
  
```  
<property> :=   
       [<scope> .] <property_name>  
  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler  
  
-   `<scope>``<property_name>`'nin kapsamını gösteren isteğe bağlı bir dizedir. Geçerli değerler `sys` `user`veya . Değer, `sys` `<property_name>` [BrokeredMessage sınıfının](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)ortak özellik adı olan sistem kapsamını gösterir. `user`[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) `<property_name>` sınıf sözlüğünün anahtarı nın bulunduğu kullanıcı kapsamını gösterir. `user``<scope>` belirtilmemişse kapsam varsayılan kapsamdır.  
  
## <a name="remarks"></a>Açıklamalar

Var olmayan bir sistem özelliğine erişme girişimi bir hatadır, var olmayan bir kullanıcı özelliğine erişme girişimi ise hata değildir. Bunun yerine, var olmayan bir kullanıcı özelliği dahili olarak bilinmeyen bir değer olarak değerlendirilir. Bilinmeyen bir değer operatör değerlendirmesi sırasında özel olarak ele alınmaktadır.  
  
## <a name="property_name"></a>property_name  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Bağımsız Değişkenler  

 `<regular_identifier>`aşağıdaki normal ifadeile temsil edilen bir dizedir:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
Bu dilbilgisi, bir harfle başlayan ve ardından bir veya daha fazla alt alt puan/harf/basamak izleyen herhangi bir dize anlamına gelir.  
  
`[:IsLetter:]`Unicode harfi olarak kategorize edilen herhangi bir Unicode karakteri anlamına gelir. `System.Char.IsLetter(c)`unicode harfi ise `true` `c` döndürür.  
  
`[:IsDigit:]`ondalık basamak olarak kategorize edilen herhangi bir Unicode karakteri anlamına gelir. `System.Char.IsDigit(c)`Unicode basamak ise `true` `c` döndürür.  
  
A `<regular_identifier>` ayrılmış bir anahtar kelime olamaz.  
  
`<delimited_identifier>`sol/sağ kare köşeli ayraçlarla ([]) çevrili herhangi bir dizedir. Sağ kare ayraç iki sağ kare ayraç olarak temsil edilir. Aşağıdaki örnekler şunlardır: `<delimited_identifier>`  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
`<quoted_identifier>`çift tırnak işaretleri ile kapalı herhangi bir dize. Tanımlayıcıdaki çift tırnak işareti iki çift tırnak işareti olarak temsil edilir. Bir dize sabiti ile kolayca karıştırılabileceğinden, alıntı yapan tanımlayıcıların kullanılması önerilmez. Mümkünse sınırlı bir tanımlayıcı kullanın. Aşağıdaki bir `<quoted_identifier>`örnektir:  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>Desen  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Açıklamalar
  
`<pattern>`dize olarak değerlendirilen bir ifade olmalıdır. LIKE işleci için bir desen olarak kullanılır.      Aşağıdaki joker karakter içerebilir:  
  
-   `%`: Sıfır veya daha fazla karakter dize.  
  
-   `_`: Herhangi bir tek karakter.  
  
## <a name="escape_char"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Açıklamalar  

`<escape_char>`uzunluk 1 dizesi olarak değerlendirilen bir ifade olmalıdır. LIKE işleci için kaçış karakteri olarak kullanılır.  
  
 Örneğin, `property LIKE 'ABC\%' ESCAPE '\'` `ABC%` `ABC`'' ile başlayan bir dize yerine eşleşir  
  
## <a name="constant"></a> sabiti  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Bağımsız Değişkenler  
  
-   `<integer_constant>`tırnak işaretlerine dahil olmayan ve ondalık nokta içermeyen sayılar dizisidir. Değerler dahili olarak `System.Int64` depolanır ve aynı aralığı izleyin.  
  
     Bunlar uzun sabitlere örnektir:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>`tırnak işaretlerine dahil olmayan ve ondalık nokta içeren bir sayı dizisidir. Değerler dahili olarak `System.Double` depolanır ve aynı aralığı/hassasiyeti izler.  
  
     Gelecek sürümde, bu sayı tam sayı semantiklerini desteklemek `System.Double` için farklı bir veri türünde depolanabilir, bu nedenle `<decimal_constant>`temel veri türünün .  
  
     Ondalık sabitlere örnekler aşağıda verilmiştir:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>`bilimsel gösterimle yazılmış bir sayıdır. Değerler dahili olarak `System.Double` depolanır ve aynı aralığı/hassasiyeti izler. Yaklaşık sayı sabitlerine örnekler aşağıda verilmiştir:  
  
    ```  
    101.5E5  
    0.5E-2  
    ```  
  
## <a name="boolean_constant"></a>boolean_constant  
  
```  
<boolean_constant> :=  
      TRUE | FALSE  
```  
  
### <a name="remarks"></a>Açıklamalar  

Boolean sabitleri **doğru** veya **YANLıŞ**anahtar kelimeleri ile temsil edilir. Değerler olarak `System.Boolean`saklanır.  
  
## <a name="string_constant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Açıklamalar  

Dize sabitleri tek tırnak işaretleriyle çevrilidir ve geçerli Unicode karakterlerini içerir. Bir dize sabitine katıştırılmış tek bir tırnak işareti iki tek tırnak işareti olarak temsil edilir.  
  
## <a name="function"></a> işlevi  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Açıklamalar
  
İşlev, `newid()` `System.Guid.NewGuid()` yöntem tarafından oluşturulan bir **System.Guid** döndürür.  
  
İşlev, `property(name)` başvurulan özelliğin değerini `name`döndürür. Değer, `name` dize değerini döndüren geçerli bir ifade olabilir.  
  
## <a name="considerations"></a>Dikkat edilmesi gerekenler
  
Aşağıdaki [SqlFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantiklerini göz önünde bulundurun:  
  
-   Özellik adları büyük/küçük harf duyarsızdır.  
  
-   Operatörler mümkün olduğunda C# örtük dönüştürme semantiklerini izler.  
  
-   Sistem [özellikleri, BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) örneklerinde açığa çıkan ortak özelliklerdir.  
  
    Aşağıdaki `IS [NOT] NULL` anlambilimi göz önünde bulundurun:  
  
    -   `property IS NULL``true` özellik yokmuş veya mülkün değeri `null`.  
  
### <a name="property-evaluation-semantics"></a>Özellik değerlendirme semantik  
  
- Var olmayan bir sistem özelliğini değerlendirme girişimi [FilterException özel](/dotnet/api/microsoft.servicebus.messaging.filterexception) durum oluşturur.  
  
- Var olmayan bir özellik içten **bilinmeyen**olarak değerlendirilir.  
  
  Aritmetik işleçlerde bilinmeyen değerlendirme:  
  
- İkili işleçler için, operandların sol ve/veya sağ **unknown**tarafı bilinmiyorsa, sonuç **bilinmemektedir.**  
  
- Unary operatörler için, bir operand **bilinmeyen**olarak değerlendirilirse, o zaman sonuç **bilinmemektedir.**  
  
  İkili karşılaştırma işleçlerinde bilinmeyen değerlendirme:  
  
- Operandların sol ve/veya sağ tarafı **bilinmiyorsa,** sonuç **bilinmemektedir.**  
  
  Bilinmeyen `[NOT] LIKE`değerlendirme:  
  
- Herhangi bir operand **bilinmeyen**olarak değerlendirilirse, o zaman sonuç **bilinmemektedir.**  
  
  Bilinmeyen `[NOT] IN`değerlendirme:  
  
- Sol operand **bilinmiyor**olarak değerlendirilirse, sonuç **bilinmemektedir.**  
  
  **AND** operatöründe bilinmeyen değerlendirme:  
  
```  
+---+---+---+---+  
|AND| T | F | U |  
+---+---+---+---+  
| T | T | F | U |  
+---+---+---+---+  
| F | F | F | F |  
+---+---+---+---+  
| U | U | F | U |  
+---+---+---+---+  
```  
  
 **OR** operatöründe bilinmeyen değerlendirme:  
  
```  
+---+---+---+---+  
|OR | T | F | U |  
+---+---+---+---+  
| T | T | T | T |  
+---+---+---+---+  
| F | T | F | U |  
+---+---+---+---+  
| U | T | U | U |  
+---+---+---+---+  
```  
  
### <a name="operator-binding-semantics"></a>Operatör bağlama semantik
  
-   Veri türü promosyonlarında `<` `<=`ve `!=` `=` örtülü dönüşümlerde C# işleci bağlayıcısı ile `>` `>=`aynı semantik leri,  
  
-   `+` `-`Veri türü promosyonlarında ve `%` örtülü dönüşümlerde C# işleci bağlayıcısı ile aynı anlambilimi takip eder ve aynı anlambilimi izler. `*` `/`

## <a name="next-steps"></a>Sonraki adımlar

- [SQLFilter sınıfı (.NET Framework)](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [SQLFilter sınıfı (.NET Standart)](/dotnet/api/microsoft.azure.servicebus.sqlfilter)
- [SQLRuleAction sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
