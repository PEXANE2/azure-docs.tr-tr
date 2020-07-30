---
title: Tanılamayı ayarla
titleSuffix: Azure Digital Twins
description: Bkz. tanılama ayarlarıyla günlüğü etkinleştirme.
author: baanders
ms.author: baanders
ms.date: 7/27/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 60ee2102a2b725c68e29afec9a44de03e52e9467
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87379645"
---
# <a name="enable-logging-with-diagnostics-settings"></a>Tanılama ayarlarıyla günlüğü etkinleştir

Örneğiniz için tanılama ayarlarıyla günlüğe kaydetmeyi etkinleştirerek ölçüm verilerinizi [Log Analytics](../azure-monitor/log-query/get-started-portal.md), bir [Event Hubs](../event-hubs/event-hubs-about.md) uç noktasına veya [Azure depolama](../storage/blobs/storage-blobs-overview.md) 'ya gönderebilirsiniz.

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Tanılama ayarlarını Azure portal açın

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure dijital TWINS örneğinize gidin. Bunu, adını Portal arama çubuğuna yazarak bulabilirsiniz. 

2. Menüden **Tanılama ayarları** ' nı seçin ve ardından **Tanılama ayarı ekleyin**.

    :::image type="content" source="media/how-to-view-metrics/diagnostic-settings.png" alt-text="Tanılama Ayarları sayfasını ve eklenecek düğmeyi gösteren ekran görüntüsü":::

3. Aşağıdaki sayfada aşağıdaki değerleri girin:
 * **Tanılama ayarı adı**: Tanılama ayarlarına bir ad verin.
 * **Kategori ayrıntıları**: hangi işlemleri izlemek istediğinizi seçin ve bu işlemler için tanılamayı etkinleştirmek üzere kutulara göz atın. Tanılama ayarlarının rapor aldığı işlemler şunlardır:
    - DigitalTwinsOperation
    - EventRoutesOperation
    - ModelsOperation
    - Queryoperation başvurusu destekleniyor
    - Tüm Ölçümler
 * **Hedef ayrıntıları**: günlükleri nereye göndermek istediğinizi seçin. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:
    - Log Analytics’e gönderme
    - Bir depolama hesabına arşivle
    - Bir olay hub'ına akış yap

    Hedef seçiminiz için gerekliyse ek ayrıntılar doldurmanız istenebilir.  
    
4. Yeni ayarları kaydedin. 

    :::image type="content" source="media/how-to-set-up-diagnostics/diagnostic-settings-details.png" alt-text="Tanılama Ayarları sayfasını ve eklenecek düğmeyi gösteren ekran görüntüsü":::

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, olay, örneğiniz için **Tanılama ayarları** sayfasında yapılandırılan hedefte geri görüntülenir. 

## <a name="next-steps"></a>Sonraki adımlar

* Tanılamayı yapılandırma hakkında daha fazla bilgi için bkz. [*Azure kaynaklarınızdan günlük verilerini toplama ve*](../azure-monitor/platform/platform-logs-overview.md)kullanma.
* Azure dijital TWINS ölçümleri hakkında daha fazla bilgi için bkz [ *. nasıl yapılır: Azure izleyici Ile ölçümleri görüntüleme*](how-to-view-metrics.md)