---
title: Azure Hizmet Veri Tos'unda SQLRuleAction sözdizimi başvurusu
description: Bu makalede, SQLRuleAction sözdizimi için bir başvuru sağlar. Eylemler, aracılı iletiye karşı gerçekleştirilen SQL tabanlı sözdiziminde yazılır.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 37615e39577ef60cccc9df91b61a6aa24ca794d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759637"
---
# <a name="sqlruleaction-syntax-reference-for-azure-service-bus"></a>Azure Hizmet Veri Servisi veri hizmetleri için SQLRuleAction sözdizimi başvurusu

*SqlRuleAction,* [SqlRuleAction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) sınıfının bir örneğidir ve [BrokeredMessage'a](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)karşı gerçekleştirilen SQL tabanlı sözdiziminde yazılmış eylemler kümesini temsil eder.   
  
Bu makalede, SQL kural eylem dilbilgisi ile ilgili ayrıntılar listelenin.  
  
```  
<statements> ::=
    <statement> [, ...n]  
  
```  
  
```  
<statement> ::=
    <action> [;]
    Remarks
    -------
    Semicolon is optional.  
  
```  
  
```  
<action> ::=
    SET <property> = <expression>
    REMOVE <property>  
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
  
-   `<scope>``<property_name>`'nin kapsamını gösteren isteğe bağlı bir dizedir. Geçerli değerler `sys` `user`veya . Değer, `sys` `<property_name>` [Aracılı İleti Sınıfının](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)ortak özellik adı olan sistem kapsamını gösterir. `user`[Aracılı](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) İleti `<property_name>` Sınıfı sözlüğünün anahtarının bulunduğu kullanıcı kapsamını gösterir. `user``<scope>` belirtilmemişse kapsam varsayılan kapsamdır.  
  
### <a name="remarks"></a>Açıklamalar  

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
  
 Bu, bir harfle başlayan ve ardından bir veya daha fazla alt alt puan/harf/basamak izleyen herhangi bir dize anlamına gelir.  
  
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
  
     Aşağıda uzun sabitlere örnekler verilmiştir:  
  
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
  
Boolean sabitleri anahtar kelimeler `TRUE` le `FALSE`temsil edilir veya. Değerler olarak `System.Boolean`saklanır.  
  
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

- SET, yeni bir özellik oluşturmak veya varolan bir özelliğin değerini güncelleştirmek için kullanılır.
- REMOVE özelliği kaldırmak için kullanılır.
- SET, ifade türü ve varolan özellik türü farklı olduğunda mümkünse örtülü dönüştürme gerçekleştirir.
- Var olmayan sistem özellikleri başvurulduysa eylem başarısız olur.
- Var olmayan kullanıcı özellikleri başvurulduysa eylem başarısız olmaz.
- Var olmayan bir kullanıcı özelliği, işleçleri değerlendirirken [SQLFilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) ile aynı semantikleri izleyerek dahili olarak "Bilinmeyen" olarak değerlendirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [SQLRuleAction sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [SQLFilter sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
