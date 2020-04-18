---
title: Azure Veri Fabrikası haritalama veri akışında sütun desenleri
description: Azure Veri Fabrikası haritalama veri akışlarında sütun desenlerini kullanarak genelleştirilmiş veri dönüştürme desenleri oluşturun
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/21/2019
ms.openlocfilehash: aacec8830948e08f66d71da88897670f7ef43788
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606126"
---
# <a name="using-column-patterns-in-mapping-data-flow"></a>Veri akışını eşlemede sütun desenlerini kullanma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Çeşitli eşleme veri akışı dönüşümleri, sabit kodlu sütun adları yerine desenleri temel alan şablon sütunlarına başvurmanızı sağlar. Bu eşleşme *sütun desenleri*olarak bilinir. Tam alan adları gerektirmek yerine sütunları ad, veri türü, akış veya konuma göre eşleşecek desenler tanımlayabilirsiniz. Sütun desenleri yararlı olduğu iki senaryo vardır:

* Gelen kaynak alanları genellikle metin dosyalarında veya NoSQL veritabanlarında sütun değiştirme durumu gibi değişirse. Bu senaryo [şema kayması](concepts-data-flow-schema-drift.md)olarak bilinir.
* Büyük bir sütun grubunda ortak bir işlem yapmak isterseniz. Örneğin, sütun adında 'toplam' olan her sütunu bir çifte dönüştürmek ister.

Sütun desenleri şu anda türetilmiş sütun, toplama, seçme ve lavabo dönüşümlerinde kullanılabilir.

## <a name="column-patterns-in-derived-column-and-aggregate"></a>Türemiş sütun ve toplamdaki sütun desenleri

Türetilmiş sütuna veya toplu dönüştürmenin Agregalar sekmesine sütun deseni eklemek için varolan sütunun sağındaki artı simgesini tıklatın. **Sütun deseni ekle'yi**seçin. 

![sütun desenleri](media/data-flow/columnpattern.png "Sütun Desenleri")

Eşleşme koşulunu girmek için [ifade oluşturucuyu](concepts-data-flow-expression-builder.md) kullanın. Sütunlara ve `name`sütuna göre `type` `stream` `position` eşleşen bir boolean ifadesi oluşturun. Desen, koşulun doğru döndüğü herhangi bir sütunu, sürüklenmiş veya tanımlanmış olan herhangi bir sütunu etkiler.

Eşleşme koşulunun altındaki iki ifade kutusu, etkilenen sütunların yeni adlarını ve değerlerini belirtir. Eşleşen `$$` alanın varolan değerine başvurmak için kullanın. Sol ifade kutusu adı tanımlar ve sağ ifade kutusu değeri tanımlar.

![sütun desenleri](media/data-flow/columnpattern2.png "Sütun Desenleri")

Yukarıdaki sütun deseni, her bir çift tür sütunuyla eşleşir ve maç başına bir toplam sütun oluşturur. Yeni sütunun adı, '_total' ile eşleşen sütunun adıdır. Yeni sütunun değeri, varolan çift değerin yuvarlatılmış, toplam toplamıdır.

Eşlediğiniz koşulun doğru olduğunu doğrulamak için, **Denetim** sekmesinde tanımlanan sütunların çıktı şemasını doğrulayabilir veya **Veri önizleme** sekmesindeki verilerin anlık görüntüsünü alabilirsiniz. 

![sütun desenleri](media/data-flow/columnpattern3.png "Sütun Desenleri")

## <a name="rule-based-mapping-in-select-and-sink"></a>Seç ve lavaboda kural tabanlı haritalama

Kaynak ve seçim dönüşümlerindeki sütunları eşleme yaparken, sabit eşleme veya kural tabanlı eşlemeler ekleyebilirsiniz. , `name`, `type` `stream`ve `position` sütunlara göre eşleştirin. Sabit ve kural tabanlı eşlemelerin herhangi bir birleşimine sahip olabilirsiniz. Varsayılan olarak, 50'den fazla sütuna sahip tüm projeksiyonlar, her sütunda eşleşen ve giriş adı olan kural tabanlı bir eşleme için varsayılan olarak. 

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

### <a name="rule-based-hierarchies"></a>Kural tabanlı hiyerarşiler

Tanımlı projeksiyonunuzun bir hiyerarşisi varsa, hiyerarşialt sütunlarını eşlemek için kural tabanlı eşleme yi kullanabilirsiniz. Eşlendirme yapmak istediğiniz alt sütunları eşleyen eşleşen bir koşul ve karmaşık sütun belirtin. Eşleşen her alt sütun, sağtarafta belirtilen 'Ad olarak' kuralı kullanılarak çıktılanır.

![kural tabanlı haritalama](media/data-flow/rule-based-hierarchy.png "Kural tabanlı eşleme")

Yukarıdaki örnek karmaşık sütunun `a`tüm alt sütunlarında eşleşir. `a`iki alt `b` sütun `c`içerir ve . Çıkış şeması iki sütun `b` içerecektir `c` ve 'Ad as' `$$`koşulu olarak.

## <a name="pattern-matching-expression-values"></a>Desen eşleşen ifade değerleri.

* `$$`çalışma zamanında her eşleşmenin adına veya değerine çevirir
* `name`her gelen sütunun adını temsil eder
* `type`her gelen sütunun veri türünü temsil eder
* `stream`akışınızdaki her akışla ilişkili adı veya dönüşümü temsil eder
* `position`veri akışınızdaki sütunların ordinal konumudur

## <a name="next-steps"></a>Sonraki adımlar
* Veri dönüşümleri için veri akışı [ifade dilieşleme](data-flow-expression-functions.md) hakkında daha fazla bilgi edinin
* [Lavabo dönüşümünde](data-flow-sink.md) sütun desenlerini kullanın ve kural tabanlı eşleme yle [dönüştürmeyi seçin](data-flow-select.md)
