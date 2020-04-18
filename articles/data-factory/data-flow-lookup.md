---
title: Veri akışını eşlemede arama dönüşümü
description: Veri akışını eşlemede arama dönüşümlerini kullanarak başka bir kaynaktan gelen başvuru verileri.
author: kromerm
ms.reviewer: daperlov
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/23/2020
ms.openlocfilehash: 24fe11610d2a91fcdb0f09b8e45ea6ff4b81bd70
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606373"
---
# <a name="lookup-transformation-in-mapping-data-flow"></a>Veri akışını eşlemede arama dönüşümü

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bir veri akışı akışında başka bir kaynaktan gelen verilere başvurmak için arama dönüşümlerini kullanın. Arama dönüştürme, eşleşen verilerden kaynak verilerinize sütunekler.

Arama dönüşümü sol dış birleştirmeye benzer. Birincil akıştaki tüm satırlar, çıkış akışında, arama akışından ek sütunlar içeren olarak bulunur. 

## <a name="configuration"></a>Yapılandırma

![Arama Dönüşümü](media/data-flow/lookup1.png "Arama")

**Birincil akış:** Gelen veri akışı. Bu akış, birbirin sol tarafına eşdeğerdir.

**Arama akışı:** Birincil akışa eklenen veriler. Hangi verilerin eklendiği arama koşullarına göre belirlenir. Bu akış, birbirin sağ tarafına eşdeğerdir.

**Birden çok satırı eşleştirin:** Etkinleştirilmişse, birincil akışta birden çok eşleşme solan bir satır birden çok satır döndürecek. Aksi takdirde, 'Match on' koşuluna göre yalnızca tek bir satır döndürülür.

**Maç üzerinde:** Yalnızca 'Birden çok satırı eşleştir' etkinse görünür. Herhangi bir satırda, ilk eşleşmede mi yoksa son eşleşmede mi eşleşeceğinizi seçin. Herhangi bir satır en hızlı yürütülür olarak önerilir. İlk satır veya son satır seçilirse, sıralama koşullarını belirtmeniz gerekir.

**Arama koşulları:** Hangi sütunlarda eşleşeceğini zindan edin. Eşitlik koşulu karşılanırsa, satırlar eşleşebilir. Hover ve [veri akışı ifade dilini](data-flow-expression-functions.md)kullanarak bir değer ayıklamak için 'Hesaplanmış sütun' seçin.

Arama dönüşümü yalnızca eşitlik eşleşmelerini destekler. Arama ifadesini, daha büyük gibi diğer işleçleri içerecek şekilde özelleştirmek için, [birleştirme dönüşümünde çapraz birleştirme](data-flow-join.md#custom-cross-join)kullanılması önerilir. Çapraz birleştirme, yürütme de olası kartezyen ürün hatalarını önler.

Her iki akıştaki tüm sütunlar çıktı verilerine dahil edilir. Yinelenen veya istenmeyen sütunları düşürmek için, arama dönüşümünüz sonrasında [bir seçim dönüşümü](data-flow-select.md) ekleyin. Sütunlar, lavabo dönüşümünde de bırakılabilir veya yeniden adlandırılabilir.

## <a name="analyzing-matched-rows"></a>Eşleşen satırları çözümleme

Arama dönüşümünüzün ardından, `isMatch()` aramanın tek tek satırlarla eşleşip eşleşmeyeceğini görmek için işlev kullanılabilir.

![Arama deseni](media/data-flow/lookup111.png "Arama deseni")

Bu desenin bir `isMatch()` örneği, işlevüzerinde bölmek için koşullu bölme dönüştürme kullanıyor. Yukarıdaki örnekte, eşleşen satırlar üst akıştan geçer ve eşleşmeyen ```NoMatch``` satırlar akış boyunca akar.

## <a name="testing-lookup-conditions"></a>Arama koşullarını test etme

Hata ayıklama modunda veri önizlemesi ile arama dönüşümünün sınanırken, bilinen küçük bir veri kümesikullanın. Büyük bir veri kümesinden satırları örnekleme yaparken, sınama için hangi satırların ve anahtarların okunacağını tahmin edemezsiniz. Sonuç belirleyici değildir, yani birleştirme koşullarınız eşleşmeleri döndürmeyebilir.

## <a name="broadcast-optimization"></a>Yayın optimizasyonu

Azure Veri Fabrikası'nda veri eşleme akışları ölçeklenmiş Spark ortamlarında yürütülür. Veri kümeniz alt düğüm bellek alanına sığabiliyorsa, yayını etkinleştirerek arama performansınız en iyi duruma getirilebilir.

![Yayın A.B.D.](media/data-flow/broadcast.png "Yayın A.B.D.")

Yayını etkinleştirmek tüm veri kümesini belleğe iter. Yalnızca birkaç bin satır içeren daha küçük veri kümeleri için yayın, arama performansınızı büyük ölçüde artırabilir. Büyük veri kümeleri için bu seçenek bellek dışı bir özel durum neden olabilir.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<leftStream>, <rightStream>
    lookup(
        <lookupConditionExpression>,
        multiple: { true | false },
        pickup: { 'first' | 'last' | 'any' },  ## Only required if false is selected for multiple
        { desc | asc }( <sortColumn>, { true | false }), ## Only required if 'first' or 'last' is selected. true/false determines whether to put nulls first
        broadcast: { 'none' | 'left' | 'right' | 'both' }
    ) ~> <lookupTransformationName>
```
### <a name="example"></a>Örnek

![Arama Dönüşümü](media/data-flow/lookup-dsl-example.png "Arama")

Yukarıdaki arama yapılandırması için veri akışı komut dosyası aşağıdaki kod snippet bulunmaktadır.

```
SQLProducts, DimProd lookup(ProductID == ProductKey,
    multiple: false,
    pickup: 'first',
    asc(ProductKey, true),
    broadcast: 'none')~> LookupKeys
```
## 
Sonraki adımlar

* Birleştirme [join](data-flow-join.md) ve [var olan](data-flow-exists.md) dönüşümler hem birden çok akış girişi alır
* Eşleşen ve eşleşmeyen ```isMatch()``` değerlerde satırları bölmek için [koşullu bölme dönüştürme](data-flow-conditional-split.md) kullanma
