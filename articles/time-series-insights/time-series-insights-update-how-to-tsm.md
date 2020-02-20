---
title: Önizleme ortamlarında veri modelleme-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights önizlemede veri modelleme hakkında bilgi edinin.
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
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77470760"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Azure Time Series Insights önizlemede veri modelleme

Bu makalede, Azure Time Series Insights önizlemede zaman serisi modeliyle nasıl çalışılacağı açıklanır. Yaygın olarak karşılaşılan çeşitli veri senaryolarına ilişkin ayrıntılar.

> [!TIP]
> * Önizleme [zamanı serisi modeli](time-series-insights-update-tsm.md)hakkında bilgi edinin.
> * [Azure Time Series Insights Preview Explorer](./time-series-insights-update-explorer.md)'da önizleme Kullanıcı arabirimine gitme hakkında daha fazla bilgi edinin.

## <a name="instances"></a>Örnekler

Azure Time Series Insights Gezgini tarayıcıdaki örnek **oluşturma**, **okuma**, **güncelleştirme**ve **silme** işlemlerini destekler. 

Başlamak için, Time Series Insights Explorer **Çözümle** görünümünden **model** görünümünü seçin.

### <a name="create-a-single-instance"></a>Tek bir örnek oluşturma

1. Zaman serisi model Seçicisi paneline gidin ve menüden **örnekler** ' i seçin. Seçtiğiniz Time Series Insights ortamınız ile ilişkili tüm örnekler görüntülenir.

    [![önce örnekleri seçerek tek bir örnek oluşturun.](media/v2-update-how-to-tsm/how-to-tsm-instances-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-instances-panel.png#lightbox)

1. **+ Ekle** öğesini seçin.

    [![+ Ekle düğmesini seçerek bir örnek ekleyin.](media/v2-update-how-to-tsm/how-to-tsm-add-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-add-instance.png#lightbox)

1. Örnek ayrıntılarını girin, tür ve hiyerarşi ilişkilendirmesini seçin ve **Oluştur**' u seçin.

### <a name="bulk-upload-one-or-more-instances"></a>Bir veya daha fazla örneği toplu karşıya yükleme

> [!TIP]
> Örneklerinizi JSON 'daki masaüstünüze kaydedebilirsiniz. İndirilen JSON dosyası daha sonra aşağıdaki adımlarla karşıya yüklenebilir.

1. **JSON karşıya yükle**' yi seçin.
1. Örnekler yükünü içeren dosyayı seçin.

    [örnekleri JSON aracılığıyla toplu karşıya yükleme ![.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-instances.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-instances.png#lightbox)

1. **Karşıya Yükle**’yi seçin.

### <a name="edit-a-single-instance"></a>Tek bir örneği düzenleme

1. Örneği seçin ve **Düzenle** veya **kurşun kalem simgesini**seçin. 
1. Gerekli değişiklikleri yapın ve **Kaydet**' i seçin.

    [tek bir örneği düzenlemek ![.](media/v2-update-how-to-tsm/how-to-tsm-edit-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-instance.png#lightbox)

### <a name="delete-an-instance"></a>Örnek silme

1. Örneği seçin ve **Sil** veya **çöp kutusu simgesini**seçin.

   [Sil ' i seçerek bir örneği silin ![.](media/v2-update-how-to-tsm/how-to-tsm-delete-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-instance.png#lightbox)

1. **Sil**' i seçerek silmeyi onaylayın.

> [!NOTE]
> Bir örnek, bir alan doğrulama denetiminin silinmesine başarılı bir şekilde geçmesi gerekir.

## <a name="hierarchies"></a>Hiyerarşiler

Azure Time Series Insights gezgin, tarayıcı içinde hiyerarşi **oluşturma**, **okuma**, **güncelleştirme**ve **silme** işlemlerini destekler. 

Başlamak için, Time Series Insights Explorer **Çözümle** görünümünden **model** görünümünü seçin.

### <a name="create-a-single-hierarchy"></a>Tek bir hiyerarşi oluşturma

1. Zaman serisi model Seçicisi paneline gidin ve menüden **hiyerarşiler** ' ı seçin. Seçtiğiniz Time Series Insights ortamınız ile ilişkili tüm hiyerarşiler görüntülenir.

    [bölme aracılığıyla bir hiyerarşi oluşturmak ![.](media/v2-update-how-to-tsm/how-to-tsm-hierarchy-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-hierarchy-panel.png#lightbox)

1. **+ Ekle** öğesini seçin.

    [![hiyerarşi + Ekle düğmesi.](media/v2-update-how-to-tsm/how-to-tsm-add-new-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-add-new-hierarchy.png#lightbox)

1. Sağ bölmede **+ Ekle düzeyi** ' ni seçin.

    [hiyerarşiye bir düzey eklemek ![.](media/v2-update-how-to-tsm/how-to-tsm-save-hierarchy-levels.png)](media/v2-update-how-to-tsm/how-to-tsm-save-hierarchy-levels.png#lightbox)

1. Hiyerarşi ayrıntılarını girip **Kaydet**' i seçin.

    [hiyerarşi ayrıntılarını belirtmek ![.](media/v2-update-how-to-tsm/how-to-tsm-add-hierarchy-level.png)](media/v2-update-how-to-tsm/how-to-tsm-add-hierarchy-level.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Bir veya daha fazla hiyerarşiyi toplu karşıya yükleme

> [!TIP]
> Hiyerarşilerinizi, JSON 'daki masaüstünüze kaydedebilirsiniz. İndirilen JSON dosyası daha sonra aşağıdaki adımlarla karşıya yüklenebilir.

1. **JSON karşıya yükle**' yi seçin.
1. Hiyerarşi yükünü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    [hiyerarşileri toplu olarak karşıya yüklemek için ![seçimleri.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-hierarchies.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-hierarchies.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Tek bir hiyerarşiyi düzenleme

1. Hiyerarşiyi seçin ve **Düzenle** veya **kurşun kalem simgesini**seçin.
1. Gerekli değişiklikleri yapın ve **Kaydet**' i seçin.

    [tek bir hiyerarşiyi düzenlemeyle ilgili seçimleri ![.](media/v2-update-how-to-tsm/how-to-tsm-edit-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-hierarchy.png#lightbox)

### <a name="delete-a-hierarchy"></a>Hiyerarşiyi silme

1. Hiyerarşiyi seçin ve **Sil** veya **çöp kutusu simgesini**seçin. 

    [Sil düğmesini seçerek hiyerarşiyi silmek ![.](media/v2-update-how-to-tsm/how-to-tsm-delete-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-hierarchy.png#lightbox)

1. **Sil**' i seçerek silmeyi onaylayın.

## <a name="types"></a>Türler

Azure Time Series Insights Gezgini tarayıcı içinde **oluşturma**, **okuma**, **güncelleştirme**ve **silme** işlemlerini destekler. 

Başlamak için, Time Series Insights Explorer **Çözümle** görünümünden **model** görünümünü seçin.

### <a name="create-a-single-type"></a>Tek bir tür oluşturma

1. Zaman serisi model Seçicisi paneline gidin ve menüden **türler** ' i seçin. Seçtiğiniz Time Series Insights ortamınız ile ilişkili tüm türler görüntülenir.

    [![zaman serisi model türleri bölmesi.](media/v2-update-how-to-tsm/how-to-tsm-type-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-type-panel.png#lightbox)

1. **+ Ekle** ' yi seçerek **Yeni bir tür Ekle** açılan penceresini görüntüleyin.
1. Türü için özellikler ve değişkenler girin. Girdikten sonra **Kaydet**' i seçin. 

    [tür eklemek için yapılandırma ayarları ![.](media/v2-update-how-to-tsm/how-to-tsm-add-new-type.png)](media/v2-update-how-to-tsm/how-to-tsm-add-new-type.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Bir veya daha fazla türü toplu karşıya yükleme

> [!TIP]
> Türlerinizi, JSON 'daki masaüstünüze kaydedebilirsiniz. İndirilen JSON dosyası daha sonra aşağıdaki adımlarla karşıya yüklenebilir.

1. **JSON karşıya yükle**' yi seçin.
1. Tür yükünü içeren dosyayı seçin.
1. **Karşıya Yükle**’yi seçin.

    [![toplu türleri karşıya yükleme seçenekleri.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-types-json.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-types-json.png#lightbox)

### <a name="edit-a-single-type"></a>Tek bir türü düzenleme

1. Türü seçin ve **Düzenle** veya **kurşun kalem simgesini**seçin.
1. Gerekli değişiklikleri yapın ve **Kaydet**' i seçin.

    [bölmedeki bir türü düzenlemek ![.](media/v2-update-how-to-tsm/how-to-tsm-edit-type.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-type.png#lightbox)

### <a name="delete-a-type"></a>Bir tür silme

1. Türü seçin ve **Sil** veya **çöp kutusu simgesini**seçin. arasında yetersiz alanla karşılaştı.

   [![Sil ' i seçerek bir türü silin.](media/v2-update-how-to-tsm/how-to-tsm-delete-type.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-type.png#lightbox)

1. **Sil**' i seçerek silmeyi onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

- Zaman serisi modeli hakkında daha fazla bilgi için [veri modellemesini](./time-series-insights-update-tsm.md)okuyun.

- Önizleme hakkında daha fazla bilgi edinmek için [Azure Time Series Insights önizleme Gezgini ' nde verileri görselleştir](./time-series-insights-update-explorer.md)' i okuyun.

- Desteklenen JSON şekilleri hakkında bilgi edinmek için [desteklenen JSON şekillerini](./time-series-insights-send-events.md#supported-json-shapes)okuyun.
