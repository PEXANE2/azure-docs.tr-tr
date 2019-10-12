---
title: Azure Time Series Insights Preview 'da veri modelleme | Microsoft Docs
description: Azure Time Series Insights önizlemede veri modellemesini anlayın.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 09/04/2019
ms.custom: seodec18
ms.openlocfilehash: 64f892f344780d4af58c70935b9b7b68bad9550d
ms.sourcegitcommit: f272ba8ecdbc126d22a596863d49e55bc7b22d37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72273748"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemede veri modelleme

Bu belge, Azure Time Series Insights önizlemesini izleyen zaman serisi modelleriyle nasıl çalışılacağını açıklar. Yaygın olarak karşılaşılan çeşitli veri senaryolarına ilişkin ayrıntılar.

Güncelleştirmeyi kullanma hakkında daha fazla bilgi edinmek için [Azure Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md)' ni okuyun.

## <a name="types"></a>Türü

### <a name="create-a-single-type"></a>Tek bir tür oluşturma

1. Zaman serisi modelleri Seçicisi paneline gidin ve menüden **türler** ' i seçin. Zaman serisi modelleri türlerine odaklanmak için paneli daraltın.

    [![Tek tür oluşturma](media/v2-update-how-to-tsm/portal-one.png)](media/v2-update-how-to-tsm/portal-one.png#lightbox)

1. **+ Ekle** öğesini seçin.
1. Türlerle ilgili tüm ayrıntıları girin ve **Oluştur**' u seçin. Bu eylem, ortamda türler oluşturur.

    [![Tür ekleme](media/v2-update-how-to-tsm/portal-two.png)](media/v2-update-how-to-tsm/portal-two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Bir veya daha fazla türü toplu karşıya yükleme

1. **JSON karşıya yükle**' yi seçin.
1. Tür yükünü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    [![Json yükleme](media/v2-update-how-to-tsm/portal-three.png)](media/v2-update-how-to-tsm/portal-three.png#lightbox)

### <a name="edit-a-single-type"></a>Tek bir türü düzenleme

1. Türü seçin ve **Düzenle**' yi seçin. 
1. Gerekli değişiklikleri yapın ve **Kaydet**' i seçin.

    [ bir tür düzenleme](media/v2-update-how-to-tsm/portal-four.png)](media/v2-update-how-to-tsm/portal-four.png#lightbox)

### <a name="delete-a-type"></a>Bir tür silme

1. Türü seçin ve **Sil**' i seçin.
1. Türlerle ilişkili bir örnek yoksa, silinir.

    [![Tür silme](media/v2-update-how-to-tsm/portal-five.png)](media/v2-update-how-to-tsm/portal-five.png#lightbox)

## <a name="hierarchies"></a>Hiyerarşileri

### <a name="create-a-single-hierarchy"></a>Tek bir hiyerarşi oluşturma

1. Zaman serisi modelleri Seçicisi paneline gidin ve menüden **hiyerarşiler** ' ı seçin. Zaman serisi modelleri hiyerarşilerine odaklanmak için paneli daraltın.

    [![ hiyerarşi seçin](media/v2-update-how-to-tsm/portal-six.png)](media/v2-update-how-to-tsm/portal-six.png#lightbox)

1. **+ Ekle** öğesini seçin.

    [![ hiyerarşi ekleme](media/v2-update-how-to-tsm/portal-seven.png)](media/v2-update-how-to-tsm/portal-seven.png#lightbox)

1. Sağ bölmede **+ Ekle düzeyi** ' ni seçin.

    [![ düzey Ekle](media/v2-update-how-to-tsm/portal-eight.png)](media/v2-update-how-to-tsm/portal-eight.png#lightbox)

1. Hiyerarşi ayrıntılarını girip **Oluştur**' u seçin.

    [![ düzey oluşturma](media/v2-update-how-to-tsm/portal-nine.png)](media/v2-update-how-to-tsm/portal-nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Bir veya daha fazla hiyerarşiyi toplu karşıya yükleme

1. **JSON karşıya yükle**' yi seçin.
1. Hiyerarşi yükünü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    [![ toplu karşıya yükleme hiyerarşileri](media/v2-update-how-to-tsm/portal-ten.png)](media/v2-update-how-to-tsm/portal-ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Tek bir hiyerarşiyi düzenleme

1. Hiyerarşiyi seçin ve **Düzenle**' yi seçin.
1. Gerekli değişiklikleri yapın ve **Kaydet**' i seçin.

    [![ tek hiyerarşiyi düzenleme](media/v2-update-how-to-tsm/portal-eleven.png)](media/v2-update-how-to-tsm/portal-eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Hiyerarşiyi silme

1. Hiyerarşiyi seçin ve **Sil**' i seçin. 
1. Hiyerarşiyle ilişkili örnek yoksa, silinir.

    [![ hiyerarşiyi silme](media/v2-update-how-to-tsm/portal-twelve.png)](media/v2-update-how-to-tsm/portal-twelve.png#lightbox)

## <a name="instances"></a>Örnekler

### <a name="create-a-single-instance"></a>Tek bir örnek oluşturma

1. Zaman serisi modelleri Seçicisi paneline gidin ve menüden **örnekler** ' i seçin. Zaman serisi modelleri örneklerine odaklanmak için paneli daraltın.

    [![Tek bir örnek oluşturma](media/v2-update-how-to-tsm/portal-thirteen.png)](media/v2-update-how-to-tsm/portal-thirteen.png#lightbox)

1. **Add (Ekle)** seçeneğini belirleyin.

    [![Örnek ekleme](media/v2-update-how-to-tsm/portal-fourteen.png)](media/v2-update-how-to-tsm/portal-fourteen.png#lightbox)

1. Örnek ayrıntılarını girin, tür ve hiyerarşi ilişkilendirmesini seçin ve **Oluştur**' u seçin.

### <a name="bulk-upload-one-or-more-instances"></a>Bir veya daha fazla örneği toplu karşıya yükleme

1. **JSON karşıya yükle**' yi seçin.
1. Örnekler yükünü içeren dosyayı seçin.

    [![ bir veya daha fazla örnek yükleme](media/v2-update-how-to-tsm/portal-fifteen.png)](media/v2-update-how-to-tsm/portal-fifteen.png#lightbox)

1. **Karşıya Yükle**’yi seçin.

### <a name="edit-a-single-instance"></a>Tek bir örneği düzenleme

1. Örneği seçin ve **Düzenle**' yi seçin. 
1. Gerekli değişiklikleri yapın ve **Kaydet**' i seçin.

    [![Tek bir örneği düzenleme](media/v2-update-how-to-tsm/portal-sixteen.png)](media/v2-update-how-to-tsm/portal-sixteen.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- Zaman serisi modelleri hakkında daha fazla bilgi için [veri modellemesini](./time-series-insights-update-tsm.md)okuyun.

- Önizleme hakkında daha fazla bilgi edinmek için [Azure Time Series Insights önizleme Gezgini ' nde verileri görselleştir](./time-series-insights-update-explorer.md)' i okuyun.

- Desteklenen JSON şekilleri hakkında bilgi edinmek için [desteklenen JSON şekillerini](./time-series-insights-send-events.md#supported-json-shapes)okuyun.
