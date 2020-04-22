---
title: Veri akışını eşlemede pivot dönüşümü
description: Azure Veri Fabrikası eşleme veri akışı Pivot Dönüşümü'ni kullanarak satırlardan sütunlara veri döndürme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/30/2019
ms.openlocfilehash: a58444f81f60b48f9c2c76f13257a6a2431158a8
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686401"
---
# <a name="pivot-transformation-in-mapping-data-flow"></a>Veri akışını eşlemede pivot dönüşümü


[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Tek bir sütunun benzersiz satır değerlerinden birden çok sütun oluşturmak için pivot dönüşümünün kullanın. Pivot, sütunlara göre grup seçtiğiniz ve toplu işlevleri kullanarak pivot sütunları oluşturduğunuz bir toplama [dönüştürmesidir.](data-flow-expression-functions.md#aggregate-functions)

## <a name="configuration"></a>Yapılandırma

Pivot dönüşümü üç farklı giriş gerektirir: sütunlara göre gruplandırma, pivot tuşu ve pivoted sütunların nasıl üretilen

### <a name="group-by"></a>Gruplandırma ölçütü:

![Seçeneklere göre gruplandırma](media/data-flow/pivot2.png "[Seçeneklere göre gruplandırma")

Döndürülen sütunları toplamak için hangi sütunları seçin. Çıktı verileri, aynı gruptaki tüm satırları değerlere göre tek bir satırda gruplayacaktır. Döndürülen sütunda yapılan toplama her grup üzerinde oluşur.

Bu bölüm isteğe bağlıdır. Sütunlara göre grup seçili değilse, tüm veri akışı toplanır ve yalnızca bir satır çıktıalınır.

### <a name="pivot-key"></a>Pivot tuşu

![Pivot tuşu](media/data-flow/pivot3.png "Pivot tuşu")

Özet anahtar, satır değerleri yeni sütunlara döndürülen sütundur. Varsayılan olarak, özet dönüştürme her benzersiz satır değeri için yeni bir sütun oluşturur.

**Değer**etiketli bölüme, döndürülecek belirli satır değerlerini girebilirsiniz. Yalnızca bu bölümde girilen satır değerleri döndürülür. Null **değerini** etkinleştirmek sütundaki null değerleri için döndürülen bir sütun oluşturur.

### <a name="pivoted-columns"></a>Döndürülen sütunlar

![Döndürülen sütunlar](media/data-flow/pivot4.png "Döndürülen sütunlar")

Sütun haline gelen her benzersiz pivot anahtar değeri için, her grup için toplu bir satır değeri oluşturun. Pivot tuşu başına birden çok sütun oluşturabilirsiniz. Her pivot sütunen az bir [toplam işlevi](data-flow-expression-functions.md#aggregate-functions)içermelidir.

**Sütun adı deseni:** Her özet sütunun sütun adını nasıl biçimlendirecek lerini seçin. Outputted sütun adı pivot anahtar değeri, sütun öneki ve isteğe bağlı önek, yeterli, orta karakterler bir arada olacaktır. 

**Sütun düzenlemesi:** Pivot tuşu başına birden fazla pivot sütun uyrtuyorsanız, sütunların nasıl sıralenmesini istediğinizi seçin. 

**Sütun öneki:** Pivot tuşu başına birden fazla pivot sütunu oluşturursanız, her sütun için bir sütun öneki girin. Yalnızca bir döndürülen sütununuzun varsa, bu ayar isteğe bağlıdır.

## <a name="help-graphic"></a>Yardım grafiği

Aşağıdaki yardımcı grafik, farklı pivot bileşenlerinin birbirleriyle nasıl etkileşimde olduğunu gösterir

![Pivot yardım grafikleri](media/data-flow/pivot5.png "Pivot yardım grafiği")

## <a name="pivot-metadata"></a>Pivot meta verileri

Pivot tuşu yapılandırmasında hiçbir değer belirtilmemişse, döndürülen sütunlar çalışma zamanında dinamik olarak oluşturulur. Döndürülen sütun sayısı, pivot sütun sayısıyla çarpılarak benzersiz pivot anahtar değerlerinin sayısına eşit olacaktır. Bu değişen bir sayı olabileceğinden, UX sütun meta verilerini **Denetle** sekmesinde görüntülemez ve sütun yayılımı olmaz. Bu sütunları dönüştürmek için, veri akışını eşleme [sütun desen](concepts-data-flow-column-pattern.md) yeteneklerini kullanın. 

Belirli pivot anahtar değerleri ayarlanırsa, döndürülen sütunlar metadata.e'de görünür. sütun adları, İncele ve Lavabo eşlemesinde kullanılabilir.

### <a name="generate-metadata-from-drifted-columns"></a>Sürüklenen sütunlardan meta veri oluşturma

Özet, satır değerlerini temel alan dinamik olarak yeni sütun adları oluşturur. Bu yeni sütunları, daha sonra veri akışınızda başvurulan meta verilere ekleyebilirsiniz. Bunu yapmak için, veri önizlemehızlı eylem [sürüklenmiş harita](concepts-data-flow-schema-drift.md#map-drifted-columns-quick-action) kullanın. 

![Sütunları özetleme](media/data-flow/newpivot1.png "Harita sürüklendi Pivot sütunları")

### <a name="sinking-pivoted-columns"></a>Batan döner sütunlar

Döndürülen sütunlar dinamik olsa da, hedef veri deponuza yine de yazılabilir. Etkinleştir Lavabo ayarlarınızda **şema kaymasına izin verin.** Bu, meta verilere dahil olmayan sütunlar yazmanıza olanak sağlar. sütun meta verileriniz, ancak şema kayması seçeneği verileri elde etmenizi sağlar.

### <a name="rejoin-original-fields"></a>Özgün alanları yeniden birleştirme

Pivot dönüşümü yalnızca grubu sütunlara göre ve döndürerek yansıtacaktır. Çıktı verilerinizin diğer giriş sütunlarını içermesini istiyorsanız, [kendi kendine birleştirme](data-flow-join.md#self-join) deseni kullanın.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<incomingStreamName>
    pivot(groupBy(Tm),
        pivotBy(<pivotKeyColumn, [<specifiedColumnName1>,...,<specifiedColumnNameN>]),
        <pivotColumnPrefix> = <pivotedColumnValue>,
        columnNaming: '< prefix >< $N | $V ><middle >< $N | $V >< suffix >',
        lateral: { 'true' | 'false'}
    ) ~> <pivotTransformationName
```
### <a name="example"></a>Örnek

Yapılandırma bölümünde gösterilen ekranlar, aşağıdaki veri akışı komut dosyasına sahiptir:

```
BasketballPlayerStats pivot(groupBy(Tm),
    pivotBy(Pos),
    {} = count(),
    columnNaming: '$V$N count',
    lateral: true) ~> PivotExample

```

## <a name="next-steps"></a>Sonraki adımlar

Sütun değerlerini satır değerlerine dönüştürmek için [unpivot dönüşümdeneyin.](data-flow-unpivot.md) 
