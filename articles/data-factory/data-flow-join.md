---
title: Veri akışını eşlemede dönüşüme katılma
description: Azure Veri Fabrikası eşleme veri akışında birleştirme dönüşümlerini kullanarak iki veri kaynağından gelen verileri birleştirme
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/02/2020
ms.openlocfilehash: 32100e9cad86f12dc8111ee8a0282a515540a4db
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346604"
---
# <a name="join-transformation-in-mapping-data-flow"></a>Veri akışını eşlemede dönüşüme katılma

Eşleme veri akışında iki kaynakveya akıştan gelen verileri birleştirmek için birleştirme dönüşümlerini kullanın. Çıktı akışı, birleştirme koşuluna göre eşleşen her iki kaynaktan tüm sütunları içerir. 

## <a name="join-types"></a>Birleştirme türleri

Veri akışlarını eşleme şu anda beş farklı birleştirme türünü destekler.

### <a name="inner-join"></a>İç Birleştirme

İç birleştirme yalnızca her iki tabloda eşleşen değerlere sahip çıktı satırları.

### <a name="left-outer"></a>Sol Dış

Sol dış birleştirme, sol akıştaki tüm satırları döndürür ve sağ akıştaki kayıtları eşleştirir. Sol akıştaki bir satır eşleşme yoksa, sağ akıştaki çıkış sütunları NULL olarak ayarlanır. Çıktı, bir iç birleştirme ve sol akıştan eşleşmeyen satırlarla döndürülen satırlar olacaktır.

> [!NOTE]
> Veri akışları tarafından kullanılan Spark motoru, birleştirme koşullarınızdaki olası kartezyen ürünler nedeniyle zaman zaman başarısız olur. Bu durumda, özel bir çapraz join'e geçebilir ve birleştirme koşulunuzu el ile girebilirsiniz. Yürütme altyapısı ilişkinin her iki tarafındaki tüm satırları hesaplamak ve sonra satırları filtrelemek gerekebilir gibi bu veri akışları daha yavaş performansa neden olabilir.

### <a name="right-outer"></a>Sağ Dış

Sağ dış birleştirme, sağ akıştaki tüm satırları döndürür ve sol akıştaki kayıtları eşleştirir. Sağ akıştaki bir satır eşleşme yoksa, sol akıştaki çıkış sütunları NULL olarak ayarlanır. Çıktı, bir iç birleştirme ve sağ akıştan eşleşmeyen satırlarla döndürülen satırlar olacaktır.

### <a name="full-outer"></a>Tam Dış

Tam dış birleştirme, eşleşmeyan sütunlar için NULL değerleriyle her iki taraftan tüm sütun ve satırları çıkar.

### <a name="custom-cross-join"></a>Özel çapraz birleştirme

Çapraz birleştirme, bir koşula dayalı olarak iki akışın çapraz ürün çıktıları. Eşitlik olmayan bir koşul kullanıyorsanız, çapraz birleştirme koşulunuz olarak özel bir ifade belirtin. Çıktı akışı, birleştirme koşulunu karşılayan tüm satırlar olacaktır.

Bu birleştirme türünü, equi olmayan birleştirmeler ```OR``` ve koşullar için kullanabilirsiniz.

Tam kartezyen bir ürün açıkça üretmek istiyorsanız, birleştirmeden önce iki bağımsız akıştan önce Türemiş Sütun dönüşümünün üzerinde eşleşecek sentetik bir anahtar oluşturmak için kullanın. Örneğin, türemiş sütunda adı verilen ```SyntheticKey``` her akışta ```1```yeni bir sütun oluşturun ve ''ye eşit olarak ayarlayın. Daha ```a.SyntheticKey == b.SyntheticKey``` sonra özel birleştirme ifadesiniz olarak kullanın.

> [!NOTE]
> Özel çapraz birleştirme de sol ve sağ ilişkinizin her iki yanından en az bir sütun eklediğinden emin olun. Çapraz birleştirmeleri her iki taraftan sütunlar yerine statik değerlerle yürütmek, tüm veri kümesinin tam taranmasıile sonuçlanır ve veri akışınız kötü performans gösterir.

## <a name="configuration"></a>Yapılandırma

1. **Sağ akış** açılır düşüşünde hangi veri akışına katılacağınızı seçin.
1. Birleştirme **türünü** seçin
1. Birleştirme koşulu için hangi anahtar sütunları eşleştirmek istediğinizi seçin. Varsayılan olarak, veri akışı her akışta bir sütun arasında eşitlik arar. Hesaplanmış bir değer üzerinden karşılaştırmayapmak için sütun açılır sütunun üzerine gidin ve **Hesaplanan sütunu**seçin.

![Dönüşüme Katılın](media/data-flow/join.png "Birleştir")

## <a name="optimizing-join-performance"></a>Katılım performansını optimize etme

SSIS gibi araçlarda birleştirme işleminin aksine, birleştirme dönüşümü zorunlu bir birleştirme işlemi değildir. Birleştirme anahtarları sıralama gerektirmez. Birleştirme işlemi, Spark'taki en iyi birleştirme işlemine göre gerçekleşir, yayın veya harita tarafı birleştirme.

![Dönüşüm ekime katılma](media/data-flow/joinoptimize.png "Optimizasyon'a katılın")

Veri akışlarından biri veya her ikisi alt düğüm belleğine uyuyorsa, en iyi duruma getirme sekmesinde **Yayın'ı** etkinleştirerek performansınızı daha da optimize edin. Ayrıca, iş çalışan başına belleğe daha iyi sığabilmesi için verilerinizi birleştirme işleminde yeniden bölümleyebilirsiniz.

## <a name="self-join"></a>Kendi Kendine Birleşin

Bir veri akışını kendisiyle birleştirmek için, belirli bir dönüşüme sahip varolan bir akış takma adı. Dönüşümün yanındaki artı simgesine tıklayarak ve **Yeni şube**seçerek yeni bir dal oluşturun. Özgün akış adatakma adıyla bir seçim dönüşümü ekleyin. Birleştirme dönüşümü ekleyin ve **sol akış** olarak özgün akışı ve **Sağ akış**olarak seç dönüşüm seçin.

![Kendi kendine birleştirme](media/data-flow/selfjoin.png "Kendi kendine birleştirme")

## <a name="testing-join-conditions"></a>Test birleştirme koşulları

Hata ayıklama modunda veri önizlemesi ile birleştirme dönüşümlerini sınarken, bilinen küçük bir veri kümesi kullanın. Büyük bir veri kümesinden satırları örnekleme yaparken, sınama için hangi satırların ve anahtarların okunacağını tahmin edemezsiniz. Sonuç belirleyici değildir, yani birleştirme koşullarınız eşleşmeleri döndürmeyebilir.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<leftStream>, <rightStream>
    join(
        <conditionalExpression>,
        joinType: { 'inner'> | 'outer' | 'left_outer' | 'right_outer' | 'cross' }
        broadcast: { 'none' | 'left' | 'right' | 'both' }
    ) ~> <joinTransformationName>
```

### <a name="inner-join-example"></a>İç birleştirme örneği

Aşağıdaki örnek, sol akışı `JoinMatchedData` ve sağ `TripData` akışı `TripFare`alan bir birleştirme dönüşümüdür.  Birleştirme `hack_license == { hack_license} && TripData@medallion == TripFare@medallion && vendor_id == { vendor_id} && pickup_datetime == { pickup_datetime}` `hack_license`koşulu, her akıştaki , `medallion`, `vendor_id`ve `pickup_datetime` sütunlar eşleşiyorsa doğru döndüren ifadedir. `joinType` Olan. `'inner'` Yalnızca sol akışta yayını etkinleştiriyoruz, `broadcast` bu `'left'`yüzden değeri var.

Veri Fabrikası UX,bu dönüşüm aşağıdaki resim gibi görünür:

![Örnekle katılın](media/data-flow/join-script1.png "Örnekle katılın")

Bu dönüşüm için veri akışı komut dosyası aşağıdaki snippet bulunmaktadır:

```
TripData, TripFare
    join(
        hack_license == { hack_license}
        && TripData@medallion == TripFare@medallion
        && vendor_id == { vendor_id}
        && pickup_datetime == { pickup_datetime},
        joinType:'inner',
        broadcast: 'left'
    )~> JoinMatchedData
```

### <a name="custom-cross-join-example"></a>Özel çapraz birleştirme örneği

Aşağıdaki örnek, sol akışı `JoiningColumns` ve sağ `LeftStream` akışı `RightStream`alan bir birleştirme dönüşümüdür. Bu dönüştürme iki akış alır ve sütun sütundan `leftstreamcolumn` `rightstreamcolumn`daha büyük olan tüm satırları birleştirir. `joinType` Olan. `cross` Yayın etkin `broadcast` değil `'none'`değeri vardır.

Veri Fabrikası UX,bu dönüşüm aşağıdaki resim gibi görünür:

![Örnekle katılın](media/data-flow/join-script2.png "Örnekle katılın")

Bu dönüşüm için veri akışı komut dosyası aşağıdaki snippet bulunmaktadır:

```
LeftStream, RightStream
    join(
        leftstreamcolumn > rightstreamcolumn,
        joinType:'cross',
        broadcast: 'none'
    )~> JoiningColumns
```

## <a name="next-steps"></a>Sonraki adımlar

Verileri birleştirdikten sonra, [türetilmiş](data-flow-derived-column.md) bir sütun oluşturun ve verilerinizi bir hedef veri deposuna [batırın.](data-flow-sink.md)
