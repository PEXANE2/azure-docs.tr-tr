---
title: Azure Time Series Insights önizlemeyi tanılama ve sorun giderme | Microsoft Docs
description: Azure Time Series Insights önizlemesi ile tanılama ve sorun gidermeyi öğrenin.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: cdcbe62fdba4f111233451680f95abc757e80ee3
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883328"
---
# <a name="diagnose-and-troubleshoot"></a>Tanılama ve sorun giderme

Bu makalede, Azure Time Series Insights önizleme ortamınızla çalışırken karşılaşabileceğiniz bazı yaygın sorunlar özetlenmektedir. Makalede ayrıca, her bir sorun için olası nedenler ve çözümler açıklanmaktadır.

## <a name="problem-i-cant-find-my-environment-in-the-preview-explorer"></a>Gidermek Önizleme Gezgininde ortamımı bulamıyorum

Time Series Insights ortamına erişim izniniz yoksa bu sorun oluşabilir. Kullanıcıların Time Series Insights ortamını görüntülemesi için okuyucu düzeyinde bir erişim rolü olması gerekir. Geçerli erişim düzeylerini doğrulamak ve ek erişim vermek için, [Azure portal](https://portal.azure.com/)Time Series Insights kaynağın veri erişim ilkeleri bölümünü ziyaret edin.

  [![Ortamınızın](media/v2-update-diagnose-and-troubleshoot/environment.png)](media/v2-update-diagnose-and-troubleshoot/environment.png#lightbox)

## <a name="problem-no-data-is-seen-in-the-preview-explorer"></a>Sorun: Önizleme Gezgininde hiçbir veri görülmedi

[Azure Time Series Insights önizleme Gezgininde](https://insights.timeseries.azure.com/preview)verilerinizi görmemenizin birkaç yaygın nedeni vardır.

- Olay kaynağınız veri almıyor olabilir.

    Bir olay hub 'ı veya IoT Hub 'ı olan olay kaynağınızın etiketinizden veya örneklerinizden veri aldığını doğrulayın. Doğrulamak için Azure portal kaynağınızın genel bakış sayfasına gidin.

    [![Pano-Öngörüler](media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png)](media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png#lightbox)

- Olay kaynak verileriniz JSON biçiminde değil.

    Time Series Insights yalnızca JSON verilerini destekler. JSON örnekleri için bkz [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

- Olay kaynak anahtarınıza gerekli bir izin eksik.

  * IoT Hub 'ı için, **hizmet bağlantısı** iznine sahip anahtarı sağlamanız gerekir.

    [![Yapılandırma](media/v2-update-diagnose-and-troubleshoot/configuration.png)](media/v2-update-diagnose-and-troubleshoot/configuration.png#lightbox)

  * Önceki görüntüde gösterildiği gibi, her iki ilke de, **hizmet bağlantısı** izni olduğundan, her ikisi de **ıothubowner** ve **Service** çalışır.
  * Bir olay hub 'ı için, **dinlemesi** iznine sahip anahtarı sağlamanız gerekir.
  
    [![İzinleri](media/v2-update-diagnose-and-troubleshoot/permissions.png)](media/v2-update-diagnose-and-troubleshoot/permissions.png#lightbox)

  * Önceki görüntüde gösterildiği gibi, **okuma** ve **yönetme** Ilkelerinin her ikisi de **dinleme** iznine sahip olduklarından çalışır.

- Belirtilen tüketici grubunuz Time Series Insights için özel değil.

    Bir IoT Hub 'ı veya Olay Hub 'ı kaydı sırasında, verileri okumak için kullanılan tüketici grubunu belirtirsiniz. Bu tüketici grubunu paylaşmayın. Tüketici grubu paylaşılmışsa, temeldeki Olay Hub 'ı, okuyuculardan birinin rastgele bağlantısını otomatik olarak keser. Okunacak Time Series Insights için benzersiz bir tüketici grubu sağlayın.

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
* JSON dizesi olarak olay kaynağınızdan gelen zaman damgası özelliği değeri, biçimine `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`sahiptir. Bu tür bir dize örneğidir `“2008-04-12T12:53Z”`.

Zaman damgası özelliği adınızın yakalanıp düzgün şekilde çalışmasını sağlamanın en kolay yolu Time Series Insights önizleme Gezginini kullanmaktır. Time Series Insights önizleme Gezgini ' nde, zaman damgası özellik adını girdikten sonra bir süre seçmek için grafiği kullanın. Seçime sağ tıklayın ve **olayları keşfet** seçeneğini belirleyin. İlk sütun üst bilgisi, zaman damgası özellik adıdır. Bunun yerine sözcüğün `($ts)` `Timestamp`yanına olması gerekir:

* `(abc)`Time Series Insights, veri değerlerini dizeler olarak okuduğunu gösterir.
* Takvim simgesi, Time Series Insights veri değerini DateTime olarak okuduğunu gösterir.
* `#`Time Series Insights, veri değerlerini tamsayı olarak okuduğunu gösterir.

Zaman damgası özelliği açıkça belirtilmemişse, varsayılan zaman damgası olarak bir olayın IoT Hub 'ı veya Olay Hub 'ı sıraya alma zamanı kullanılır.

## <a name="problem-i-cant-view-or-edit-my-time-series-model"></a>Gidermek Zaman serisi modelimi görüntüleyemiyorum veya düzenleyemiyorum

- Time Series Insights S1 veya S2 ortamına erişim sağlayabilirsiniz.

   Zaman serisi modelleri yalnızca PAYG ortamlarında desteklenir. Zaman serisi öngörüleri Önizleme Gezgini'nden S1/S2 ortamınızı erişim hakkında daha fazla bilgi için bkz. [gezginde verileri görselleştirme](./time-series-insights-update-explorer.md).

   [![Erişmesini](media/v2-update-diagnose-and-troubleshoot/access.png)](media/v2-update-diagnose-and-troubleshoot/access.png#lightbox)

- Modeli görüntüleme ve düzenleme izinlerine sahip olmayabilirsiniz.

   Kullanıcıların zaman serisi modelini düzenlemek ve görüntülemek için katkıda bulunan düzeyinde erişimi olması gerekir. Geçerli erişim düzeylerini doğrulamak ve ek erişim vermek için, Azure portal Time Series Insights kaynağınızın veri erişim Ilkeleri bölümünü ziyaret edin.

## <a name="problem-all-my-instances-in-the-preview-explorer-lack-a-parent"></a>Sorun: Önizleme Gezgini 'ndeki tüm örneklerim üst öğeye sahip değil

Ortamınızda tanımlı bir zaman serisi model hiyerarşisi yoksa bu sorun oluşabilir. Daha fazla bilgi için bkz. [zaman serisi modelleriyle çalışma](./time-series-insights-update-how-to-tsm.md).

  [![Zaman serisi modelleri](media/v2-update-diagnose-and-troubleshoot/tsm.png)](media/v2-update-diagnose-and-troubleshoot/tsm.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Zaman serisi modelleriyle çalışmayı](./time-series-insights-update-how-to-tsm.md)okuyun.

- [Desteklenen JSON şekilleri](./how-to-shape-query-json.md)hakkında bilgi edinin.
