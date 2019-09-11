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
ms.openlocfilehash: 245a69f5e5834e68bbbd17a96859a93bc16eacbe
ms.sourcegitcommit: 86d49daccdab383331fc4072b2b761876b73510e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70744191"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemede veri modelleme

Bu belge, Azure Time Series Insights önizlemesini izleyen zaman serisi modelleriyle nasıl çalışılacağını açıklar. Yaygın olarak karşılaşılan çeşitli veri senaryolarına ilişkin ayrıntılar.

Güncelleştirmeyi kullanma hakkında daha fazla bilgi edinmek için [Azure Time Series Insights önizleme Gezgini](./time-series-insights-update-explorer.md)' ni okuyun.

## <a name="types"></a>Türler

### <a name="create-a-single-type"></a>Tek bir tür oluşturma

1. Zaman serisi modelleri Seçicisi paneline gidin ve menüden **türler** ' i seçin. Zaman serisi modelleri türlerine odaklanmak için paneli daraltın.

    [![Tek bir tür oluşturma](media/v2-update-how-to-tsm/portal-one.png)](media/v2-update-how-to-tsm/portal-one.png#lightbox)

1. **+ Ekle** öğesini seçin.
1. Türlerle ilgili tüm ayrıntıları girin ve **Oluştur**' u seçin. Bu eylem, ortamda türler oluşturur.

    [![Tür ekleme](media/v2-update-how-to-tsm/portal-two.png)](media/v2-update-how-to-tsm/portal-two.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Bir veya daha fazla türü toplu karşıya yükleme

1. **JSON karşıya yükle**' yi seçin.
1. Tür yükünü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    [![JSON yükleme](media/v2-update-how-to-tsm/portal-three.png)](media/v2-update-how-to-tsm/portal-three.png#lightbox)

### <a name="edit-a-single-type"></a>Tek bir türü düzenleme

1. Türü seçin ve **Düzenle**' yi seçin. 
1. Gerekli değişiklikleri yapın ve **Kaydet**' i seçin.

    [![Bir tür düzenleme](media/v2-update-how-to-tsm/portal-four.png)](media/v2-update-how-to-tsm/portal-four.png#lightbox)

### <a name="delete-a-type"></a>Bir tür silme

1. Türü seçin ve **Sil**' i seçin.
1. Türlerle ilişkili bir örnek yoksa, silinir.

    [![Bir tür silme](media/v2-update-how-to-tsm/portal-five.png)](media/v2-update-how-to-tsm/portal-five.png#lightbox)

## <a name="hierarchies"></a>Hiyerarşiler

### <a name="create-a-single-hierarchy"></a>Tek bir hiyerarşi oluşturma

1. Zaman serisi modelleri Seçicisi paneline gidin ve menüden **hiyerarşiler** ' ı seçin. Zaman serisi modelleri hiyerarşilerine odaklanmak için paneli daraltın.

    [![Hiyerarşileri Seç](media/v2-update-how-to-tsm/portal-six.png)](media/v2-update-how-to-tsm/portal-six.png#lightbox)

1. **+ Ekle** öğesini seçin.

    [![Hiyerarşi ekleme](media/v2-update-how-to-tsm/portal-seven.png)](media/v2-update-how-to-tsm/portal-seven.png#lightbox)

1. Sağ bölmede **+ Ekle düzeyi** ' ni seçin.

    [![Düzey Ekle](media/v2-update-how-to-tsm/portal-eight.png)](media/v2-update-how-to-tsm/portal-eight.png#lightbox)

1. Hiyerarşi ayrıntılarını girip **Oluştur**' u seçin.

    [![Düzey oluştur](media/v2-update-how-to-tsm/portal-nine.png)](media/v2-update-how-to-tsm/portal-nine.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Bir veya daha fazla hiyerarşiyi toplu karşıya yükleme

1. **JSON karşıya yükle**' yi seçin.
1. Hiyerarşi yükünü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    [![Hiyerarşileri toplu karşıya yükleme](media/v2-update-how-to-tsm/portal-ten.png)](media/v2-update-how-to-tsm/portal-ten.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Tek bir hiyerarşiyi düzenleme

1. Hiyerarşiyi seçin ve **Düzenle**' yi seçin.
1. Gerekli değişiklikleri yapın ve **Kaydet**' i seçin.

    [![Tek bir hiyerarşiyi düzenleme](media/v2-update-how-to-tsm/portal-eleven.png)](media/v2-update-how-to-tsm/portal-eleven.png#lightbox)

### <a name="delete-a-hierarchy"></a>Hiyerarşiyi silme

1. Hiyerarşiyi seçin ve **Sil**' i seçin. 
1. Hiyerarşiyle ilişkili örnek yoksa, silinir.

    [![Hiyerarşiyi silme](media/v2-update-how-to-tsm/portal-twelve.png)](media/v2-update-how-to-tsm/portal-twelve.png#lightbox)

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

    [![Bir veya daha fazla örneği toplu karşıya yükleme](media/v2-update-how-to-tsm/portal-fifteen.png)](media/v2-update-how-to-tsm/portal-fifteen.png#lightbox)

1. **Karşıya Yükle**’yi seçin.

### <a name="edit-a-single-instance"></a>Tek bir örneği düzenleme

1. Örneği seçin ve **Düzenle**' yi seçin. 
1. Gerekli değişiklikleri yapın ve **Kaydet**' i seçin.

    [![Tek bir örneği düzenleme](media/v2-update-how-to-tsm/portal-sixteen.png)](media/v2-update-how-to-tsm/portal-sixteen.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- Zaman serisi modelleri hakkında daha fazla bilgi için [veri modellemesini](./time-series-insights-update-tsm.md)okuyun.

- Önizleme hakkında daha fazla bilgi edinmek için [Azure Time Series Insights önizleme Gezgini ' nde verileri görselleştir](./time-series-insights-update-explorer.md)' i okuyun.

- Desteklenen JSON şekilleri hakkında bilgi edinmek için [desteklenen JSON şekillerini](./time-series-insights-send-events.md#json)okuyun.
