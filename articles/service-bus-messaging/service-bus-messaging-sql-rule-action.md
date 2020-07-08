---
title: Azure Service Bus 'de SQLRuleAction söz dizimi başvurusu
description: Bu makale, SQLRuleAction söz dizimi için bir başvuru sağlar. Eylemler, aracılı bir iletiyle gerçekleştirilen SQL dil tabanlı söz dizimine yazılır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 61fa6e046b4d4a0ba91bf8608c846755026d07ec
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85341574"
---
# <a name="sqlruleaction-syntax-reference-for-azure-service-bus"></a>Azure Service Bus için SQLRuleAction söz dizimi başvurusu

Bir *Sqlruleaction* , [sqlruleaction](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction) sınıfının bir örneğidir ve bir [brokeredmessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)öğesine karşı gerçekleştirilen SQL dil tabanlı sözdiziminde yazılmış eylemlerin kümesini temsil eder.   
  
Bu makalede, SQL kuralı eylem dilbilgisinde ayrıntıları listelenmektedir.  
  
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
  
## <a name="arguments"></a>Arguments  
  
-   `<scope>`, kapsamını belirten isteğe bağlı bir dizedir `<property_name>` . Geçerli değerler veya ' dir `sys` `user` . `sys`Değer, `<property_name>` [aracılı edmessage sınıfının](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)ortak özellik adı olan sistem kapsamını gösterir. `user`Kullanıcı kapsamını `<property_name>` , [aracılı Edmessage sınıf](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sözlüğünün bir anahtarı olduğunu gösterir. `user`belirtilmemişse, kapsam varsayılan kapsamdır `<scope>` .  
  
### <a name="remarks"></a>Açıklamalar  

Var olmayan bir sistem özelliğine erişme girişimi bir hatadır, ancak varolmayan bir Kullanıcı özelliğine erişme girişimi bir hata değildir. Bunun yerine, var olmayan bir Kullanıcı özelliği, dahili olarak bilinmeyen bir değer olarak değerlendirilir. Bilinmeyen bir değer, işleç değerlendirmesi sırasında özel olarak değerlendirilir.  
  
## <a name="property_name"></a>property_name  
  
```  
<property_name> ::=  
     <identifier>  
     | <delimited_identifier>  
  
<identifier> ::=  
     <regular_identifier> | <quoted_identifier> | <delimited_identifier>  
  
```  
  
### <a name="arguments"></a>Arguments  
 `<regular_identifier>`, aşağıdaki normal ifade tarafından temsil edilen bir dizedir:  
  
```  
[[:IsLetter:]][_[:IsLetter:][:IsDigit:]]*  
```  
  
 Bu, bir harfle başlayan ve ardından bir veya daha fazla alt çizgi/harf/rakam gelen bir dize anlamına gelir.  
  
 `[:IsLetter:]`Unicode harfi olarak kategorilere ayrılan herhangi bir Unicode karakter anlamına gelir. `System.Char.IsLetter(c)``true` `c` bir Unicode harfse döndürür.  
  
 `[:IsDigit:]`ondalık basamak olarak kategorilere ayrılan herhangi bir Unicode karakter anlamına gelir. `System.Char.IsDigit(c)``true` `c` Unicode basamağı ise döndürür.  
  
 `<regular_identifier>`, Ayrılmış bir anahtar sözcük olamaz.  
  
 `<delimited_identifier>`sol/sağ köşeli ayraç ([]) ile çevrelenen herhangi bir dizedir. Sağ köşeli ayraç iki sağ köşeli ayraç olarak gösterilir. Aşağıdakiler aşağıda verilmiştir `<delimited_identifier>` :  
  
```  
[Property With Space]  
[HR-EmployeeID]  
  
```  
  
 `<quoted_identifier>`, çift tırnak işaretleriyle çevrelenen herhangi bir dizedir. Tanımlayıcıdaki çift tırnak işareti iki çift tırnak işareti olarak temsil edilir. Bir dize sabiti ile kolayca karışabileceğinden, tırnak işareti tanımlayıcıları kullanılması önerilmez. Mümkünse sınırlı bir tanımlayıcı kullanın. Aşağıda bir örneği verilmiştir `<quoted_identifier>` :  
  
```  
"Contoso & Northwind"  
```  
  
## <a name="pattern"></a>kalıp  
  
```  
<pattern> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Açıklamalar
  
 `<pattern>`dize olarak değerlendirilen bir ifade olmalıdır. LIKE işleci için bir model olarak kullanılır.      Aşağıdaki joker karakterleri içerebilir:  
  
-   `%`: Herhangi bir sıfır veya daha fazla karakter dizesi.  
  
-   `_`: Herhangi bir tek karakter.  
  
## <a name="escape_char"></a>escape_char  
  
```  
<escape_char> ::=  
      <expression>  
```  
  
### <a name="remarks"></a>Açıklamalar
  
 `<escape_char>`1 uzunluğunda bir dize olarak değerlendirilen bir ifade olmalıdır. LIKE işleci için bir kaçış karakteri olarak kullanılır.  
  
 Örneğin, `property LIKE 'ABC\%' ESCAPE '\'` `ABC%` ile başlayan bir dize yerine eşleşmeler `ABC` .  
  
## <a name="constant"></a> sabiti  
  
```  
<constant> ::=  
      <integer_constant> | <decimal_constant> | <approximate_number_constant> | <boolean_constant> | NULL  
```  
  
### <a name="arguments"></a>Arguments  
  
-   `<integer_constant>`, tırnak işaretleri içine alınmış ve ondalık noktaları içermeyen sayıların bir dizesidir. Değerler dahili olarak depolanır `System.Int64` ve aynı aralığı izler.  
  
     Aşağıda, uzun sabitler örnekleri verilmiştir:  
  
    ```  
    1894  
    2  
    ```  
  
-   `<decimal_constant>`, tırnak işaretleri içine alınmış sayıların bir dizesidir ve bir ondalık noktası içerir. Değerler dahili olarak depolanır `System.Double` ve aynı aralığa/duyarlığa uyar.  
  
     Gelecekteki bir sürümde, bu numara tam sayı semantiğini desteklemek için farklı bir veri türünde depolanabilir, bu nedenle temel alınan veri türünün için olduğu olguyu temel almamalıdır `System.Double` `<decimal_constant>` .  
  
     Ondalık sabitlerin örnekleri aşağıda verilmiştir:  
  
    ```  
    1894.1204  
    2.0  
    ```  
  
-   `<approximate_number_constant>`bilimsel gösterimde yazılmış bir sayıdır. Değerler dahili olarak depolanır `System.Double` ve aynı aralığa/duyarlığa uyar. Aşağıda, yaklaşık sayıda Sabitte örnek verilmiştir:  
  
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
  
Boole sabitleri, anahtar kelimeleri veya ile temsil edilir `TRUE` `FALSE` . Değerler olarak depolanır `System.Boolean` .  
  
## <a name="string_constant"></a>string_constant  
  
```  
<string_constant>  
```  
  
### <a name="remarks"></a>Açıklamalar
  
Dize sabitleri tek tırnak işaretleri içine alınır ve geçerli Unicode karakterleri içerir. Bir dize sabitine gömülü tek bir tırnak işareti, iki tek tırnak işareti olarak temsil edilir.  
  
## <a name="function"></a> işlevi  
  
```  
<function> :=  
      newid() |  
      property(name) | p(name)  
```  
  
### <a name="remarks"></a>Açıklamalar  

`newid()`İşlevi, yöntemi tarafından oluşturulan bir **System. Guid** döndürür `System.Guid.NewGuid()` .  
  
`property(name)`İşlevi tarafından başvurulan özelliğin değerini döndürür `name` . `name`Değer, bir dize değeri döndüren geçerli bir ifade olabilir.  
  
## <a name="considerations"></a>Önemli noktalar

- SET, yeni bir özellik oluşturmak veya var olan bir özelliğin değerini güncelleştirmek için kullanılır.
- Kaldır özelliği kaldırmak için kullanılır.
- KÜME, ifade türü ve var olan özellik türü farklı olduğunda mümkün olduğunda örtük dönüştürme gerçekleştirir.
- Mevcut olmayan sistem özelliklerine başvuruluyorsa, eylem başarısız olur.
- Mevcut olmayan kullanıcı özelliklerine başvuruluyorsa eylem başarısız olmaz.
- Var olmayan bir Kullanıcı özelliği "Bilinmeyen" olarak değerlendirilir ve işleçler değerlendirilirken [Sqlfilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) ile aynı semantiğini takip edin.

## <a name="next-steps"></a>Sonraki adımlar

- [SQLRuleAction sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
- [SQLFilter sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
