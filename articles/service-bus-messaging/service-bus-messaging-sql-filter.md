---
title: Azure Service Bus SQLFilter sözdizimi başvurusu | Microsoft Docs
description: Bu makalede, SQLFilter dilbilgisi hakkında ayrıntılar sağlanmaktadır. Bir SqlFilter, SQL-92 standardının bir alt kümesini destekler.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 8412dea583ae119b30976e53d4751411b45339a4
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85341603"
---
# <a name="sqlfilter-syntax"></a>SQLFilter söz dizimi

*Sqlfilter* nesnesi [sqlfilter sınıfının](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)bir örneğidir ve bir [brokeredmessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)tarafından değerlendirilen bir SQL dil tabanlı filtre ifadesini temsil eder. Bir SqlFilter, SQL-92 standardının bir alt kümesini destekler.  
  
 Bu konuda, SqlFilter dilbilgisinde ayrıntıları listelenmektedir.  
  
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
  
## <a name="arguments"></a>Arguments  
  
-   `<scope>`, kapsamını belirten isteğe bağlı bir dizedir `<property_name>` . Geçerli değerler veya ' dir `sys` `user` . `sys`Değer, `<property_name>` [aracılı edmessage sınıfının](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)ortak özellik adı olan sistem kapsamını gösterir. `user`Kullanıcı kapsamını `<property_name>` , [aracılı edmessage sınıf](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sözlüğünün bir anahtarı olduğunu gösterir. `user`belirtilmemişse, kapsam varsayılan kapsamdır `<scope>` .  
  
## <a name="remarks"></a>Açıklamalar

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
  
Bu dil, bir harfle başlayan ve ardından bir veya daha fazla alt çizgi/harf/rakam gelen bir dize anlamına gelir.  
  
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
  
     Bunlar, uzun sabitler örneğidir:  
  
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

Boole sabitleri, **true** veya **false**anahtar sözcükleriyle temsil edilir. Değerler olarak depolanır `System.Boolean` .  
  
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
  
## <a name="considerations"></a>Dikkat edilmesi gerekenler
  
Aşağıdaki [Sqlfilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) semantiğini göz önünde bulundurun:  
  
-   Özellik adları büyük/küçük harfe duyarlıdır.  
  
-   İşleçler mümkün olduğunda C# örtük dönüştürme semantiğini izler.  
  
-   Sistem Özellikleri, [Brokeredmessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) örneklerinde kullanıma sunulan ortak özelliklerdir.  
  
    Aşağıdaki semantiğini göz önünde bulundurun `IS [NOT] NULL` :  
  
    -   `property IS NULL`, `true` özelliği yoksa veya özelliğin değeri olan olarak değerlendirilir `null` .  
  
### <a name="property-evaluation-semantics"></a>Özellik değerlendirmesi semantiği  
  
- Var olmayan bir sistem özelliğini değerlendirme girişimi bir [Filterexception](/dotnet/api/microsoft.servicebus.messaging.filterexception) özel durumu oluşturur.  
  
- Varolmayan bir özellik dahili olarak **Bilinmeyen**olarak değerlendirilir.  
  
  Aritmetik işleçlerinde bilinmeyen değerlendirme:  
  
- İkili işleçler için, işlenenlerin sol ve/veya sağ tarafı **bilinmiyor**olarak değerlendiriliyorsa, sonuç **bilinmez**.  
  
- Birli İşleçler için, bir işlenen **bilinmiyor**olarak değerlendiriliyorsa sonuç **bilinmez**.  
  
  İkili karşılaştırma işleçleri içinde bilinmeyen değerlendirme:  
  
- İşlenenlerin sol ve/veya sağ tarafı **bilinmiyor**olarak değerlendiriliyorsa, sonuç **bilinmez**.  
  
  İçinde bilinmeyen değerlendirme `[NOT] LIKE` :  
  
- Herhangi bir işlenen **Bilinmeyen**olarak değerlendiriliyorsa, sonuç **bilinmez**.  
  
  İçinde bilinmeyen değerlendirme `[NOT] IN` :  
  
- Sol işlenen **Bilinmeyen**olarak değerlendiriliyorsa, sonuç **bilinmez**.  
  
  **Ve** işlecinde bilinmeyen değerlendirme:  
  
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
  
 **Or** işlecinde bilinmeyen değerlendirme:  
  
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
  
### <a name="operator-binding-semantics"></a>İşleç bağlama semantiği
  
-   ,,,, Ve gibi karşılaştırma işleçleri, `>` `>=` `<` `<=` `!=` `=` veri türü yükseltmeleri ve örtük dönüşümlerde C# işleci bağlamasıyla aynı semantiğini izler.  
  
-   ,,, Ve gibi aritmetik işleçler, `+` `-` `*` `/` `%` veri türü yükseltmeleri ve örtük dönüşümlerde C# işleci bağlamasıyla aynı semantiğini izler.

## <a name="next-steps"></a>Sonraki adımlar

- [SQLFilter sınıfı (.NET Framework)](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)
- [SQLFilter sınıfı (.NET Standard)](/dotnet/api/microsoft.azure.servicebus.sqlfilter)
- [SQLRuleAction sınıfı](/dotnet/api/microsoft.servicebus.messaging.sqlruleaction)
