---
title: Anlam yorumu-Bilgi Keşfetme Hizmeti API 'SI
titlesuffix: Azure Cognitive Services
description: Bilgi Keşfetme Hizmeti (KES) API 'sinde anlamsal yorumu nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 26f8d885f8cf85ab849ba221392df206e492aac4
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79220066"
---
# <a name="semantic-interpretation"></a>Anlam yorumu

Anlamsal yorum, semantik çıktıyı dilbilgisi aracılığıyla yorumlanan her bir yol ile ilişkilendirir.  Özellikle hizmet, son çıktıyı hesaplamak için yorum tarafından geçilen `tag` öğelerinde deyim dizisini değerlendirir.  

Bir ifade, bir sabit değerin veya bir değişkenin başka bir değişkene atanması olabilir.  Ayrıca, bir değişkene 0 veya daha fazla parametresi olan bir işlevin çıkışını atayabilir.  Her işlev parametresi, bir sabit değer veya değişken kullanılarak belirtilebilir.  İşlev herhangi bir çıkış döndürmezse, atama atlanır.

```xml
<tag>x = 1; y = x;</tag>
<tag>q = All(); q = And(q, q2);</tag>
<tag>AssertEquals(x, 1);</tag>
```

Bir değişken, bir harfle başlayan ve yalnızca harflerden (A-Z), rakamlardan (0-9) ve alt çizgi (\_) oluşan bir ad tanımlayıcısı kullanılarak belirtilir.  Türü, kendisine atanan değişmez değer veya işlev çıkış değerinden dolaylı olarak algılanır. 

Şu anda desteklenen veri türlerinin bir listesi aşağıda verilmiştir:

|Tür|Açıklama|Örnekler|
|----|----|----|
|Dize|0 veya daha fazla karakter dizisi|"Merhaba Dünya!"<br/>""|
|Bool|Boole değeri|true<br/>false|
|Int32|32-bit işaretli tamsayı.  -2.1 E9 to 2.1 E9|123<br/>-321|
|Int64|64-bit işaretli tamsayı. -9.2 E18 ve 9.2 E18|9876543210|
|çift|Çift duyarlıklı kayan nokta. 1.7 e +/-308 (15 basamak)|123,456789<br/>-1 23456789e E2|
|Guid|Genel benzersiz tanımlayıcı|"602DD052-CC47-4B23-A16A-26B52D30C05B"|
|Sorgu|Dizindeki veri nesnelerinin bir alt kümesini belirten sorgu ifadesi|All ()<br/>Ve (*S1*, *S2*)|

## <a name="semantic-functions"></a>Anlam Işlevleri

Yerleşik bir semantik işlevler kümesi vardır.  Bunlar, gelişmiş sorguların oluşturulmasını sağlar ve dilbilgisi yorumlamalar üzerinde bağlama duyarlı bir denetim sağlar.

### <a name="and-function"></a>And Işlevi

`query = And(query1, query2);`

İki giriş sorgusunun kesişiminden oluşan bir sorgu döndürür.

### <a name="or-function"></a>Or Işlevi

`query = Or(query1, query2);`

İki giriş sorgusunun birleşimden oluşturulmuş bir sorgu döndürür.

### <a name="all-function"></a>All Işlevi

`query = All();`

Tüm veri nesnelerini içeren bir sorgu döndürür.

Aşağıdaki örnekte, 1 veya daha fazla anahtar sözcük kesişimine göre bir sorguyu tekrarlayarak oluşturmak için All () işlevini kullanırız.

```
<tag>query = All();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = And(query, keyword);</tag>
</item>
```

### <a name="none-function"></a>None Işlevi

`query = None();`

Veri nesnesi içermeyen bir sorgu döndürür.

Aşağıdaki örnekte, 1 veya daha fazla anahtar sözcük birleşimini temel alan bir sorguyu tekrarlayarak oluşturmak için None () işlevini kullanırız.

```
<tag>query = None();</tag>
<item repeat="1-">
  <attrref uri="academic#Keyword" name="keyword">
  <tag>query = Or(query, keyword);</tag>
</item>
```

### <a name="query-function"></a>Sorgu Işlevi

```
query = Query(attrName, value)
query = Query(attrName, value, op)
```

Özniteliği *attrName* , yalnızca "EQ" olarak varsayılan olarak belirtilen işlem op *değerine göre eşleşen* veri nesnelerini içeren *op*bir sorgu döndürür.  Genellikle, eşleşen giriş sorgusu dizesine dayalı bir sorgu oluşturmak için bir `attrref` öğesi kullanın.  Bir değer başka yollarla verildiyse veya elde edilmişse, bu değerle eşleşen bir sorgu oluşturmak için Query () işlevi kullanılabilir.

Aşağıdaki örnekte, belirli bir yılda Akademik yayınlar belirtmek için destek uygulamak üzere Query () işlevini kullanırız.

```xml
written in the 90s
<tag>
  beginYear = Query("academic#Year", 1990, "ge");
  endYear = Query("academic#Year", 2000, "lt");
  year = And(beginYear, endYear);
</tag>
```

### <a name="composite-function"></a>Bileşik Işlev

`query = Composite(innerQuery);`

Ortak bir *bileşik öznitelik özniteliğinin*alt özniteliklerine yönelik eşleştirmelerle oluşan bir *ınnerquery* 'yi kapsülleyen bir sorgu döndürür.  Kapsülleme, eşleşen herhangi bir veri nesnesinin *bileşik öznitelik özniteliğinin* öğesini tek tek tek bir *ınnerquery*ile karşılayan en az bir değere sahip olmasını gerektirir.  Bileşik bir özniteliğin alt özniteliklerine ait bir sorgunun, diğer sorgularla birleştirilebilmesi için, bileşik () işlevi kullanılarak kapsüllenmesi gerektiğini unutmayın.

Örneğin, aşağıdaki sorgu "Microsoft" iken "Harry Shum" ile akademik yayınlar döndürüyor:
```
Composite(And(Query("academic#Author.Name", "harry shum"), 
              Query("academic#Author.Affiliation", "microsoft")));
```

Diğer taraftan, aşağıdaki sorgu, yazarlardan birinin "Harry Shum" olduğu ve bu yayınların birinin "Microsoft" olduğu Akademik yayınlar döndürür:
```
And(Composite(Query("academic#Author.Name", "harry shum"), 
    Composite(Query("academic#Author.Affiliation", "microsoft")));
```

### <a name="getvariable-function"></a>GetVariable Işlevi

`value = GetVariable(name, scope);`

Belirtilen *kapsam*altında tanımlanan değişken *adının* değerini döndürür.  *ad* , bir harfle başlayan ve yalnızca harf (a-Z), rakam (0-9) ve alt çizgi (_) içeren bir tanıtıcıdır.  *kapsam* , "istek" veya "sistem" olarak ayarlanabilir.  Farklı kapsamlar altında tanımlanan değişkenlerin, anlam işlevlerinin çıktısı aracılığıyla tanımlananları dahil olmak üzere birbirinden farklı olduğunu unutmayın.

İstek kapsamı değişkenleri, geçerli yorumlama isteği içindeki tüm yorumlamalar arasında paylaşılır.  Bunlar, dilbilgisinde yorumlamalar aramasını denetlemek için kullanılabilir.

Sistem değişkenleri, hizmet tarafından önceden tanımlanmıştır ve sistemin geçerli durumu hakkında çeşitli istatistikler almak için kullanılabilir.  Şu anda desteklenen sistem değişkenlerinin kümesi aşağıda verilmiştir:

|Adı|Tür|Açıklama|
|----|----|----|
|IsAtEndOfQuery|Bool|geçerli yorum tüm giriş sorgusu metinleriyle eşleşirse doğru|
|IsBeyondEndOfQuery|Bool|geçerli yorum, giriş sorgusu metninin ötesinde tamamlama önerdiyse doğru|

### <a name="setvariable-function"></a>SetVariable Işlevi

`SetVariable(name, value, scope);`

Belirtilen *kapsam*altındaki değişken *adına* *değer* atar.  *ad* , bir harfle başlayan ve yalnızca harf (a-Z), rakam (0-9) ve alt çizgi (_) içeren bir tanıtıcıdır.  Şu anda, *kapsam* için geçerli olan tek değer "istek" dir.  Ayarlanabilir bir sistem değişkeni yoktur.

İstek kapsamı değişkenleri, geçerli yorumlama isteği içindeki tüm yorumlamalar arasında paylaşılır.  Bunlar, dilbilgisinde yorumlamalar aramasını denetlemek için kullanılabilir.

### <a name="assertequals-function"></a>AssertEquals Işlevi

`AssertEquals(value1, value2);`

*Değer1* ve *değer2* eşdeğer ise, işlev başarılı olur ve yan etkileri yoktur.  Aksi takdirde, işlev başarısız olur ve yorumu reddeder.

### <a name="assertnotequals-function"></a>Assertnotquals Işlevi

`AssertNotEquals(value1, value2);`

*Değer1* ve *değer2* eşdeğer değilse, işlev başarılı olur ve yan etkileri yoktur.  Aksi takdirde, işlev başarısız olur ve yorumu reddeder.


