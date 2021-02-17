---
title: Azure izleyici çalışma kitabı Honey çağrısıyla görselleştirmeleri
description: Azure izleyici çalışma kitabı Honey çağrısıyla görselleştirmeleri hakkında bilgi edinin.
services: azure-monitor
author: lgayhardt
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 09/18/2020
ms.author: lagayhar
ms.openlocfilehash: 0c657d84144ee7bd69a6de1d5d2e1e769dc59f4d
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100621523"
---
# <a name="honey-comb-visualizations"></a>Honey çağrısıyla görselleştirmeleri

Honey Comb, isteğe bağlı olarak kümeler halinde gruplanılabilen ölçüm veya kategorilerin yüksek yoğunluklu görünümlerine izin verir. Etkin noktaları görsel olarak tanımlamak ve daha fazla ayrıntıya gitme yararlı olur.

Aşağıdaki görüntüde, sanal makinelerin iki abonelik üzerinde CPU kullanımı gösterilmektedir. Her hücre bir sanal makineyi temsil eder ve renk/etiket, ortalama CPU kullanımını temsil eder (Reds, etkin makinelerdir). Sanal makineler aboneliğe göre kümelenir.

[![Ekran görüntüsü, sanal makinelerin iki abonelik üzerinde CPU kullanımını gösterir](.\media\workbooks-honey-comb\cpu-example.png)](.\media\workbooks-honey-comb\cpu-example.png#lightbox)

## <a name="adding-a-honey-comb"></a>Honey çağrısıyla ekleme

1. Düzenleme araç çubuğu öğesine tıklayarak çalışma kitabını düzenleme moduna geçirin.
2. Çalışma kitabına bir günlük sorgu denetimi eklemek için alt kısımdaki **Ekle**  ' yi ve ardından *Sorgu Ekle* ' yi kullanın.
3. *Kaynak türü* olarak "Log Analytics" ve *kaynak* noktası için sanal makine performans günlüğü olan bir çalışma alanına "günlükleri *" ni seçin*.
4. Analizinize yönelik KQL 'yi girmek için sorgu düzenleyicisini kullanın.

```kusto
    Perf
| where CounterName == 'Available MBytes'
| summarize CounterValue = avg(CounterValue) by Computer, _ResourceId
| extend ResourceGroup = extract(@'/subscriptions/.+/resourcegroups/(.+)/providers/microsoft.compute/virtualmachines/.+', 1, _ResourceId)
| extend ResourceGroup = iff(ResourceGroup == '', 'On-premise computers', ResourceGroup), Id = strcat(_ResourceId, '::', Computer)
```

5. Sorguyu çalıştırın.
6. *Görselleştirmeyi* "Graf" olarak ayarlayın.
7. **Grafik ayarları**' nı seçin.
    1. Alttaki *Düzen alanlarında* , şunu ayarlayın:
        1. Grafik türü: **Hive kümeleri**.
        2. Düğüm kimliği: `Id` .
        3. Gruplandırma ölçütü: `None` .
        4. Düğüm boyutu: 100.
        5. Altıgenler arasındaki kenar boşluğu: `5` .
        6. Renklendirme türü: **heatmap**.
        7. Düğüm renk alanı: `CouterValue` .
        8. Renk paleti: `Red to Green` .
        9. En küçük değer: `100` .
        10. En büyük değer: `2000` .
    2. En üstteki *düğüm biçimi ayarları* ' nda, şunu ayarlayın:
        1. En üstteki Içerik:
            1. Sütunu kullan: `Computer` .
            2. Sütun Oluşturucusu " `Text` .
        9. Içeriği Ortala:
            1. Sütunu kullan: `CounterValue` .
            2. Sütun Oluşturucu: `Big Number` .
            3. Renk paleti: `None` .
            4. *Özel sayı biçimlendirme* kutusunu işaretleyin.
            5. Birimler: `Megabytes` .
            6. En fazla kesirli basamaklar: `1` .
8. Bölmenin alt kısmındaki **Kaydet ve Kapat** düğmesini seçin.

[![Yukarıdaki sorgu ve ayarlarla sorgu denetimi, grafik ayarları ve Honey çağrısıyla ekran görüntüsü](.\media\workbooks-honey-comb\available-memory.png)](.\media\workbooks-honey-comb\available-memory.png#lightbox)

## <a name="honey-comb-layout-settings"></a>Honey çağrısıyla Düzen ayarları

| Ayar | Açıklama |
|:------------- |:-------------|
| `Node Id` | Düğümlerin benzersiz KIMLIĞINI sağlayan bir sütun seçer. Sütunun değeri dize veya sayı olabilir. |
| `Group By Field` | Düğümlerin kümelemek için sütunu seçin. |
| `Node Size` | Altıonal hücrelerinin boyutunu ayarlar. `Margin between hexagons`Honey çağrısıyla grafiğinin görünümünü özelleştirmek için özelliğini kullanın. |
| `Margin between hexagons` | Altıonal hücreleri arasındaki boşluğu ayarlar. `Node size`Honey çağrısıyla grafiğinin görünümünü özelleştirmek için özelliğini kullanın. |
| `Coloring type` | Düğümü renklendirmek için kullanılacak düzeni seçer. |
| `Node Color Field` | Düğüm alanlarının temel alınacağı ölçümü sağlayan bir sütun seçer. |

## <a name="node-coloring-types"></a>Düğüm renklendirme türleri

| Renklendirme türü | Açıklama |
|:------------- |:-------------|
| `None` | Tüm düğümler aynı renge sahip. |
| `Categorical` | Düğümlere, sonuç kümesindeki bir sütundan değere veya kategoriye göre renkler atanır. Yukarıdaki örnekte, renklendirme sonuç kümesinin sütun _türünü_ temel alır. Desteklenen paletler `Default` , `Pastel` ve `Cool tone` .  |
| `Heatmap` | Bu türde, hücreler ölçüm sütununu ve renk paletini temel alarak renklendirilir. Bu, hücrelerdeki ölçüm formaları vurgulamak için basit bir yol sağlar. |
| `Thresholds` | Bu türde, hücre renkleri eşik kurallarına göre ayarlanır (örneğin, _CPU > 90% => Red, 60% > cpu > 90% => sarı, cpu < 60% => yeşil_) |
| `Field Based` | Bu türde, bir sütun, düğüm için kullanılmak üzere belirli bir RGB değeri sağlar. En çok esnekliği sağlar, ancak genellikle daha fazla iş gerektirir.  |
      
## <a name="node-format-settings"></a>Düğüm biçimi ayarları

Honey çağrısıyla yazarları, bir düğümün farklı bölümlerine hangi içeriğin gidebileceklerini belirtebilir: üst, sol, orta, sağ ve alt. Yazarlar, hiçbir işleyicilere çalışma kitaplarından birini destekler (metin, büyük sayı, Spark çizgileri, simge, vb.).

## <a name="next-steps"></a>Sonraki adımlar

- [Çalışma kitaplarında Birleşik çubuk Oluşturucu](workbooks-composite-bar.md)oluşturmayı öğrenin.
- [Azure izleyici günlük verilerini Power BI 'ye aktarmayı](../platform/powerbi.md)öğrenin.
