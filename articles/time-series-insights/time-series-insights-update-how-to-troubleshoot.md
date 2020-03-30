---
title: Bir Önizleme ortamını tanılayın ve sorun giderme - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Azure Zaman Serisi Öngörüleri Önizleme ortamını nasıl tanılayıp sorun gidereceklerini öğrenin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: 667dee6365f38ae058e91c61c24838d8912df26a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80152679"
---
# <a name="diagnose-and-troubleshoot-a-preview-environment"></a>Önizleme ortamını tanılama ve sorun giderme

Bu makalede, Azure Zaman Serisi Öngörüleri Önizleme ortamınızla çalışırken karşılaşabileceğiniz sık karşılaşılan birkaç sorun özetlenmiştir. Makalede, her sorunun olası nedenleri ve çözümleri de açıklanmaktadır.

## <a name="problem-i-cant-find-my-environment-in-the-preview-explorer"></a>Sorun: Önizleme gezgininde ortamımı bulamıyorum

Zaman Serisi Öngörüleri ortamına erişmek için izinleriniz yoksa, bu sorun oluşabilir. Kullanıcıların Time Series Insights ortamlarını görüntülemek için okuyucu düzeyinde bir erişim rolüne ihtiyaçları vardır. Geçerli erişim düzeylerini doğrulamak ve ek erişim sağlamak için [Azure portalındaki](https://portal.azure.com/)Zaman Serisi Öngörüleri kaynağındaki **Veri Erişim İlkeleri** bölümüne gidin.

  [![Veri erişim ilkelerini doğrulayın.](media/preview-troubleshoot/verify-data-access-policies.png)](media/preview-troubleshoot/verify-data-access-policies.png#lightbox)

## <a name="problem-no-data-is-seen-in-the-preview-explorer"></a>Sorun: Önizleme gezgininde veri görülmez

Verilerinizin [Azure Zaman Serisi Öngörüleri Önizleme gezgininde](https://insights.timeseries.azure.com/preview)görünmemelerinin birkaç yaygın nedeni vardır.

- Olay kaynağınız veri almıyor olabilir.

    Olay merkezi veya IoT hub'ı olan olay kaynağınızın etiketlerinizden veya örneklerinizden veri aldığını doğrulayın. Doğrulamak için Azure portalındaki kaynağınızın genel bakış sayfasına gidin.

    [![Pano ölçümlerine genel bakışı gözden geçirin.](media/preview-troubleshoot/verify-dashboard-metrics.png)](media/preview-troubleshoot/verify-dashboard-metrics.png#lightbox)

- Etkinlik kaynağı verileriniz JSON formatında değildir.

    Time Series Insights yalnızca JSON verilerini destekler. JSON örnekleri için [Desteklenen JSON şekillerini](./how-to-shape-query-json.md)okuyun.

- Olay kaynağı anahtarınız gerekli izni kaçırıyor.

  * Bir IoT hub'ı **için, hizmet bağlantısı** izni olan anahtarı sağlamanız gerekir.

    [![IoT hub izinlerini doğrulayın.](media/preview-troubleshoot/verify-correct-permissions.png)](media/preview-troubleshoot/verify-correct-permissions.png#lightbox)

    * Hizmet **bağlama** iznine sahip oldukları için hem ilkeler **iothubowner** hem de **hizmet** çalışır.

  * Bir olay hub'ı **için, Dinleme** izni olan anahtarı sağlamanız gerekir.
  
    [![Olay hub izinlerini gözden geçirin.](media/preview-troubleshoot/verify-eh-permissions.png)](media/preview-troubleshoot/verify-eh-permissions.png#lightbox)

    * Hem **Okuma** hem de **Yönet** **ilkeleri, Dinleme** iznine sahip oldukları için çalışır.

- Sağlanan tüketici grubunuzun sağladığı zaman serisi öngörüleri münhasır değildir.

    Bir IoT hub'ının veya etkinlik merkezinin kaydı sırasında, verileri okumak için kullanılan tüketici grubunu belirtirsiniz. Bu tüketici grubu çevre başına benzersiz olmalıdır. Tüketici grubu paylaşılırsa, temel olay hub'ı okuyuculardan birinin bağlantısını rasgele otomatik olarak keser. Zaman Serisi Öngörüleri için benzersiz bir tüketici grubu sağlayın.

- Sağlama sırasında belirtilen Zaman Serisi Kimliği özelliğiniz yanlış, eksik veya geçersizdir.

    Zaman Serisi Kimliği özelliği ortamın sağlanması sırasında yanlış yapılandırılırsa, bu sorun oluşabilir. Daha fazla bilgi [için, Zaman Serisi Kimliği seçmek için en iyi uygulamaları](./time-series-insights-update-how-to-id.md)okuyun. Şu anda, farklı bir Time Series Kimliği kullanmak için varolan bir Time Series Öngörüleri ortamını güncelleştiremezsiniz.

## <a name="problem-some-data-shows-but-some-is-missing"></a>Sorun: Bazı veriler gösterir, ancak bazı eksik

Zaman Serisi Kimliği olmadan veri gönderiyor olabilirsiniz.

- Bu sorun, yükte Zaman Serisi Kimliği alanı olmadan olay gönderdiğinde oluşabilir. Daha fazla bilgi için [Desteklenen JSON şekillerini](./how-to-shape-query-json.md)okuyun.
- Ortamınız daraltıldığı için bu sorun oluşabilir.

    > [!NOTE]
    > Şu anda, Time Series Insights maksimum 6 Mbps'lik bir alım hızını destekler.

## <a name="problem-data-was-showing-but-now-ingestion-has-stopped"></a>Sorun: Veriler gösteriyordu, ama şimdi yutma durdu

- Olay kaynağı anahtarınız yenilendi ve Önizleme ortamınızın yeni olay kaynağı anahtarına ihtiyacı olabilir.

Bu sorun, olay kaynağınızı oluştururken sağlanan anahtar artık geçerli olmadığında oluşur. Hub'ınızda telemetri görürsünüz, ancak Zaman Serisi Öngörülerinde Giriş Alınan İletiler yoktur. Anahtarın yenilenip oluşturulmadığından emin değilseniz, Olay Hub'larınızın Etkinlik günlüğünde "Ad Alanı Yetkilendirme Kuralları Oluştur veya Güncelleştir" için arama yapabilir veya IoT hub'ı için "IotHub Kaynağı Oluştur veya güncelleştir" araması yapabilirsiniz. 

Zaman Serisi Öngörüleri Önizleme ortamınızı yeni anahtarla güncellemek için Azure portalında hub kaynağınızı açın ve yeni anahtarı kopyalayın. TSI kaynağınıza gidin ve Olay Kaynakları'nı tıklatın. 

   [![Anahtarı güncelleştirin.](media/preview-troubleshoot/update-hub-key-step-1.png)](media/preview-troubleshoot/update-hub-key-step-1.png#lightbox)

Yutma işleminin durduğu olay kaynağını seçin, yeni tuşa yapıştırın ve Kaydet'i tıklatın.

   [![Anahtarı güncelleştirin.](media/preview-troubleshoot/update-hub-key-step-2.png)](media/preview-troubleshoot/update-hub-key-step-2.png#lightbox)

## <a name="problem-my-event-sources-timestamp-property-name-doesnt-work"></a>Sorun: Etkinlik kaynağımın Timestamp özellik adı çalışmıyor

Ad ve değerin aşağıdaki kurallara uygun olduğundan emin olun:

* Timestamp özellik adı büyük/küçük harf duyarlıdır.
* JSON dizesi olarak olay kaynağınızdan gelen Timestamp `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`özellik değeri biçimine sahiptir. Böyle bir dize `“2008-04-12T12:53Z”`bir örnek .

Timestamp özellik adınızın yakalandığını ve düzgün çalıştığından emin olmanın en kolay yolu, Time Series Insights Preview gezginini kullanmaktır. Zaman Serisi Öngörüleri Önizleme gezgini içinde, Zaman Damgası özellik adını sağladıktan sonra bir süre seçmek için grafiği kullanın. Seçimi sağ tıklatın ve **olayları keşfet** seçeneğini seçin. İlk sütun üstbilgi, Timestamp özellik adınızdır. Bunun yerine, `($ts)` kelimenin `Timestamp`yanında olmalıdır:

* `(abc)`, Zaman Serisi Öngörüleri'nin veri değerlerini dize olarak okuduğunu gösterir.
* Zaman Serisi Öngörüleri'nin veri değerini tarih olarak okuduğunu gösteren **takvim** simgesi.
* `#`, Zaman Serisi Öngörüleri'nin veri değerlerini bir sonda olarak okuduğunu gösterir.

Zaman Damgası özelliği açıkça belirtilmemişse, bir olayın IoT hub'ı veya olay merkezi Enqueued Time varsayılan zaman damgası olarak kullanılır.

## <a name="problem-i-cant-view-data-from-my-warm-store-in-the-explorer"></a>Sorun: Explorer'daki sıcak depomdaki verileri görüntüleyemiyorum

- Yakın zamanda sıcak mağazanızı sağlamış olabilirsiniz ve veriler hala akmaya devam ediyor.
- Sıcak mağazanızı silmiş olabilirsiniz, bu durumda veri kaybetmiş olabilirsiniz.

## <a name="problem-i-cant-view-or-edit-my-time-series-model"></a>Sorun: Zaman Serisi Modelimi görüntüleyemiyorum veya edemiyorum

- Bir Time Series Insights S1 veya S2 ortamına erişiyor olabilirsiniz.

   Zaman Serisi Modelleri yalnızca istediğiniz kadar öde ortamlarında desteklenir. Zaman Serisi Öngörüler Önizleme gezgininden S1 veya S2 ortamınıza nasıl erişilisüreceğiniz hakkında daha fazla bilgi [için, explorer'daki verileri Görselleştir'i](./time-series-insights-update-explorer.md)okuyun.

   [![Çevrede olay yok.](media/preview-troubleshoot/troubleshoot-no-events.png)](media/preview-troubleshoot/troubleshoot-no-events.png#lightbox)

- Modeli görüntülemek ve düzenleme izniniz olmayabilir.

   Kullanıcıların Zaman Serisi Modelini düzenlemek ve görüntülemek için katılımcı düzeyinde erişime ihtiyaçları vardır. Geçerli erişim düzeylerini doğrulamak ve ek erişim sağlamak için Azure portalındaki Time Series Öngörüleri kaynağınızdaki **Veri Erişim İlkeleri** bölümüne gidin.

## <a name="problem-all-my-instances-in-the-preview-explorer-lack-a-parent"></a>Sorun: Önizleme gezginindeki tüm örneklerimde bir ebeveyn yok

Ortamınızda Zaman Serisi Modeli hiyerarşisi tanımlı değilse, bu sorun oluşabilir. Daha fazla bilgi için [Zaman Serisi Modelleri ile Çalışma](./time-series-insights-update-how-to-tsm.md)'yı okuyun.

  [![Ebeveynsiz örnekler bir uyarı görüntüler.](media/preview-troubleshoot/unparented-instances.png)](media/preview-troubleshoot/unparented-instances.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- [Zaman Serisi Modelleri ile İş](./time-series-insights-update-how-to-tsm.md)Oku .

- Desteklenen [JSON şekilleri](./how-to-shape-query-json.md)hakkında bilgi edinin.

- Azure Time Series Öngörüleri Önizleme'de [planlamayı ve sınırları](./time-series-insights-update-plan.md) gözden geçirin.
