---
title: Önizleme ortamlarında veri modelleme - Azure Time Series Insights | Microsoft Dokümanlar
description: Azure Time Series Öngörüleri Önizleme'de veri modelleme hakkında bilgi edinin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/18/2020
ms.custom: seodec18
ms.openlocfilehash: 39ebbf99ad31cce20eabc20fbdc056c889235713
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77470760"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Azure Time Series Öngörüleri Önizlemesinde veri modelleme

Bu makalede, Azure Zaman Serisi Öngörüleri Önizlemesinde Zaman Serisi Modeli ile nasıl çalışılalı ş. Birkaç yaygın veri senaryosunun ayrıntılarını eder.

> [!TIP]
> * Önizleme Zaman [Serisi Modeli](time-series-insights-update-tsm.md)hakkında bilgi edinin.
> * [Azure Zaman Serisi Öngörüleri Önizleme gezgininde](./time-series-insights-update-explorer.md)Önizleme UI'sinde gezinme hakkında daha fazla bilgi edinin.

## <a name="instances"></a>Örnekler

Azure Zaman Serisi Öngörüleri gezgini, tarayıcı içindeki Örnek **OLUŞTURMA**, **OKUMA**, **GÜNCELLEME**ve **DELETE** işlemlerini destekler. 

Başlamak için, Zaman Serisi Öngörüler explorer **Analyze** görünümünden **Model** görünümünü seçin.

### <a name="create-a-single-instance"></a>Tek bir örnek oluşturma

1. Zaman Serisi Model seçici paneline gidin ve menüden **Örnekler'i** seçin. Seçtiğiniz Time Series Insights ortamıyla ilişkili tüm örnekler görüntülenir.

    [![İlk Örnekleri seçerek tek bir örnek oluşturun.](media/v2-update-how-to-tsm/how-to-tsm-instances-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-instances-panel.png#lightbox)

1. **+ Ekle** öğesini seçin.

    [![+ Ekle düğmesini seçerek bir örnek ekleyin.](media/v2-update-how-to-tsm/how-to-tsm-add-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-add-instance.png#lightbox)

1. Örnek ayrıntılarını girin, tür ve hiyerarşi ilişkilendirmesini seçin ve **Oluştur'u**seçin.

### <a name="bulk-upload-one-or-more-instances"></a>Toplu yükleme bir veya daha fazla örnek

> [!TIP]
> JSON'da örneklerinizi masaüstünüze kaydedebilirsiniz. İndirilen JSON dosyası aşağıdaki adımlarla yüklenebilir.

1. **Upload JSON'ı**seçin.
1. Örneklerin yükünü içeren dosyayı seçin.

    [![JSON üzerinden toplu yükleme örnekleri.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-instances.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-instances.png#lightbox)

1. **Yükle'yi**seçin.

### <a name="edit-a-single-instance"></a>Tek bir örneği edin

1. Örneği seçin ve **edit** veya **kalem simgesini**seçin. 
1. Gerekli değişiklikleri yapın ve **Kaydet'i**seçin.

    [![Tek bir örneği edin.](media/v2-update-how-to-tsm/how-to-tsm-edit-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-instance.png#lightbox)

### <a name="delete-an-instance"></a>Bir örneği silme

1. Örneği seçin ve **silme** veya **çöp kutusu simgesini**seçin.

   [![Sil'i seçerek bir örneği silin.](media/v2-update-how-to-tsm/how-to-tsm-delete-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-instance.png#lightbox)

1. Sil'i seçerek silmeişlemini **onaylayın.**

> [!NOTE]
> Bir örnek başarıyla silinecek bir alan doğrulama denetimi geçmesi gerekir.

## <a name="hierarchies"></a>Hiyerarşiler

Azure Zaman Serisi Öngörüleri gezgini, tarayıcı içindeki Hiyerarşi **CREATE,** **READ**, **UPDATE**ve **DELETE** işlemlerini destekler. 

Başlamak için, Zaman Serisi Öngörüler explorer **Analyze** görünümünden **Model** görünümünü seçin.

### <a name="create-a-single-hierarchy"></a>Tek bir hiyerarşi oluşturma

1. Zaman Serisi Model seçici paneline gidin ve menüden **Hiyerarşiler'i** seçin. Seçtiğiniz Time Series Insights ortamıyla ilişkili tüm hiyerarşiler görüntülenir.

    [![Bölmeden bir hiyerarşi oluşturun.](media/v2-update-how-to-tsm/how-to-tsm-hierarchy-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-hierarchy-panel.png#lightbox)

1. **+ Ekle** öğesini seçin.

    [![Hiyerarşi + Ekle düğmesi.](media/v2-update-how-to-tsm/how-to-tsm-add-new-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-add-new-hierarchy.png#lightbox)

1. Seç + Sağ bölmeye **seviye ekle.**

    [![Hiyerarşiye bir düzey ekleyin.](media/v2-update-how-to-tsm/how-to-tsm-save-hierarchy-levels.png)](media/v2-update-how-to-tsm/how-to-tsm-save-hierarchy-levels.png#lightbox)

1. Hiyerarşi ayrıntılarını girin ve **Kaydet'i**seçin.

    [![Hiyerarşi ayrıntılarını belirtin.](media/v2-update-how-to-tsm/how-to-tsm-add-hierarchy-level.png)](media/v2-update-how-to-tsm/how-to-tsm-add-hierarchy-level.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Toplu yükleme bir veya daha fazla hiyerarşi

> [!TIP]
> Hiyerarşilerinizi JSON'da masaüstünüze kaydedebilirsiniz. İndirilen JSON dosyası aşağıdaki adımlarla yüklenebilir.

1. **Upload JSON'ı**seçin.
1. Hiyerarşi yükünü içeren dosyayı seçin.
1. **Yükle'yi**seçin.

    [![Hiyerarşilerin toplu yüklemesi için seçimler.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-hierarchies.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-hierarchies.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Tek bir hiyerarşiyi edin

1. Hiyerarşiyi seçin ve **edit** veya **kalem simgesini**seçin.
1. Gerekli değişiklikleri yapın ve **Kaydet'i**seçin.

    [![Tek bir hiyerarşiyi düzenlemek için seçimler.](media/v2-update-how-to-tsm/how-to-tsm-edit-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-hierarchy.png#lightbox)

### <a name="delete-a-hierarchy"></a>Hiyerarşiyi silme

1. Hiyerarşiyi seçin ve **silme** veya **çöp kutusu simgesini**seçin. 

    [![Sil düğmesini seçerek hiyerarşiyi silin.](media/v2-update-how-to-tsm/how-to-tsm-delete-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-hierarchy.png#lightbox)

1. Sil'i seçerek silmeişlemini **onaylayın.**

## <a name="types"></a>Türler

Azure Zaman Serisi Öngörüleri gezgini tarayıcı içindeki Type **CREATE,** **READ**, **UPDATE**ve **DELETE** işlemlerini destekler. 

Başlamak için, Zaman Serisi Öngörüler explorer **Analyze** görünümünden **Model** görünümünü seçin.

### <a name="create-a-single-type"></a>Tek bir tür oluşturma

1. Zaman Serisi Model seçici paneline gidin ve menüden **Türler'i** seçin. Seçtiğiniz Time Series Insights ortamıyla ilişkili tüm türler görüntülenir.

    [![Zaman Serisi Model türleri bölmesi.](media/v2-update-how-to-tsm/how-to-tsm-type-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-type-panel.png#lightbox)

1. **Yeni bir tür** pop-up modal ekle görüntülemek için + **Ekle'yi** seçin.
1. Türünüz için özellikleri ve değişkenleri girin. Girilen de **Kaydet'i**seçin. 

    [![Tür eklemek için yapılandırma ayarları.](media/v2-update-how-to-tsm/how-to-tsm-add-new-type.png)](media/v2-update-how-to-tsm/how-to-tsm-add-new-type.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Toplu yükleme bir veya daha fazla tür

> [!TIP]
> JSON'da türlerinizi masaüstünüze kaydedebilirsiniz. İndirilen JSON dosyası aşağıdaki adımlarla yüklenebilir.

1. **Upload JSON'ı**seçin.
1. Tür yükünü içeren dosyayı seçin.
1. **Yükle'yi**seçin.

    [![Toplu yükleme seçenekleri.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-types-json.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-types-json.png#lightbox)

### <a name="edit-a-single-type"></a>Tek bir türü edin

1. Türünü seçin ve **edit** veya **kalem simgesini**seçin.
1. Gerekli değişiklikleri yapın ve **Kaydet'i**seçin.

    [![Bölmedeki bir türü edin.](media/v2-update-how-to-tsm/how-to-tsm-edit-type.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-type.png#lightbox)

### <a name="delete-a-type"></a>Bir türü silme

1. Türünü seçin ve **silme** veya **çöp kutusu simgesini**seçin. .

   [![Sil'i seçerek bir türü silin.](media/v2-update-how-to-tsm/how-to-tsm-delete-type.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-type.png#lightbox)

1. Sil'i seçerek silmeişlemini **onaylayın.**

## <a name="next-steps"></a>Sonraki adımlar

- Zaman Serisi Modeli hakkında daha fazla bilgi için [Veri modellemesi'ni](./time-series-insights-update-tsm.md)okuyun.

- Önizleme hakkında daha fazla bilgi edinmek için [Azure Zaman Serisi Öngörüleri Önizleme gezgininde Verileri Görselleştir'i](./time-series-insights-update-explorer.md)okuyun.

- Desteklenen JSON şekilleri hakkında bilgi edinmek için [Desteklenen JSON şekillerini](./time-series-insights-send-events.md#supported-json-shapes)okuyun.
