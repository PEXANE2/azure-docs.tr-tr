---
title: Eşleme veri akışında dönüştürmeyi seçin
description: Azure Data Factory eşleme veri akışı dönüşümü seçme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/02/2020
ms.openlocfilehash: 70e0a95a85920562af8bf9d3fffa6633709dccc5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84322099"
---
# <a name="select-transformation-in-mapping-data-flow"></a>Eşleme veri akışında dönüştürmeyi seçin

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Sütunları yeniden adlandırmak, bırakmak veya yeniden sıralamak için seçme dönüşümünü kullanın. Bu dönüşüm satır verilerini değiştirmez, ancak hangi sütunların aşağı akış olarak dağıtılacağını seçer. 

Bir SELECT dönüşümünde, kullanıcılar sabit eşlemeler belirtebilir, kural tabanlı eşleme yapmak için desenleri kullanabilir veya otomatik eşlemeyi etkinleştirebilir. Hem sabit hem de kural tabanlı eşlemeler aynı seçim dönüştürmesi içinde kullanılabilir. Bir sütun, tanımlı eşlemelerden biriyle eşleşmezse, bırakılır.

## <a name="fixed-mapping"></a>Sabit eşleme

Projeksiyonda tanımlanmış 50 ' den az sütun varsa, tüm tanımlı sütunlar varsayılan olarak sabit bir eşlemeye sahip olur. Sabit eşleme tanımlı, gelen sütununu alır ve tam bir adı eşleştirir.

![Sabit eşleme](media/data-flow/fixedmapping.png "Sabit eşleme")

> [!NOTE]
> Sabit eşleme kullanarak bir düzeltebilecekler sütununu eşleyemezsiniz veya yeniden adlandıramazsınız

### <a name="mapping-hierarchical-columns"></a>Hiyerarşik sütunları eşleme

Sabit eşlemeler, hiyerarşik bir sütunun alt sütununu en üst düzey bir sütuna eşlemek için kullanılabilir. Tanımlı bir hiyerarşiniz varsa, sütun açılan listesini kullanarak bir alt sütun seçin. Seçim dönüşümü, alt sütunun değeri ve veri türü ile yeni bir sütun oluşturur.

![hiyerarşik eşleme](media/data-flow/select-hierarchy.png "hiyerarşik eşleme")

## <a name="rule-based-mapping"></a>Kural tabanlı eşleme


> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4xiXz]

Birden çok sütunu aynı anda eşlemek veya düzeltebilecekler sütunları aşağı akış olarak geçirmek istiyorsanız, sütun düzenlerini kullanarak eşlemelerinizi tanımlamak için kural tabanlı eşleme kullanın. ,,, Ve sütunlarını temel alarak eşleştirin `name` `type` `stream` `position` . Sabit ve kural tabanlı eşlemelerin herhangi bir birleşimini kullanabilirsiniz. Varsayılan olarak, 50 'den büyük sütunları olan tüm projeksiyonlar varsayılan olarak, her sütunda eşleşen ve giriş yapan adı izleyen kural tabanlı bir eşleme olur. 

Kural tabanlı eşleme eklemek için **eşleme Ekle** ' ye tıklayın ve **kural tabanlı eşleme**' yi seçin.

![kural tabanlı eşleme](media/data-flow/rule2.png "Kural tabanlı eşleme")

Her kural tabanlı eşleme için iki giriş gerekir: ile eşleşmesi gereken durum ve her eşlenmiş sütunun adı. Her iki değer de [ifade Oluşturucusu](concepts-data-flow-expression-builder.md)aracılığıyla yapılır. Sol ifade kutusunda, Boolean eşleşme koşulunuz girin. Sağ ifade kutusunda, eşleşen sütunun ne eşleştirileceği belirtin.

![kural tabanlı eşleme](media/data-flow/rule-based-mapping.png "Kural tabanlı eşleme")

`$$`Eşleşen bir sütunun giriş adına başvurmak için söz dizimini kullanın. Yukarıdaki görüntünün bir örnek olarak kullanılması, bir kullanıcının adı altı karakterden kısa olan tüm dize sütunlarında eşleştirmek istediğini varsayalım. Gelen bir sütun adlandırıldıysa `test` , ifadesi `$$ + '_short'` sütunu yeniden adlandırır `test_short` . Var olan tek eşleme varsa, koşulu karşılamayan tüm sütunlar, outputfrom verilerinden bırakılır.

Desenler hem düzeltebilecekler hem de tanımlı sütunlarla eşleşir. Hangi tanımlı sütunların bir kuralla eşlendiğini görmek için kuralın yanındaki göz gözlük simgesine tıklayın. Veri önizlemeyi kullanarak çıktlarınızı doğrulayın.

### <a name="regex-mapping"></a>Regex eşleme

Aşağı köşeli çift ayraç simgesine tıklarsanız, bir Regex-Mapping koşulu belirtebilirsiniz. Bir Regex eşleme koşulu, belirtilen Regex koşuluyla eşleşen tüm sütun adlarıyla eşleşir. Bu, standart kural tabanlı eşlemelerle birlikte kullanılabilir.

![kural tabanlı eşleme](media/data-flow/regex-matching.png "Kural tabanlı eşleme")

Yukarıdaki örnek, Regex düzeniyle `(r)` veya küçük harf içeren bir sütun adı ile eşleşir. Standart kural tabanlı eşlemeye benzer şekilde, eşleşen tüm sütunlar, sözdizimi kullanılarak sağdaki koşul tarafından değiştirilir `$$` .

Sütun adınızla birden çok Regex eşleşmesi varsa, `$n` ' n ' öğesine karşılık gelen eşleşme ' ı kullanarak belirli eşleşmelere başvurabilirsiniz. Örneğin, ' $2 ', bir sütun adı içindeki ikinci eşleşmeyi ifade eder.

### <a name="rule-based-hierarchies"></a>Kural tabanlı hiyerarşiler

Tanımlı projeksiyonda bir hiyerarşisi varsa, hiyerarşiler alt sütunlarını eşlemek için kural tabanlı eşleme kullanabilirsiniz. Eşleşen bir koşul ve alt sütunlarını eşlemek istediğiniz karmaşık sütunu belirtin. Sağ tarafta belirtilen ' ad As ' kuralı kullanılarak eşleşen her alt sütun silinir.

![kural tabanlı eşleme](media/data-flow/rule-based-hierarchy.png "Kural tabanlı eşleme")

Yukarıdaki örnek, karmaşık sütunun tüm alt sütunlarında eşleşir `a` . `a`iki alt sütun `b` ve içerir `c` . Çıkış şeması iki sütun içerir `b` ve `c` ' ad As ' koşulu olur `$$` .

### <a name="parameterization"></a>Parametreleştirme

Kural tabanlı eşlemeyi kullanarak sütun adlarını parametreleştirebilirsiniz. ```name```Gelen sütun adlarını bir parametreye göre eşleştirmek için anahtar sözcüğünü kullanın. Örneğin, bir veri akışı parametresi varsa ```mycolumn``` , şuna eşit olan herhangi bir sütun adıyla eşleşen bir kural oluşturabilirsiniz ```mycolumn``` . Eşleşen sütununu ' iş anahtarı ' gibi sabit kodlanmış bir dize olarak yeniden adlandırabilir ve açıkça başvurabilirsiniz. Bu örnekte, eşleşen koşul ```name == $mycolumn``` ve ad koşulu ' iş anahtarı ' olur. 

## <a name="auto-mapping"></a>Otomatik eşleme

Bir seçme dönüşümü **eklenirken otomatik eşleme** kaydırıcı, otomatik eşleme kaydırıcısının geçerek etkinleştirilebilir. Otomatik eşleme ile, select dönüşümü, içerdikleri tüm gelen sütunları, içerdikleri aynı ada sahip olarak eşleştirir. Bu, düzeltebilecekler sütunları içerir. Bu, çıktı verilerinde şemada tanımlı olmayan sütunlar içerebilen anlamına gelir. Düzeltebilecekler sütunları hakkında daha fazla bilgi için bkz. [Schema DRFT](concepts-data-flow-schema-drift.md).

![Otomatik eşleme](media/data-flow/automap.png "Otomatik eşleme")

Üzerinde otomatik eşleme ile, select dönüşümü yineleme ayarlarını atla ' yı kabul eder ve var olan sütunlar için yeni bir diğer ad sağlar. Diğer ad, aynı akışta ve kendi kendine birleştirme senaryolarında birden çok birleştirme veya arama yaparken faydalıdır. 

## <a name="duplicate-columns"></a>Yinelenen sütunlar

Varsayılan olarak, select dönüşümü hem giriş hem de çıkış projeksiyonundaki yinelenen sütunları bırakır. Yinelenen giriş sütunları genellikle, birleştirmenin her tarafında sütun adlarının çoğaltılacağı JOIN ve Lookup dönüşümlerinden gelir. İki farklı giriş sütununu aynı ada eşledikten sonra yinelenen çıkış sütunları oluşabilir. Onay kutusunu değiştirerek yinelenen sütunlarda bırakma veya geçiş yapılıp yapılmayacağını seçin.

![Yinelenenleri atla](media/data-flow/select-skip-dup.png "Yinelenenleri atla")

## <a name="ordering-of-columns"></a>Sütunların sıralaması

Eşleşmelerin sırası, çıkış sütunlarının sırasını belirler. Bir giriş sütunu birden çok kez eşlenmişse, yalnızca ilk eşleme kabul edilir. Yinelenen bir sütun bırakma için, ilk eşleşme tutulacak.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Syntax

```
<incomingStream>
    select(mapColumn(
        each(<hierarchicalColumn>, match(<matchCondition>), <nameCondition> = $$), ## hierarchical rule-based matching
        <fixedColumn>, ## fixed mapping, no rename
        <renamedFixedColumn> = <fixedColumn>, ## fixed mapping, rename
        each(match(<matchCondition>), <nameCondition> = $$), ## rule-based mapping
        each(patternMatch(<regexMatching>), <nameCondition> = $$) ## regex mapping
    ),
    skipDuplicateMapInputs: { true | false },
    skipDuplicateMapOutputs: { true | false }) ~> <selectTransformationName>
```

### <a name="example"></a>Örnek

Aşağıda, bir seçme eşlemesinin ve veri akışı betiğinin bir örneği verilmiştir:

![Betik örneğini seçin](media/data-flow/select-script-example.png "Betik örneğini seçin")

```
DerivedColumn1 select(mapColumn(
        each(a, match(true())),
        movie,
        title1 = title,
        each(match(name == 'Rating')),
        each(patternMatch(`(y)`),
            $1 + 'regex' = $$)
    ),
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> Select1
```

## <a name="next-steps"></a>Sonraki adımlar
* Yeniden adlandırma, yeniden sıralama ve diğer ad Sütunlarını Seç ' i kullandıktan sonra, verileri bir veri deposuna eklemek için [Havuz dönüştürmeyi](data-flow-sink.md) kullanın.
