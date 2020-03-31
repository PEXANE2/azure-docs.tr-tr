---
title: Azure Monitör çalışma kitapları zaman parametreleri
description: Önceden oluşturulmuş ve özel parametreli çalışma kitaplarıyla karmaşık raporlamayı basitleştirin
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 380b8a7ce286ab06b6935bf63bf3a0e82f371c2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658022"
---
# <a name="workbook-time-parameters"></a>Çalışma kitabı zaman parametreleri

Zaman parametreleri, kullanıcıların analizin zaman bağlamını ayarlamasına olanak sağlar ve hemen hemen tüm raporlar tarafından kullanılır. Kurulum ve kullanımı nispeten basittir - yazarların özel zaman aralıkları seçeneği de dahil olmak üzere açılır zaman aralıklarında gösterilebilmeleri için zaman aralıklarını belirtmelerine olanak sağlar. 

## <a name="creating-a-time-parameter"></a>Zaman parametresi oluşturma
1. Edit modunda boş bir çalışma kitabıyla başlayın.
2. Çalışma kitabındaki bağlantılardan _parametre ekle'yi_ seçin.
3. Mavi _Parametre Ekle_ düğmesine tıklayın.
4. Açılan yeni parametre bölmesine girin:
    1. Parametre adı:`TimeRange`
    2. Parametre türü:`Time range picker`
    3. Gerekli:`checked`
    4. Kullanılabilir zaman aralıkları: Son saat, Son 12 saat, Son 24 saat, Son 48 saat, Son 3 gün, Son 7 gün ve Özel zaman aralığı seçimine izin verin
5. Parametreyi oluşturmak için araç çubuğundan 'Kaydet'i seçin.

    ![Zaman aralığı parametresi oluşturmayı gösteren görüntü](./media/workbooks-time/time-settings.png)

Çalışma kitabı okuma modunda böyle görünecektir.

![Okuma modunda bir zaman aralığı parametresi gösteren görüntü](./media/workbooks-time/parameters-time.png)

## <a name="referencing-a-time-parameter"></a>Zaman parametresi gönderme
### <a name="via-bindings"></a>Bağlama yoluyla
1. Çalışma kitabına bir sorgu denetimi ekleyin ve bir Uygulama Öngörüleri kaynağını seçin.
2. Çoğu çalışma kitabı denetimi _Bir Zaman Aralığı_ kapsam seçiciyi destekler. Zaman _Aralığı_ açılır açılır açın `{TimeRange}` ve alt kısmında zaman rang parametreleri grubunu seçin.
3. Bu, zaman aralığı parametresini grafiğin zaman aralığına bağlar. Örnek sorgunun zaman kapsamı artık Son 24 saattir.
4. Sonuçları görmek için sorguyu çalıştır

    ![Bağlamalar yoluyla başvurulan zaman aralığı parametresi gösteren resim](./media/workbooks-time/time-binding.png)

### <a name="in-kql"></a>KQL içinde
1. Çalışma kitabına bir sorgu denetimi ekleyin ve bir Uygulama Öngörüleri kaynağını seçin.
2. KQL'de, parametreyi kullanarak bir zaman kapsamı filtresi girin:`| where timestamp {TimeRange}`
3. Bu, sorgu değerlendirme süresini `| where timestamp > ago(1d)`parametrenin zaman aralığı değeri olana genişletir.
4. Sonuçları görmek için sorguyu çalıştır

    ![KQL'de başvurulan bir zaman aralığını gösteren görüntü](./media/workbooks-time/time-in-code.png)

### <a name="in-text"></a>Metin olarak 
1. Çalışma kitabına metin denetimi ekleyin.
2. İşaretle, girin`The chosen time range is {TimeRange:label}`
3. _Bitmiş Düzenleme'yi_ Seçin
4. Metin denetimi metni gösterir: _Seçilen zaman aralığı Son 24 saattir_

## <a name="time-parameter-options"></a>Zaman parametresi seçenekleri
| Parametre | Açıklama | Örnek |
| ------------- |:-------------|:-------------|
| `{TimeRange}` | Zaman aralığı etiketi | Son 24 saat |
| `{TimeRange:label}` | Zaman aralığı etiketi | Son 24 saat |
| `{TimeRange:value}` | Zaman aralığı değeri | > önce(1d) |
| `{TimeRange:query}` | Zaman aralığı sorgusu | > önce(1d) |
| `{TimeRange:start}` | Zaman aralığı başlangıç saati | 20.03.2019 16:18 |
| `{TimeRange:end}` | Zaman aralığı bitiş saati | 21.03.2019 16:18 |
| `{TimeRange:grain}` | Zaman aralığı tahıl | 30 m |


### <a name="using-parameter-options-in-a-query"></a>Sorguda parametre seçeneklerini kullanma
```kusto
requests
| make-series Requests = count() default = 0 on timestamp from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain}
```

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitapları hakkında daha fazla bilgi [edinmeye başlayın](workbooks-visualizations.md) birçok zengin görselleştirme seçeneği.
* Çalışma kitabı kaynaklarınıza erişimi [kontrol](workbooks-access-control.md) edin ve paylaşın.
