---
title: Koşullu bilişsel beceri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'daki koşullu beceri, filtreleme, varsayılanlar oluşturma ve değerleri beceri tanımında birleştirme olanağı sağlar.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: b5f1fc7f877854dd06fbbe09ff82e47208fa12d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72792039"
---
# <a name="conditional-cognitive-skill"></a>Koşullu bilişsel beceri

**Koşullu** beceri, bir çıktıya atayabilmek için verileri belirlemek için Boolean işlemi gerektiren Azure Bilişsel Arama senaryolarını sağlar. Bu senaryolar filtreleme, varsayılan değer atama ve bir koşula dayalı verileri birleştirme içerir.

Aşağıdaki pseudocode koşullu beceri nin neler başardığını gösterir:

```
if (condition) 
    { output = whenTrue } 
else 
    { output = whenFalse } 
```

> [!NOTE]
> Bu beceri bir Azure Bilişsel Hizmetler API'sine bağlı değildir ve kullanmak için ücretlendirilmezsiniz. Ancak, sizi günde az sayıda zenginleştirmeyle sınırlayan "Ücretsiz" kaynak seçeneğini geçersiz kılmak için yine de [bir Bilişsel Hizmetler kaynağı eklemeniz](cognitive-search-attach-cognitive-services.md) gerekir.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ConditionalSkill


## <a name="evaluated-fields"></a>Değerlendirilen alanlar

Girdileri alanlar değerlendirildiği için bu beceri özeldir.

Aşağıdaki öğeler bir ifadenin geçerli değerleridir:

-   Ek açıklama yolları (ifadelerdeki yollar "$(" ve ")" ile sınırlandırılmalıdır)
 <br/>
    Örnekler:
    ```
        "= $(/document)"
        "= $(/document/content)"
    ```

-  Literals (dizeleri, sayılar, doğru, yanlış, null) <br/>
    Örnekler:
    ```
       "= 'this is a string'"   // string (note the single quotation marks)
       "= 34"                   // number
       "= true"                 // Boolean
       "= null"                 // null value
    ```

-  Karşılaştırma işleçleri kullanan ifadeler (==, !=, >=, >, <=, <) <br/>
    Örnekler:
    ```
        "= $(/document/language) == 'en'"
        "= $(/document/sentiment) >= 0.5"
    ```

-   Boolean işleçlerini kullanan ifadeler (&&, ||, !, ^) <br/>
    Örnekler:
    ```
        "= $(/document/language) == 'en' && $(/document/sentiment) > 0.5"
        "= !true"
    ```

-   Sayısal işleçkullanan ifadeler (+, -, \*, /, %) <br/>
    Örnekler: 
    ```
        "= $(/document/sentiment) + 0.5"         // addition
        "= $(/document/totalValue) * 1.10"       // multiplication
        "= $(/document/lengthInMeters) / 0.3049" // division
    ```

Koşullu beceri değerlendirmeyi desteklediğinden, bunu küçük dönüşüm senaryolarında kullanabilirsiniz. Örneğin, [beceri tanımına](#transformation-example)bakın 4 .

## <a name="skill-inputs"></a>Beceri girdileri
Girişler büyük/küçük harf duyarlıdır.

| Giriş   | Açıklama |
|-------------|-------------|
| Durum   | Bu giriş, [değerlendirilecek](#evaluated-fields) koşulu temsil eden değerlendirilmiş bir alandır. Bu durum boolean değeri *(doğru* veya *yanlış)* değerlendirmek gerekir.   <br/>  Örnekler: <br/> "= doğru" <br/> "= $(/document/language) =='fr'" <br/> "= $(/document/pages/\*/language) == $(/document/expectedLanguage)" <br/> |
| whenTrue    | Bu giriş, koşul *doğru*değerlendirilirse döndürülecek değeri temsil eden [değerlendirilmiş](#evaluated-fields) bir alandır. Sabitler dizeleri tek tırnak işaretleri (' ve ') olarak döndürülmelidir. <br/>Örnek değerler: <br/> "= 'sözleşme'"<br/>"= $(/document/contractType)" <br/> "= $(/document/ntities/\*)" <br/> |
| whenFalse   | Bu giriş, koşul *false'a*değerlendirilirse döndürülecek değeri temsil eden [değerlendirilmiş](#evaluated-fields) bir alandır. <br/>Örnek değerler: <br/> "= 'sözleşme'"<br/>"= $(/document/contractType)" <br/> "= $(/document/ntities/\*)" <br/>

## <a name="skill-outputs"></a>Beceri çıktıları
Basitçe "çıktı" denen tek bir çıktı var. Koşul yanlışsa *false* veya koşul doğruysa *false* değerini döndürür.

## <a name="examples"></a>Örnekler

### <a name="sample-skill-definition-1-filter-documents-to-return-only-french-documents"></a>Örnek beceri tanımı 1: Yalnızca Fransızca belgeleri döndürmek için belgeleri filtreleme

Aşağıdaki çıktı, belgenin dili Fransızca ise bir dizi cümle ("/document/frenchSentences") döndürür. Dil Fransızca değilse, değer *null*olarak ayarlanır.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == 'fr'" },
        { "name": "whenTrue", "source": "/document/sentences" },
        { "name": "whenFalse", "source": "= null" }
    ],
    "outputs": [ { "name": "output", "targetName": "frenchSentences" } ]
}
```
"/document/frenchSentences" başka bir becerinin *bağlamı* olarak kullanılırsa, bu beceri yalnızca "/document/frenchSentences" *null*olarak ayarlenmemişse çalışır.


### <a name="sample-skill-definition-2-set-a-default-value-for-a-value-that-doesnt-exist"></a>Örnek beceri tanımı 2: Var olmayan bir değer için varsayılan değer ayarlama

Aşağıdaki çıktı, belgenin diline veya dil ayarlanmadığında "es" olarak ayarlanmış bir ek açıklama ("/document/languageWithDefault") oluşturur.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document",
    "inputs": [
        { "name": "condition", "source": "= $(/document/language) == null" },
        { "name": "whenTrue", "source": "= 'es'" },
        { "name": "whenFalse", "source": "= $(/document/language)" }
    ],
    "outputs": [ { "name": "output", "targetName": "languageWithDefault" } ]
}
```

### <a name="sample-skill-definition-3-merge-values-from-two-fields-into-one"></a>Örnek beceri tanımı 3: İki alandaki değerleri tek bir alanda birleştirme

Bu örnekte, bazı cümlelerin *frenchSentiment* özelliği bulunmaktadır. *FrenchSentiment* özelliği null olduğunda, *englishSentiment* değerini kullanmak isteriz. Çıktıyı *duyarlılık* ("/document/sentiment/*/sentiment") adı verilen bir üyeye atıyoruz.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= $(/document/sentences/*/frenchSentiment) == null" },
        { "name": "whenTrue", "source": "/document/sentences/*/englishSentiment" },
        { "name": "whenFalse", "source": "/document/sentences/*/frenchSentiment" }
    ],
    "outputs": [ { "name": "output", "targetName": "sentiment" } ]
}
```

## <a name="transformation-example"></a>Dönüşüm örneği
### <a name="sample-skill-definition-4-data-transformation-on-a-single-field"></a>Örnek beceri tanımı 4: Tek bir alanda veri dönüşümü

Bu örnekte, 0 ile 1 arasında bir *duyarlılık* alıyoruz. -1 ile 1 arasında dönüştürmek istiyoruz. Bu küçük dönüşümü yapmak için şartlı beceriyi kullanabiliriz.

Bu örnekte, koşul her zaman *doğru*olduğundan, becerinin koşullu yönünü kullanmayız.

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ConditionalSkill",
    "context": "/document/sentences/*",
    "inputs": [
        { "name": "condition", "source": "= true" },
        { "name": "whenTrue", "source": "= $(/document/sentences/*/sentiment) * 2 - 1" },
        { "name": "whenFalse", "source": "= 0" }
    ],
    "outputs": [ { "name": "output", "targetName": "normalizedSentiment" } ]
}
```

## <a name="special-considerations"></a>Özel hususlar
Bazı parametreler değerlendirilir, bu nedenle belgelenen deseni izlemek için özellikle dikkatli olmanız gerekir. İfadeler eşitler işaretiyle başlamalıdır. Bir yol "$(" ve ")" ile sınırlandırılmalıdır. Dizeleri tek tırnak işaretlerine koyduğunuzdan emin olun. Bu, değerlendiricinin dizeleri ve gerçek yolları ve işleçleri ayırt yardımcı olur. Ayrıca, işleçler etrafında beyaz boşluk koymak emin olun (örneğin, bir yol bir "*" çarpma daha farklı bir şey anlamına gelir).


## <a name="next-steps"></a>Sonraki adımlar

+ [Yerleşik yetenekler](cognitive-search-predefined-skills.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
