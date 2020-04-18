---
title: Veri akışını eşleme Dönüştürme seçin
description: Azure Veri Fabrikası haritalama veri akışı Dönüştürme'yi seçin
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/18/2020
ms.openlocfilehash: 71a7c812b71e67c7b729844d0672e520c88e2d08
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606342"
---
# <a name="select-transformation-in-mapping-data-flow"></a>Veri akışını eşlemede dönüşümü seçme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Sütunları yeniden adlandırmak, bırakmak veya yeniden sıralamak için select dönüşümünü kullanın. Bu dönüştürme satır verilerini değiştirmez, ancak hangi sütunların aşağı yayıltıldığı seçilir. 

Belirli bir dönüştürmede, kullanıcılar sabit eşlemeler belirtebilir, kural tabanlı eşleme yapmak için desenleri kullanabilir veya otomatik eşlemesini etkinleştirebilir. Sabit ve kural tabanlı eşlemeler her ikisi de aynı seçim dönüşümü içinde kullanılabilir. Bir sütun tanımlı eşlemelerden biriyle eşleşmiyorsa, sütun bırakılır.

## <a name="fixed-mapping"></a>Sabit eşleme

Projeksiyonunuzda tanımlanan 50'den az sütun varsa, tanımlanan tüm sütunlar varsayılan olarak sabit bir eşleme yle olur. Sabit bir eşleme tanımlı, gelen sütun alır ve tam bir ad eşler.

![Sabit eşleme](media/data-flow/fixedmapping.png "Sabit eşleme")

> [!NOTE]
> Sürüklenen bir sütunu sabit bir eşleçle eşlenemez veya yeniden adlandıramazsınız

### <a name="mapping-hierarchical-columns"></a>Hiyerarşik sütunları eşleme

Sabit eşlemeler, hiyerarşik bir sütunun alt sütununun üst düzey bir sütunla eşlenebilmek için kullanılabilir. Tanımlı bir hiyerarşiniz varsa, bir alt sütun seçmek için sütun açılır dosyasını kullanın. Select dönüşümü, alt sütunun değeri ve veri türünü içeren yeni bir sütun oluşturur.

![hiyerarşik haritalama](media/data-flow/select-hierarchy.png "hiyerarşik haritalama")

## <a name="rule-based-mapping"></a>Kural tabanlı eşleme

Aynı anda çok sayıda sütunun eşlemi yapmak veya sürüklenen sütunları akış aşağı aktarmak istiyorsanız, sütun desenlerini kullanarak eşlemelerinizi tanımlamak için kural tabanlı eşleme kullanın. , `name`, `type` `stream`ve `position` sütunlara göre eşleştirin. Sabit ve kural tabanlı eşlemelerin herhangi bir birleşimine sahip olabilirsiniz. Varsayılan olarak, 50'den fazla sütuna sahip tüm projeksiyonlar, her sütunda eşleşen ve giriş adı olan kural tabanlı bir eşleme için varsayılan olarak. 

Kural tabanlı eşleme eklemek için **eşleme ekle'yi** tıklatın ve **Kural tabanlı eşlemi**seçin.

![kural tabanlı haritalama](media/data-flow/rule2.png "Kural tabanlı eşleme")

Her kural tabanlı eşleme iki giriş gerektirir: eşlenen her sütunla eşleşecek koşul ve ne adlandırılabilmek. Her iki değer de [ifade oluşturucu](concepts-data-flow-expression-builder.md)aracılığıyla giriş yapılır. Sol ifade kutusuna boolean maç durumunuzu girin. Sağ ifade kutusunda, eşleşen sütunun eşleneceğini belirtin.

![kural tabanlı haritalama](media/data-flow/rule-based-mapping.png "Kural tabanlı eşleme")

Eşleşen `$$` bir sütunun giriş adına başvurmak için sözdizimini kullanın. Yukarıdaki resmi örnek olarak kullanarak, bir kullanıcının adları altı karakterden kısa olan tüm dize sütunlarında eşleşmek istediğini söyleyin. Gelen bir sütun adlandırılmışsa, `test`ifade `$$ + '_short'` sütunu `test_short`yeniden adlandıracaktır. Var olan tek eşleme buysa, koşulu karşılamayan tüm sütunlar çıktı edilen verilerden bırakılır.

Desenler, sürüklenen ve tanımlanan sütunlar ile eşleşir. Hangi tanımlanmış sütunların bir kuralla eşlenerek eşlendirileni görmek için kuralın yanındaki gözlük simgesini tıklatın. Veri önizlemesini kullanarak çıktınızı doğrulayın.

### <a name="regex-mapping"></a>Regex haritalama

Aşağı köşeli ayraç simgesini tıklattığınızda, bir regex eşleme koşulu belirtebilirsiniz. Regex eşleme koşulu, belirtilen regex koşuluyla eşleşen tüm sütun adlarıyla eşleşir. Bu standart kural tabanlı eşlemeler ile birlikte kullanılabilir.

![kural tabanlı haritalama](media/data-flow/regex-matching.png "Kural tabanlı eşleme")

Yukarıdaki örnek, regex `(r)` deseni veya küçük harf r içeren herhangi bir sütun adı ile eşleşir. Standart kural tabanlı eşleme benzer şekilde, eşleşen tüm sütunlar sözdizimi kullanılarak `$$` sağdaki koşula göre değiştirilir.

Sütun adınızda birden çok regex eşleşmesi varsa, `$n` 'n' hangi eşleşmeyi ifade eden yeri kullanarak belirli eşleşmelere başvurabilirsiniz. Örneğin, '$2' bir sütun adı içindeki ikinci eşleşmeyi ifade eder.

### <a name="rule-based-hierarchies"></a>Kural tabanlı hiyerarşiler

Tanımlı projeksiyonunuzun bir hiyerarşisi varsa, hiyerarşialt sütunlarını eşlemek için kural tabanlı eşleme yi kullanabilirsiniz. Eşlendirme yapmak istediğiniz alt sütunları eşleyen eşleşen bir koşul ve karmaşık sütun belirtin. Eşleşen her alt sütun, sağtarafta belirtilen 'Ad olarak' kuralı kullanılarak çıktılanır.

![kural tabanlı haritalama](media/data-flow/rule-based-hierarchy.png "Kural tabanlı eşleme")

Yukarıdaki örnek karmaşık sütunun `a`tüm alt sütunlarında eşleşir. `a`iki alt `b` sütun `c`içerir ve . Çıkış şeması iki sütun `b` içerecektir `c` ve 'Ad as' `$$`koşulu olarak.

### <a name="parameterization"></a>Parameterization

Kural tabanlı eşlemi kullanarak sütun adlarını parametreleştirebilirsiniz. Gelen sütun ```name``` adlarını bir parametreyle eşleştirmek için anahtar kelimeyi kullanın. Örneğin, bir veri akışı parametreniz ```mycolumn```varsa, ```mycolumn```'ye eşit olan herhangi bir sütun adıyla eşleşen bir kural oluşturabilirsiniz. Eşleşen sütunu 'iş anahtarı' gibi sabit kodlanmış bir dizeyle yeniden adlandırabilir ve açıkça başvurulayabilirsiniz. Bu örnekte, eşleşen ```name == $mycolumn``` koşul ve ad koşulu 'iş anahtarı'. 

## <a name="auto-mapping"></a>Otomatik haritalama

Bir seçim dönüşümü eklerken, Otomatik eşleme kaydırıcısı değiştirilerek **Otomatik eşleme** etkinleştirilebilir. Otomatik eşleme yle, select dönüşümü yinelenenler hariç tüm gelen sütunları girişleriyle aynı ada eşler. Bu, sürüklenen sütunları içerir, bu da çıktı verilerinin şemanızda tanımlanmamış sütunlar içerebileceği anlamına gelir. Sürüklenen sütunlar hakkında daha fazla bilgi için [şema kaymasına](concepts-data-flow-schema-drift.md)bakın.

![Otomatik haritalama](media/data-flow/automap.png "Otomatik haritalama")

Otomatik eşleme açıkken, select dönüşümü atlama yinelenen ayarlarını onurlandırır ve varolan sütunlar için yeni bir diğer ad sağlar. Diğer ad, aynı akışta ve kendi kendine birleştirme senaryolarında birden çok birleştirme veya arama yaparken yararlıdır. 

## <a name="duplicate-columns"></a>Yinelenen sütunlar

Varsayılan olarak, select dönüşümü hem giriş hem de çıktı projeksiyonundaki yinelenen sütunları düşürür. Yinelenen giriş sütunları genellikle birleştirme ve arama dönüşümlerinden gelir ve sütun adlarının birleşimin her iki tarafında çoğaltılır. İki farklı giriş sütununu aynı ada eşlerseniz yinelenen çıktı sütunları oluşabilir. Onay kutusunu değiştirerek yinelenen sütunları bırakıp bırakmayacağınız veya aktarıp aktarmayacağınızı seçin.

![Yinelenenleri Atla](media/data-flow/select-skip-dup.png "Yinelenenleri Atla")

## <a name="ordering-of-columns"></a>Sütun ların sıralanması

Eşleme sırası çıktı sütunlarının sırasını belirler. Bir giriş sütunu birden çok kez eşlenirse, yalnızca ilk eşleme onurlanır. Yinelenen sütun düşmesi için ilk eşleşme tutulur.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

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

Aşağıda, seçili bir eşleme ve veri akışı komut dosyasına bir örnek verilmiştir:

![Komut dosyası örneğini seçin](media/data-flow/select-script-example.png "Komut dosyası örneğini seçin")

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
* Sütunları yeniden adlandırmak, yeniden sıralamak ve diğer ad sütunları seçmek için Seç'i kullandıktan sonra, verilerinizi bir veri deposuna çekmek için [Lavabo dönüşümünü](data-flow-sink.md) kullanın.
