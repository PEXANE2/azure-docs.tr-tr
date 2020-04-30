---
title: Eşleme veri akışında Özet dönüştürme
description: Azure Data Factory eşleme veri akışı Özet dönüşümünü kullanarak satırlardan sütunlara kadar özet verileri özetleme
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/30/2019
ms.openlocfilehash: a58444f81f60b48f9c2c76f13257a6a2431158a8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81686401"
---
# <a name="pivot-transformation-in-mapping-data-flow"></a>Eşleme veri akışında Özet dönüştürme


[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Tek bir sütunun benzersiz satır değerlerinden birden çok sütun oluşturmak için pivot dönüşümünü kullanın. Pivot sütunları Gruplandır ' ı seçip [toplama işlevlerini](data-flow-expression-functions.md#aggregate-functions)kullanarak Özet sütunları oluşturabileceğiniz bir toplama dönüştürmesidir.

## <a name="configuration"></a>Yapılandırma

Özet dönüştürme üç farklı giriş gerektirir: sütunlara göre Gruplandır, pivot tuşu ve özetleme sütunlarının nasıl üretilemeyeceği

### <a name="group-by"></a>Gruplandırma ölçütü:

![Seçeneklere göre gruplandırma](media/data-flow/pivot2.png "[Gruplandırma ölçütü seçenekleri")

Özetleme sütunlarının toplanacak sütunları seçin. Çıkış verileri, aynı grupla birlikte tüm satırları tek bir satırda gruplar. Özetleme sütununda gerçekleştirilen toplama her bir grup üzerinde gerçekleşir.

Bu bölüm isteğe bağlıdır. Hiçbir gruplandırma sütunu seçilmezse, tüm veri akışı toplanır ve yalnızca bir satır silinir.

### <a name="pivot-key"></a>Özet anahtar

![Özet anahtar](media/data-flow/pivot3.png "Özet anahtar")

Pivot tuşu, satır değerlerinin yeni sütunlara göre özetlenen sütunundir. Varsayılan olarak, Özet dönüştürme her benzersiz satır değeri için yeni bir sütun oluşturur.

**Değer**etiketli bölümünde, özetlenecek belirli satır değerlerini girebilirsiniz. Yalnızca bu bölüme girilen satır değerleri özetlendirilecektir. **Null değeri** etkinleştirmek sütunundaki null değerler için bir özetleme sütunu oluşturacak.

### <a name="pivoted-columns"></a>Özetleme sütunları

![Özetleme sütunları](media/data-flow/pivot4.png "Özetleme sütunları")

Bir sütun haline gelen her benzersiz Özet anahtar değeri için, her grup için bir toplu satır değeri oluşturun. Her pivot anahtarı için birden çok sütun oluşturabilirsiniz. Her Özet sütun en az bir [toplama işlevi](data-flow-expression-functions.md#aggregate-functions)içermelidir.

**Sütun adı stili:** Her bir Özet sütununun sütun adını biçimlendirmeyi seçin. OutputColumn adı, PIVOT anahtar değeri, sütun öneki ve isteğe bağlı önek, yok, orta karakter birleşimi olacaktır. 

**Sütun düzenleme:** Özet anahtarı başına birden fazla Özet sütunu oluşturursanız, sütunların nasıl sıralanmasını istediğinizi seçin. 

**Sütun öneki:** Özet anahtarı başına birden fazla Özet sütunu oluşturursanız, her sütun için bir sütun öneki girin. Yalnızca bir özetleme sütununuzu varsa bu ayar isteğe bağlıdır.

## <a name="help-graphic"></a>Yardım grafiği

Aşağıdaki yardım grafiğinde farklı Özet bileşenlerinin birbirleriyle nasıl etkileşim kurduğu gösterilmektedir

![Özet yardım grafikleri](media/data-flow/pivot5.png "Özet yardım grafiği")

## <a name="pivot-metadata"></a>Özet meta verileri

Özet anahtar yapılandırmasında hiçbir değer belirtilmemişse, özetleme sütunları çalışma zamanında dinamik olarak oluşturulur. Özetleme sütunlarının sayısı, Özet sütun sayısıyla çarpılarak benzersiz Özet anahtar değerleri sayısına eşit olacaktır. Bu değişen bir sayı olabilir, bu durumda, UX sütun meta verilerini **İnceleme** sekmesinde görüntülemez ve sütun yayma olmaz. Bu sütunları dönüştürmek için veri akışının eşlenmesinin [sütun deseninin](concepts-data-flow-column-pattern.md) yeteneklerini kullanın. 

Belirli Özet anahtar değerleri ayarlandıysa, özetleme sütunları meta verilerde görüntülenir. e sütun adları, Inceleme ve havuz eşlemesinde sizin için kullanılabilir olacaktır.

### <a name="generate-metadata-from-drifted-columns"></a>Düzeltebilecekler sütunlarından meta veriler oluşturma

Pivot, satır değerlerine göre dinamik olarak yeni sütun adları oluşturur. Bu yeni sütunları, daha sonra veri akışınız içinde başvurulabilen meta verilere ekleyebilirsiniz. Bunu yapmak için, veri önizlemede [Map düzeltebilecekler](concepts-data-flow-schema-drift.md#map-drifted-columns-quick-action) hızlı eylemini kullanın. 

![Sütunları özetleme](media/data-flow/newpivot1.png "Map düzeltebilecekler Pivot sütunları")

### <a name="sinking-pivoted-columns"></a>Özetlenen sütunları dök

Özetleme sütunları dinamik olsa da, yine de hedef veri deponuza yazılabilir. Havuz ayarlarınızda **şema Drçıkmasına Izin ver** 'i etkinleştirin. Bu, meta verilerde bulunmayan sütunları yazmanıza izin verir. sütun meta verilerinizi, ancak şema DRFT seçeneği, verileri Land kullanmanıza olanak sağlayacak.

### <a name="rejoin-original-fields"></a>Özgün alanları yeniden katıl

Özet dönüştürme yalnızca gruplandırma ölçütü olan sütunları ve özetlenen sütunları proje olarak oluşturacak. Çıkış verilerinizin diğer giriş sütunlarını içermesini istiyorsanız, bir [kendi kendine JOIN](data-flow-join.md#self-join) örüntüsünün kullanın.

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

Yapılandırma bölümünde gösterilen ekranlar aşağıdaki veri akışı betiğine sahiptir:

```
BasketballPlayerStats pivot(groupBy(Tm),
    pivotBy(Pos),
    {} = count(),
    columnNaming: '$V$N count',
    lateral: true) ~> PivotExample

```

## <a name="next-steps"></a>Sonraki adımlar

Sütun değerlerini satır değerlerine dönüştürmek için [UNPIVOT dönüşümünü](data-flow-unpivot.md) deneyin. 
