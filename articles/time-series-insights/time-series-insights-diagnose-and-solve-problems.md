---
title: Sorunları tanıla, sorun giderme ve çözme - Azure Time Series Insights | Microsoft Dokümanlar
description: Bu makalede, Azure Zaman Serisi Öngörüleri ortamınızdaki sık karşılaşılan sorunları nasıl tanılayabildiği, sorun gidereceğiniz ve çözeceğiniz açıklanmaktadır.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 02/04/2020
ms.custom: seodec18
ms.openlocfilehash: 209df97169c71d910677ffdb2e2b12593882445b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80152611"
---
# <a name="diagnose-and-solve-issues-in-your-time-series-insights-environment"></a>Time Series Insights ortamınızdaki sorunları tanılayın ve çözün

Bu makalede, Azure Zaman Serisi Öngörüleri ortamınızda karşılaşabileceğiniz bazı sorunlar açıklanmaktadır. Makale, çözüm için potansiyel nedenler ve çözümler sunar.

## <a name="video"></a>Video

### <a name="learn-about-common-time-series-insights-customer-challenges-and-mitigationsbr"></a>Sık karşılaşılan Time Series Insights müşteri zorlukları ve azaltıcı etkenler hakkında bilgi edinin.</br>

> [!VIDEO https://www.youtube.com/embed/7U0SwxAVSKw]

## <a name="problem-no-data-is-shown"></a>Sorun: veri gösterilmez

Azure Zaman [Serisi Öngörüleri gezgininde](https://insights.timeseries.azure.com) çeşitli sık nedenlerle veri oluşmaz:

### <a name="cause-a-event-source-data-isnt-in-json-format"></a>Neden A: olay kaynağı verileri JSON formatında değil

Azure Time Series Insights yalnızca JSON verilerini destekler. JSON örnekleri için [Desteklenen JSON şekillerini](./how-to-shape-query-json.md)okuyun.

### <a name="cause-b-the-event-source-key-is-missing-a-required-permission"></a>Neden B: olay kaynağı anahtarı gerekli bir izin eksik

* Azure IoT Hub'ındaki bir IoT hub'ı **için, hizmet bağlama** izinlerine sahip anahtarı sağlamanız gerekir. Her ikiside **hizmet bağlama** izinleri olduğundan **iothubowner** veya **hizmet** ilkelerini seçin.

   [![IoT Hub hizmeti izinleri birbirine bağlar](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png#lightbox)

* Azure Etkinlik Hub'larında bir etkinlik hub'ı **için, dinleme** izinleri olan anahtarı sağlamanız gerekir. Her ikisi de **dinleme** izinleri **olduğundan, okuma** veya **yönetme** ilkeleriçalışır.

   [![Olay merkezi dinleme izinleri](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)](media/diagnose-and-solve-problems/eventhub-listen-permissions.png#lightbox)

### <a name="cause-c-the-consumer-group-provided-isnt-exclusive-to-time-series-insights"></a>Neden C: sağlanan tüketici grubu Time Series Insights'a özel değildir

Bir IoT hub'ı veya bir etkinlik hub'ı kaydettirdiğinizde, verileri okumak için kullanmak istediğiniz tüketici grubunu ayarlamak önemlidir. Bu tüketici grubu *paylaşılamaz.* Tüketici grubu paylaşılırsa, temel IoT hub'ı veya olay merkezi otomatik olarak ve okuyuculardan birinin bağlantısını rasgele keser. Zaman Serisi Öngörüleri için benzersiz bir tüketici grubu sağlayın.

### <a name="cause-d-the-environment-has-just-been-provisioned"></a>Neden D: çevre sadece temin edilmiştir

Veriler, ortam ve verileri ilk oluşturulduktan sonra birkaç dakika içinde Zaman Serisi Öngörüleri kaşifinizde görünür.

## <a name="problem-some-data-is-shown-but-data-is-missing"></a>Sorun: bazı veriler gösterilir, ancak veriler eksik

Veriler yalnızca kısmen görünüyorsa ve veriler geride kalıyor gibi görünüyorsa, birkaç olasılığı göz önünde bulundurmanız gerekir.

### <a name="cause-a-your-environment-is-being-throttled"></a>Neden A: ortamınız daraltılıyor

Veri içeren bir olay kaynağı oluşturduktan sonra ortamlar sağlandığında [azaltma](time-series-insights-environment-mitigate-latency.md) yaygın bir sorundur. Azure IoT Hub ve Azure Etkinlikleri Hub'ları verileri yedi güne kadar saklar. Zaman Serisi Öngörüleri her zaman etkinlik kaynağındaki en eski olayla başlar (ilk katılım, ilk çıkan veya *FIFO).*

Örneğin, bir olay kaynağında, tek birimli Bir Zaman Serisi Öngörüleri ortamına bağlandığınızda 5 milyon olay varsa, Time Series Öngörüleri günde yaklaşık 1 milyon olay okur. Time Series Insights beş günlük gecikme süresi yaşıyor gibi görünebilir. Ancak, olan şey çevrenin daraltılması.

Olay kaynağınızda eski olaylar varsa, azaltmaya iki şekilde yaklaşabilirsiniz:

- Zaman Serisi Öngörüleri'nde göstermek istemediğiniz eski olayları kaldırmaya yardımcı olmak için etkinlik kaynağınızın bekletme sınırlarını değiştirin.
- Eski olayların iş bsayısını artırmak için daha büyük bir ortam boyutu (birim sayısı) sağlama. Yukarıdaki örneği kullanarak, aynı S1 ortamını bir gün için beş üniteye çıkarsanız, ortamın bir gün içinde yetişmesi gerekir. Sabit durumlu etkinlik üretiminiz günde 1 milyon veya daha az olaysa, etkinliğin kapasitesini yakaladıktan sonra bir birime indirebilirsiniz.

Azaltma sınırı, çevrenin SKU türüne ve kapasitesine göre uygulanır. Ortamdaki tüm olay kaynakları bu kapasiteyi paylaşır. IoT hub'ınızın veya etkinlik hub'ınızın olay kaynağı verileri zorunlu sınırların ötesine itiyorsa, azaltma ve gecikme yle karşılaşırsınız.

Aşağıdaki şekil, S1 SKU'su ve 3 kapasiteli bir Time Series Insights ortamını göstermektedir. Günde 3 milyon etkinlik yapabilir.

[![Çevre SKU akım kapasitesi](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)](media/diagnose-and-solve-problems/environment-sku-current-capacity.png#lightbox)

Örnek olarak, bir ortamın bir olay merkezinden iletiler yutturdugini varsay. Günlük giriş hızı ~67.000 mesajdır. Bu oran dakikada yaklaşık 46 iletianlamına gelir. 

* Her olay hub iletisi tek bir Time Series Insights olayına düzleştirilmişse, azaltma gerçekleşmez. 
* Her olay hub iletisi 100 Zaman Serisi Öngörüleri etkinliğine düzleştirilmişse, her dakika 4.600 olay yutulmalıdır. 

3 kapasiteye sahip bir S1 SKU ortamı dakikada sadece 2.100 etkinlik (günde 1 milyon etkinlik = üç birim = dakikada 2.100 olay) dakikada 700 olay) 

Düzleme mantığının nasıl çalıştığını üst düzey bir anlayış için [Desteklenen JSON şekillerini](./how-to-shape-query-json.md)okuyun.

#### <a name="recommended-resolutions-for-excessive-throttling"></a>Aşırı azaltma için önerilen çözünürlükler

Gecikmeyi gidermek için ortamınızın SKU kapasitesini artırın. Daha fazla bilgi için [Zaman Serisi Öngörüleri ortamınızı ölçeklendir'i](time-series-insights-how-to-scale-your-environment.md)okuyun.

### <a name="cause-b-initial-ingestion-of-historical-data-slows-ingress"></a>Neden B: geçmiş verilerin ilk yutulması girişi yavaşlatır

Varolan bir olay kaynağını bağlarsanız, Büyük olasılıkla IoT hub'ınız veya olay hub'ınız zaten veri içerir. Ortam, olay kaynağının ileti saklama döneminin başlangıcından itibaren veri çekmeye başlar. Bu varsayılan işleme geçersiz kılınamıyor. Azaltma yımeşgul edebilirsiniz. Azaltma, geçmiş verileri yuttukça yakalamak biraz zaman alabilir.

#### <a name="recommended-resolutions-for-large-initial-ingestion"></a>Büyük başlatma alımı için önerilen çözünürlükler

Gecikmeyi gidermek için:

1. SKU kapasitesini izin verilen maksimum değere yükseltin (bu durumda 10). Kapasiteyi artırdıktan sonra giriş işlemi çok daha hızlı bir şekilde yetişmeye başlar. Artan kapasite için ücretlendirilirsiniz. Ne kadar hızlı yetiştediğinizi görselleştirmek [için, Zaman Serisi Öngörüler gezgininde](https://insights.timeseries.azure.com)kullanılabilirlik grafiğini görüntüleyebilirsiniz.

2. Gecikmeye yakalandığında, SKU kapasitesini normal giriş hızınıza düşürün.

## <a name="problem-data-was-showing-previously-but-is-no-longer-showing"></a>Sorun: veriler daha önce gösteriyordu, ancak artık göstermiyor

TSI artık veri sindirmiyor, ancak olaylar hala Iot Hub'ına veya Olay Hub'ına akıyor

### <a name="cause-a-your-hub-access-key-was-regenerated-and-your-environment-needs-updating"></a>Neden A: Hub erişim anahtarınız yeniden oluşturuldu ve ortamınızın güncellenmesi gerekiyor

Bu sorun, olay kaynağınızı oluştururken sağlanan anahtar artık geçerli olmadığında oluşur. Hub'ınızda telemetri görürsünüz, ancak Zaman Serisi Öngörülerinde Giriş Alınan İletiler yoktur. Anahtarın yenilenip oluşturulmadığından emin değilseniz, Olay Hub'larınızın Etkinlik günlüğünde "Ad Alanı Yetkilendirme Kuralları Oluştur veya Güncelleştir" için arama yapabilir veya IoT hub'ı için "IotHub Kaynağı Oluştur veya güncelleştir" araması yapabilirsiniz.

Zaman Serisi Öngörüleri ortamınızı yeni anahtarla güncellemek için Azure portalında hub kaynağınızı açın ve yeni anahtarı kopyalayın. TSI kaynağınıza gidin ve Olay Kaynakları'nı tıklatın. 

   [![Anahtarı güncelleştirin.](media/diagnose-and-solve-problems/update-hub-key-step-1.png)](media/diagnose-and-solve-problems/update-hub-key-step-1.png#lightbox)

Yutma işleminin durduğu olay kaynağını seçin, yeni tuşa yapıştırın ve Kaydet'i tıklatın.

   [![Anahtarı güncelleştirin.](media/diagnose-and-solve-problems/update-hub-key-step-2.png)](media/diagnose-and-solve-problems/update-hub-key-step-2.png#lightbox)

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Sorun: olay kaynağımın zaman damgası özellik adı ayarı çalışmıyor

Zaman damgası özelliğinin adı ve değerinin aşağıdaki kurallara uygun olduğundan emin olun:

* Zaman damgası özelliği adı büyük/küçük harf duyarlıdır.
* JSON dizesi olarak etkinlik kaynağınızdan gelen zaman damgası özellik değeri _yyyy-MM-ddTHH:mm:ss biçimine sahip olmalıdır. FFFFFFFK_. Bir örnek **2008-04-12T12:53Z**olduğunu.

Zaman damgası özelliği adınızın yakalandığını ve düzgün çalıştığından emin olmanın en kolay yolu, Time Series Insights gezginini kullanmaktır. Zaman Serisi Öngörüleri gezgininde, grafiği kullanarak, zaman damgası özellik adını girdikten sonra bir süre seçin. Seçimi sağ tıklatın ve ardından **Olayları Keşfet** seçeneğini belirleyin.

İlk sütun üstbilgi, zaman damgası özellik adınız olmalıdır. **Zaman Damgası**sözcüğünün yanında **($ts)** görüntülenir.

Aşağıdaki değerler görüntülenmez:

- *(abc)*: Zaman Serisi Öngörüleri'nin veri değerlerini dizeler olarak okuduğunu gösterir.
- *Takvim simgesi*: Zaman Serisi Öngörüleri'nin veri değerini *datetime*olarak okuduğunu gösterir.
- *#*: Zaman Serisi Öngörüleri'nin veri değerlerini bir sonda olarak okuduğunu gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure [Zaman Serisi Öngörüleri'nde gecikme süresini nasıl azaltabilirsiniz](time-series-insights-environment-mitigate-latency.md)hakkında bilgi edinin.

- [Zaman Serisi Öngörüleri ortamınızı nasıl ölçeklendireceklerini](time-series-insights-how-to-scale-your-environment.md)öğrenin.