---
title: Önizleme ortamını tanılama ve sorunlarını giderme-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights önizleme ortamını tanılamayı ve sorun gidermeyi öğrenin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: seodec18
ms.openlocfilehash: 0e4ec63ffe715b17f55fde2a53c15d96d391cdba
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75452599"
---
# <a name="diagnose-and-troubleshoot-a-preview-environment"></a>Önizleme ortamını tanılama ve sorunlarını giderme

Bu makalede, Azure Time Series Insights önizleme ortamınızla çalışırken karşılaşabileceğiniz bazı yaygın sorunlar özetlenmektedir. Makalede ayrıca, her bir sorun için olası nedenler ve çözümler açıklanmaktadır.

## <a name="problem-i-cant-find-my-environment-in-the-preview-explorer"></a>Sorun: ortamı önizleme Gezgininde bulamıyorum

Time Series Insights ortamına erişim izniniz yoksa bu sorun oluşabilir. Kullanıcıların Time Series Insights ortamını görüntülemesi için okuyucu düzeyinde bir erişim rolü olması gerekir. Geçerli erişim düzeylerini doğrulamak ve ek erişim vermek için [Azure portal](https://portal.azure.com/)Time Series Insights kaynağın **veri erişim ilkeleri** bölümüne gidin.

  [veri erişim ilkelerini doğrulamak ![.](media/preview-troubleshoot/verify-data-access-policies.png)](media/preview-troubleshoot/verify-data-access-policies.png#lightbox)

## <a name="problem-no-data-is-seen-in-the-preview-explorer"></a>Sorun: Önizleme Gezgininde hiçbir veri görülmedi

[Azure Time Series Insights önizleme Gezgininde](https://insights.timeseries.azure.com/preview)verilerinizi görmemenizin birkaç yaygın nedeni vardır.

- Olay kaynağınız veri almıyor olabilir.

    Bir olay hub 'ı veya IoT Hub 'ı olan olay kaynağınızın etiketinizden veya örneklerinizden veri aldığını doğrulayın. Doğrulamak için Azure portal kaynağınızın genel bakış sayfasına gidin.

    [Pano ölçümlerine genel bakış ![gözden geçirin.](media/preview-troubleshoot/verify-dashboard-metrics.png)](media/preview-troubleshoot/verify-dashboard-metrics.png#lightbox)

- Olay kaynak verileriniz JSON biçiminde değil.

    Time Series Insights yalnızca JSON verilerini destekler. JSON örnekleri için bkz [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

- Olay kaynak anahtarınıza gerekli bir izin eksik.

  * IoT Hub 'ı için, **hizmet bağlantısı** iznine sahip anahtarı sağlamanız gerekir.

    [IoT Hub izinlerini doğrulamak ![.](media/preview-troubleshoot/verify-correct-permissions.png)](media/preview-troubleshoot/verify-correct-permissions.png#lightbox)

    * Her iki ilke de **ıothubowner** ve **Service** , **hizmet Connect** iznine sahip olduklarından çalışır.

  * Bir olay hub 'ı için, **dinlemesi** iznine sahip anahtarı sağlamanız gerekir.
  
    [Olay Hub 'ı izinlerini gözden ![.](media/preview-troubleshoot/verify-eh-permissions.png)](media/preview-troubleshoot/verify-eh-permissions.png#lightbox)

    * **Okuma** ve **yönetme** Ilkeleri her ikisi de **dinleme** iznine sahip olduklarından çalışır.

- Belirtilen tüketici grubunuz Time Series Insights için özel değil.

    Bir IoT Hub 'ı veya Olay Hub 'ı kaydı sırasında, verileri okumak için kullanılan tüketici grubunu belirtirsiniz. Bu Tüketici grubu, ortam başına benzersiz olmalıdır. Tüketici grubu paylaşılmışsa, temeldeki Olay Hub 'ı, okuyuculardan birinin rastgele bağlantısını otomatik olarak keser. Okunacak Time Series Insights için benzersiz bir tüketici grubu sağlayın.

- Sağlama sırasında belirtilen zaman serisi KIMLIĞI özelliği yanlış, eksik veya null.

    Bu sorun, ortamı sağlama sırasında zaman serisi KIMLIĞI özelliği yanlış yapılandırılmışsa meydana gelebilir. Daha fazla bilgi için bkz. [bir zaman SERISI kimliği seçmek Için en iyi uygulamalar](./time-series-insights-update-how-to-id.md). Şu anda, mevcut bir Time Series Insights ortamını farklı bir zaman serisi KIMLIĞI kullanacak şekilde güncelleştiremezsiniz.

## <a name="problem-some-data-shows-but-some-is-missing"></a>Sorun: bazı veriler gösteriliyor, ancak bazıları eksik

Zaman serisi KIMLIĞI olmadan veri gönderiyor olabilirsiniz.

- Bu sorun, yük içindeki zaman serisi KIMLIĞI alanı olmadan olayları gönderdiğinizde meydana gelebilir. Daha fazla bilgi için bkz. [desteklenen JSON şekilleri](./how-to-shape-query-json.md).
- Bu sorun, ortamınız kısıtlanmakta olduğu için meydana gelebilir.

    > [!NOTE]
    > Time Series Insights, en fazla 6 Mbps alma oranını destekler.

## <a name="problem-my-event-sources-timestamp-property-name-doesnt-work"></a>Sorun: olay kaynağınızın zaman damgası Özellik adı çalışmıyor

Ad ve değer şu kurallara uyar emin olun:

* Zaman damgası Özellik adı büyük/küçük harfe duyarlıdır.
* Olay kaynağınızdan JSON dizesi olarak gelen zaman damgası özelliği değeri `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`biçimindedir. Bu tür bir dize örneğidir `“2008-04-12T12:53Z”`.

Zaman damgası özelliği adınızın yakalanıp düzgün şekilde çalışmasını sağlamanın en kolay yolu Time Series Insights önizleme Gezginini kullanmaktır. Time Series Insights önizleme Gezgini ' nde, zaman damgası özellik adını girdikten sonra bir süre seçmek için grafiği kullanın. Seçime sağ tıklayın ve **olayları keşfet** seçeneğini belirleyin. İlk sütun üst bilgisi, zaman damgası özellik adıdır. Bunun yerine `Timestamp`sözcüğünün yanında `($ts)` olmalıdır:

* `(abc)`, Time Series Insights veri değerlerini dizeler olarak okuduğunu gösterir.
* Time Series Insights, veri değerini DateTime olarak okuduğunu gösteren **Takvim** simgesi.
* `#`, Time Series Insights veri değerlerini tamsayı olarak okuduğunu gösterir.

Zaman damgası özelliği açıkça belirtilmemişse, varsayılan zaman damgası olarak bir olayın IoT Hub 'ı veya Olay Hub 'ı sıraya alma zamanı kullanılır.

## <a name="problem-i-cant-view-data-from-my-warm-store-in-the-explorer"></a>Sorun: gezgin 'de sıcak mağazamın verilerini görüntüleyemiyorum

- Kısa süre önce ısınma deponuzu sağlamış olabilirsiniz ve veriler hala içinde akar.
- Isınma deponuzu silmiş olabilirsiniz ve bu durumda veri kaybı olur.

## <a name="problem-i-cant-view-or-edit-my-time-series-model"></a>Sorun: zaman serisi modelimi görüntüleyemiyorum veya düzenleyemiyorum

- Time Series Insights S1 veya S2 ortamına erişim sağlayabilirsiniz.

   Zaman serisi modelleri yalnızca Kullandıkça Öde ortamlarında desteklenir. Time Series Insights önizleme Gezgini ' nden S1 veya S2 ortamınıza erişme hakkında daha fazla bilgi için bkz. [Explorer 'da verileri görselleştirme](./time-series-insights-update-explorer.md).

   [ortamda hiçbir olay ![.](media/preview-troubleshoot/troubleshoot-no-events.png)](media/preview-troubleshoot/troubleshoot-no-events.png#lightbox)

- Modeli görüntüleme ve düzenleme izinlerine sahip olmayabilirsiniz.

   Kullanıcıların zaman serisi modelini düzenlemek ve görüntülemek için katkıda bulunan düzeyinde erişimi olması gerekir. Geçerli erişim düzeylerini doğrulamak ve ek erişim vermek için, Azure portal Time Series Insights kaynağınızın **veri erişim ilkeleri** bölümüne gidin.

## <a name="problem-all-my-instances-in-the-preview-explorer-lack-a-parent"></a>Sorun: Önizleme Gezgini 'ndeki tüm örneklerim üst öğeye sahip değil

Ortamınızda tanımlı bir zaman serisi model hiyerarşisi yoksa bu sorun oluşabilir. Daha fazla bilgi için bkz. [zaman serisi modelleriyle çalışma](./time-series-insights-update-how-to-tsm.md).

  [![üst öğe olmayan örnekler, bir uyarı görüntüler.](media/preview-troubleshoot/unparented-instances.png)](media/preview-troubleshoot/unparented-instances.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Zaman serisi modelleriyle çalışmayı](./time-series-insights-update-how-to-tsm.md)okuyun.

- [Desteklenen JSON şekilleri](./how-to-shape-query-json.md)hakkında bilgi edinin.

- Azure Time Series Insights önizlemede [planlama ve limitleri](./time-series-insights-update-plan.md) gözden geçirin.
