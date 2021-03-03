---
title: Eşleme veri akışında arama dönüşümü
description: Eşleme veri akışındaki arama dönüşümünü kullanarak başka bir kaynaktan veri başvurusu.
author: kromerm
ms.reviewer: daperlov
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/19/2021
ms.openlocfilehash: b8754742c572a8dbc1f55c64e47bec640d757d65
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101739377"
---
# <a name="lookup-transformation-in-mapping-data-flow"></a>Eşleme veri akışında arama dönüşümü

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri akışı akışındaki başka bir kaynaktaki verilere başvurmak için arama dönüşümünü kullanın. Arama dönüştürmesi, eşleşen verilerden alınan sütunları kaynak verilerinize ekler.

Bir arama dönüştürmesi, bir sol dış birleşime benzer. Birincil akıştaki tüm satırlar, arama akışından ek sütunları olan çıkış akışında bulunur.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4xsVT]

## <a name="configuration"></a>Yapılandırma

![Ekran görüntüsü, aşağıdaki metinde açıklanan etiketlere sahip arama ayarları sekmesini gösterir.](media/data-flow/lookup1.png "Arama")

**Birincil akış:** Gelen veri akışı. Bu akış, birleştirmenin sol tarafıyla eşdeğerdir.

**Arama akışı:** Birincil akışa eklenen veriler. Eklenen veriler arama koşullarına göre belirlenir. Bu akış, birleştirmenin sağ tarafıyla eşdeğerdir.

**Birden çok satırı eşleştir:** Etkinleştirilirse, birincil akışta birden fazla eşleşme içeren bir satır birden çok satır döndürür. Aksi takdirde, ' Match on ' koşuluna göre yalnızca tek bir satır döndürülür.

**Eşleşme:** Yalnızca ' birden fazla satır Eşleştir ' seçilirse görünür. Herhangi bir satır, ilk eşleşme veya son eşleşme ile eşleşmeyeceğinizi seçin. En hızlı şekilde çalıştığı için herhangi bir satır önerilir. İlk satır veya son satır seçiliyse sıralama koşullarını belirtmeniz gerekir.

**Arama koşulları:** Hangi sütunların eşleşeceğini seçin. Eşitlik koşulu karşılanıyorsa, satırlar eşleşme olarak kabul edilir. [Veri akışı ifade dilini](data-flow-expression-functions.md)kullanarak bir değer ayıklamak için üzerine gelin ve ' hesaplanan sütun ' seçeneğini belirleyin.

Her iki akıştaki tüm sütunlar çıktı verilerine dahil edilir. Yinelenen veya istenmeyen sütunları bırakmak için, arama dönüşümünüze sonra bir [seçme dönüşümü](data-flow-select.md) ekleyin. Ayrıca, bir havuz dönüşümünde sütunlar bırakılabilir veya yeniden adlandırılabilir.

### <a name="non-equi-joins"></a>Eşit olmayan birleşimler

Arama koşullarınıza eşit (! =) veya büyüktür (>) gibi koşullu bir işleç kullanmak için iki sütun arasındaki operatör açılan listesini değiştirin. Eşitlenmiş olmayan birleşimler, **en iyileştirme** sekmesinde **sabit** yayınlama kullanılarak yayınlanamayacak iki akışdan en az birini gerektirir.

![Eşlenmemiş arama](media/data-flow/non-equi-lookup.png "Eşlenmemiş arama")

## <a name="analyzing-matched-rows"></a>Eşleşen satırlar çözümleniyor

Arama dönüşümünüzün ardından işlev, `isMatch()` aramanın ayrı satırlar için eşleştirilmesinin olup olmadığını görmek için kullanılabilir.

![Arama kalıbı](media/data-flow/lookup111.png "Arama kalıbı")

Bu modele bir örnek, işlev üzerine bölünecek koşullu bölünmüş dönüştürmeyi kullanmaktır `isMatch()` . Yukarıdaki örnekte, eşleşen satırlar üst akış ve eşleşen olmayan satırları Stream üzerinden akar ```NoMatch``` .

## <a name="testing-lookup-conditions"></a>Arama koşullarını test etme

Hata ayıklama modundaki arama dönüşümünü veri önizleme ile sınarken, küçük bir bilinen veri kümesi kullanın. Büyük bir veri kümesinden satırlar örneklenirken, hangi satırların ve anahtarların test için okunacağını tahmin edemeyecektir. Sonuç belirleyici değildir, yani JOIN koşullarınızın herhangi bir eşleşme döndürmeyebilir.

## <a name="broadcast-optimization"></a>Yayın iyileştirmesi

![Yayın katılımı](media/data-flow/broadcast.png "Yayın katılımı")

Birleşimler, aramalar ve mevcut dönüşümde, bir veya her iki veri akışı çalışan düğümü belleğine sığması halinde **yayını** etkinleştirerek performansı iyileştirebilirsiniz. Spark altyapısı, varsayılan olarak bir kenar yayınlanıp yayınlanmayacağını otomatik olarak karar verir. Hangi tarafın yayınlanmak üzere el ile seçmek için, **sabit**' i seçin.

Birleşimlerinizin zaman aşımı hatalarıyla çalışmadığı **durumlar dışında yayınlamayı** devre dışı bırakmanız önerilmez.

## <a name="cached-lookup"></a>Önbelleğe alınan arama

Aynı kaynakta birden çok daha küçük arama yapıyorsanız, önbelleğe alınmış havuz ve arama arama dönüşümünüze göre daha iyi bir kullanım örneği olabilir. Bir önbellek havuzunun daha iyi bir veri deposunda en büyük değeri, bir hata iletisi veritabanına eşleşen hata kodlarını arayabileceği yaygın örnekler. Daha fazla bilgi için, [önbellek havuzları](data-flow-sink.md#cache-sink) ve [önbelleğe alınmış aramalar](concepts-data-flow-expression-builder.md#cached-lookup)hakkında bilgi edinin.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Syntax

```
<leftStream>, <rightStream>
    lookup(
        <lookupConditionExpression>,
        multiple: { true | false },
        pickup: { 'first' | 'last' | 'any' },  ## Only required if false is selected for multiple
        { desc | asc }( <sortColumn>, { true | false }), ## Only required if 'first' or 'last' is selected. true/false determines whether to put nulls first
        broadcast: { 'auto' | 'left' | 'right' | 'both' | 'off' }
    ) ~> <lookupTransformationName>
```
### <a name="example"></a>Örnek

![Ekran görüntüsü aşağıdaki kodun arama ayarları sekmesini gösterir.](media/data-flow/lookup-dsl-example.png "Arama")

Yukarıdaki arama yapılandırması için veri akışı betiği aşağıdaki kod parçacığında bulunur.

```
SQLProducts, DimProd lookup(ProductID == ProductKey,
    multiple: false,
    pickup: 'first',
    asc(ProductKey, true),
    broadcast: 'auto')~> LookupKeys
```
## 
Sonraki adımlar

* [JOIN](data-flow-join.md) ve [Exists](data-flow-exists.md) dönüştürmelerinin ikisi de birden çok akış girişi alır
* Eşleşen ve eşleşmeyen değerlere satırları ayırmak için ile [koşullu bölünmüş dönüşüm](data-flow-conditional-split.md) kullanın ```isMatch()```
