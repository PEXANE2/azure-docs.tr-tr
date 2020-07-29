---
title: Azure Izleyici çalışma kitapları zaman parametreleri
description: Önceden oluşturulmuş ve özel parametreli çalışma kitapları ile karmaşık raporlamayı kolaylaştırın
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 380b8a7ce286ab06b6935bf63bf3a0e82f371c2f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77658022"
---
# <a name="workbook-time-parameters"></a>Çalışma kitabı zaman parametreleri

Zaman parametreleri kullanıcıların analiz zaman bağlamını ayarlamanıza ve neredeyse tüm raporlar tarafından kullanılmasına izin verir. Kurulum ve kullanım açısından, yazarların özel zaman aralıkları seçeneği dahil olmak üzere açılan kutuda gösterilecek zaman aralıklarını belirtmesini sağlamak oldukça kolaydır. 

## <a name="creating-a-time-parameter"></a>Time parametresi oluşturma
1. Düzenleme modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabı içindeki bağlantılardan _parametre Ekle_ ' yi seçin.
3. Mavi _parametre Ekle_ düğmesine tıklayın.
4. Açılır yeni parametre bölmesinde şunu girin:
    1. Parametre adı:`TimeRange`
    2. Parametre türü:`Time range picker`
    3. Gerekli:`checked`
    4. Kullanılabilir zaman aralıkları: son saat, son 12 saat, son 24 saat, son 48 saat, son 3 gün, son 7 gün ve özel zaman aralığı seçimine Izin ver
5. Parametresini oluşturmak için araç çubuğundan ' Kaydet ' seçeneğini belirleyin.

    ![Zaman aralığı parametresinin oluşturulmasını gösteren resim](./media/workbooks-time/time-settings.png)

Çalışma kitabının okuma modunda nasıl görüneceğine bu şekilde bakabilirsiniz.

![Okuma modundaki bir zaman aralığı parametresini gösteren resim](./media/workbooks-time/parameters-time.png)

## <a name="referencing-a-time-parameter"></a>Bir zaman parametresine başvurma
### <a name="via-bindings"></a>Bağlamalar aracılığıyla
1. Çalışma kitabına bir sorgu denetimi ekleyin ve bir Application Insights kaynağı seçin.
2. Çoğu çalışma kitabı denetimi _zaman aralığı_ kapsam seçiciyi destekler. _Zaman aralığı_ açılan öğesini açın ve `{TimeRange}` alt kısımdaki saat ettirmeye parametreleri grubunda öğesini seçin.
3. Bu, zaman aralığı parametresini grafiğin zaman aralığına bağlar. Örnek sorgunun zaman kapsamı son 24 saattir.
4. Sonuçları görmek için sorguyu çalıştırın

    ![Bağlamalar aracılığıyla başvurulan zaman aralığı parametresini gösteren resim](./media/workbooks-time/time-binding.png)

### <a name="in-kql"></a>KQL 'de
1. Çalışma kitabına bir sorgu denetimi ekleyin ve bir Application Insights kaynağı seçin.
2. KQL 'de, parametresini kullanarak bir zaman kapsamı filtresi girin:`| where timestamp {TimeRange}`
3. Bu, `| where timestamp > ago(1d)` parametresinin zaman aralığı değeri olan sorgu değerlendirmesi süresini genişletir.
4. Sonuçları görmek için sorguyu çalıştırın

    ![KQL 'de başvurulan zaman aralığını gösteren resim](./media/workbooks-time/time-in-code.png)

### <a name="in-text"></a>Metinde 
1. Çalışma kitabına bir metin denetimi ekleyin.
2. Markaşağı içinde şunu girin`The chosen time range is {TimeRange:label}`
3. _Düzenle bitti_ 'yi seçin
4. Metin denetimi metin gösterecektir: _Seçilen zaman aralığı son 24 saat_

## <a name="time-parameter-options"></a>Zaman parametresi seçenekleri
| Parametre | Açıklama | Örnek |
| ------------- |:-------------|:-------------|
| `{TimeRange}` | Zaman aralığı etiketi | Son 24 saat |
| `{TimeRange:label}` | Zaman aralığı etiketi | Son 24 saat |
| `{TimeRange:value}` | Zaman aralığı değeri | > önce (1D) |
| `{TimeRange:query}` | Zaman aralığı sorgusu | > önce (1D) |
| `{TimeRange:start}` | Zaman aralığı başlangıç zamanı | 3/20/2019 4:18 PM |
| `{TimeRange:end}` | Zaman aralığı bitiş saati | 3/21/2019 4:18 PM |
| `{TimeRange:grain}` | Zaman aralığı gren | 30 milyon |


### <a name="using-parameter-options-in-a-query"></a>Sorguda parametre seçeneklerini kullanma
```kusto
requests
| make-series Requests = count() default = 0 on timestamp from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain}
```

## <a name="next-steps"></a>Sonraki adımlar

* Çok sayıda zengin görselleştirmeler seçeneği hakkında daha fazla [bilgi edinmeye başlayın](workbooks-visualizations.md) .
* Çalışma kitabı kaynaklarınıza erişimi [denetleme](workbooks-access-control.md) ve paylaşma.
