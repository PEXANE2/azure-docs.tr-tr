---
title: Sorunları tanılama, sorun giderme ve çözme-Azure Time Series Insights
description: Bu makalede Azure Time Series Insights ortamınızdaki yaygın sorunların nasıl tanılanacağı, giderileceği ve çözüleceği açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 02/04/2020
ms.custom: seodec18
ms.openlocfilehash: 4d9efa1ebf1a3e3b146c4f45b0e84047562141cd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192723"
---
# <a name="diagnose-and-solve-issues-in-your-time-series-insights-environment"></a>Time Series Insights ortamınızdaki sorunları tanılayın ve çözün

Bu makalede Azure Time Series Insights ortamınızda karşılaşabileceğiniz sorunlar açıklanmaktadır. Makalede olası nedenler ve çözüm çözümleri sunulmaktadır.

## <a name="video"></a>Video

### <a name="learn-about-common-time-series-insights-challenges-and-mitigationsbr"></a>Yaygın Time Series Insights zorlukları ve azaltmaları hakkında bilgi edinin</br>

> [!VIDEO https://www.youtube.com/embed/7U0SwxAVSKw]

## <a name="problem-no-data-is-shown"></a>Sorun: veri gösterilmemiştir

[Azure Time Series Insights Gezgininde](https://insights.timeseries.azure.com)hiçbir veri görünmemişse, bu genel nedenleri göz önünde bulundurun.

### <a name="cause-a-event-source-data-isnt-in-json-format"></a>Neden: olay kaynak verileri JSON biçiminde değil

Azure Time Series Insights yalnızca JSON verilerini destekler. JSON örnekleri için bkz. [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

### <a name="cause-b-the-event-source-key-is-missing-a-required-permission"></a>Neden B: olay kaynak anahtarında gerekli bir izin eksik

* Azure IoT Hub IoT Hub 'ı için, hizmet bağlantısı izinleri olan anahtarı sağlamanız gerekir. **İothubowner** ya da **hizmet** İlkesi ' ni seçin. Her ikisinde de hizmet bağlantısı izinleri vardır.

   [![IoT Hub hizmet bağlantısı izinleri](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png#lightbox)

* Azure Event Hubs bir olay hub 'ı için, dinleme izinleri olan anahtarı sağlamanız gerekir. **Okuma** ve **yönetme** ilkeleri her ikisi de dinleme izinleri içerdiğinden her ikisi de çalışacaktır.

   [![Olay Hub 'ı dinleme izinleri](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)](media/diagnose-and-solve-problems/eventhub-listen-permissions.png#lightbox)

### <a name="cause-c-the-provided-consumer-group-isnt-exclusive-to-time-series-insights"></a>Neden C: belirtilen tüketici grubu Time Series Insights özel değil

Bir IoT Hub 'ı veya bir olay hub 'ını kaydettiğinizde, verileri okumak için kullanmak istediğiniz tüketici grubunu ayarlamanız önemlidir. Bu Tüketici grubu *paylaşılamıyor*. Tüketici grubu paylaşılmışsa, temel alınan IoT Hub 'ı veya Olay Hub 'ı otomatik olarak okuyuculardan birinin bağlantısını keser. Okunacak Time Series Insights için benzersiz bir tüketici grubu sağlayın.

### <a name="cause-d-the-environment-has-just-been-provisioned"></a>Neden D: ortam henüz sağlandı

Veriler, ortam ve verileri ilk oluşturulduktan sonra birkaç dakika içinde Time Series Insights Gezgini 'nde görünür.

## <a name="problem-some-data-is-shown-but-data-is-missing"></a>Sorun: bazı veriler gösteriliyor, ancak veriler eksik

Veriler yalnızca kısmen göründüğünde ve veriler yanlış olduğunda bu olası sorunları göz önünde bulundurun.

### <a name="cause-a-your-environment-is-being-throttled"></a>Neden: ortamınız kısıtlanıyor

Veri içeren bir olay kaynağı oluşturduktan sonra, ortamlar sağlandığında [kısıtlama](time-series-insights-environment-mitigate-latency.md) yaygın bir sorundur. Azure IoT Hub ve Azure olayları hub 'Ları, verileri yedi güne kadar depolar. Time Series Insights her zaman olay kaynağındaki en eski olay (ilk ın, ilk çıkar veya *FIFO*) ile başlar.

Örneğin, bir S1, tek birimlik Time Series Insights ortamına bağlandığınızda bir olay kaynağında 5.000.000 olayındaysanız, Time Series Insights günde yaklaşık 1.000.000 olay okur. Time Series Insights beş günden fazla gecikme yaşmış olabilir. Ancak neler olduğu, ortamın kısıtlandığı şeydir.

Olay kaynağınızda eski olaylarınız varsa, azaltmayı iki şekilde değiştirebilirsiniz:

- Time Series Insights ' de görünmesini istemediğiniz eski olayları kaldırmaya yardımcı olması için olay kaynağınızın saklama sınırlarını değiştirin.
- Eski olayların aktarım hızını artırmak için daha büyük bir ortam boyutu sağlayın (birim sayısı). Yukarıdaki örnekte, aynı S1 ortamını bir gün için beş birim ile artırırsanız, ortam bir gün içinde yakalamalı. Sabit durum olay üretim, günde 1.000.000 veya daha az olay ise, Time Series Insights tamamlandıktan sonra olay kapasitesini bir birimle azaltabilirsiniz.

Zorlanan azaltma sınırı, ortamın SKU türünü ve kapasitesini temel alır. Ortamdaki tüm olay kaynakları bu kapasiteyi paylaşır. IoT Hub veya Olay Hub 'ınızın olay kaynağı verileri zorlanan limitlerin ötesinde alıyorsa, azaltma ve bir gecikme yaşar.

Aşağıdaki şekilde, S1 SKU 'SU ve 3 kapasitesi olan bir Time Series Insights ortamı gösterilmektedir. Günde 3.000.000 olay alabilir.

[![Ortam kapasitesi](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)](media/diagnose-and-solve-problems/environment-sku-current-capacity.png#lightbox)

Bir olay hub 'ından iletileri gösteren bir ortam düşünün. 67.000 ileti hakkında günlük giriş oranına sahiptir. Bu oran her dakikada yaklaşık 46 ileti dönüştürür.

* Her olay hub 'ı iletisi tek bir Time Series Insights olayına düzleştirilir, azaltma gerçekleşmez.
* Her olay hub 'ı iletisi 100 Time Series Insights olayına düzleştirilir, her dakika 4.600 olay belirtilmelidir.

Kapasitesi 3 olan bir S1 SKU ortamı, her dakikada yalnızca 2.100 olay alabilir (her gün 1.000.000 olay 700 = ayda, üç birim için dakikada olay = 2.100 olay).

Düzleştirme mantığının nasıl çalıştığını yüksek düzeyde anlamak için bkz. [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

#### <a name="recommended-resolutions-for-excessive-throttling"></a>Aşırı azaltma için önerilen çözümler

Gecikme sayısını onarmak için ortamınızın SKU kapasitesini artırın. Daha fazla bilgi için [Time Series Insights ortamınızdaki ölçeği](time-series-insights-how-to-scale-your-environment.md)okuyun.

### <a name="cause-b-initial-ingestion-of-historical-data-slows-ingress"></a>Neden B: geçmiş verilerin Ilk alımı yavaşlar

Var olan bir olay kaynağını bağladığınızda, IoT Hub veya Olay Hub 'ınız zaten veri içeriyor olabilir. Ortam, olay kaynağının ileti bekletme döneminin başından itibaren veri çekmeye başlar. Bu varsayılan işlem geçersiz kılınamaz. Azaltmayı izleyebilirsiniz. Azaltma, geçmiş verileri alırken biraz zaman alabilir.

#### <a name="recommended-resolutions-for-large-initial-ingestion"></a>Büyük başlangıç alımı için önerilen çözümler

Öteleme 'i onarmak için:

1. SKU kapasitesini izin verilen maksimum değere (Bu durumda 10) yükseltin. Kapasiteyi artırdıktan sonra, giriş işlemi çok daha hızlı bir şekilde daha hızlı bir şekilde çalışmaya başlar. Artan kapasite için ücretlendirilirsiniz. Ne kadar hızlı bir şekilde yakalamak istediğinizi görselleştirmek için, kullanılabilirlik grafiğini [Time Series Insights Gezgini](https://insights.timeseries.azure.com)'nde görüntüleyebilirsiniz.

2. Öteleme yakalandığında, SKU kapasitesini normal giriş oranınızı azaltın.

## <a name="problem-data-was-showing-previously-but-is-no-longer-showing"></a>Sorun: veriler daha önce gösteriliyor ancak artık gösterilmiyor

Time Series Insights artık verileri geri alma, ancak olaylar hala IoT Hub veya Olay Hub 'ına akışa yaramazsa, bu olası nedeni göz önünde bulundurun.

### <a name="cause-a-your-hub-access-key-was-regenerated-and-your-environment-needs-to-be-updated"></a>Bunun nedeni: hub erişim anahtarınız yeniden üretildi ve ortamınızın güncelleştirilmesi gerekiyor

Bu sorun, olay kaynağınızı oluştururken girilen anahtar artık geçerli olmadığında oluşur. Hub 'ınızda telemetri görürsünüz ancak Time Series Insights Ileti alınmadı. Anahtarın yeniden oluşturulup üretilmediğini bilmiyorsanız, "ad alanı yetkilendirme kuralları oluşturma veya güncelleştirme" için Olay Hub 'ının etkinlik günlüğünde arama yapabilirsiniz. IoT Hub 'ı için "ıothub kaynağı oluşturma veya güncelleştirme" araması yapın.

Time Series Insights ortamınızı yeni anahtarla güncelleştirmek için, hub kaynağınızı Azure portal açın ve yeni anahtarı kopyalayın. Time Series Insights kaynağına gidin ve **olay kaynaklarını**seçin:

   [![Olay kaynaklarını seçin](media/diagnose-and-solve-problems/update-hub-key-step-1.png)](media/diagnose-and-solve-problems/update-hub-key-step-1.png#lightbox)

Alımı durdurulan olay kaynağını veya kaynakları seçin, yeni anahtarı yapıştırın ve **Kaydet**' i seçin:

   [![Yeni anahtarı Yapıştır](media/diagnose-and-solve-problems/update-hub-key-step-2.png)](media/diagnose-and-solve-problems/update-hub-key-step-2.png#lightbox)

## <a name="problem-the-event-sources-timestamp-property-name-setting-doesnt-work"></a>Sorun: olay kaynağının zaman damgası Özellik adı ayarı çalışmıyor

Olay kaynağınızdan JSON dizesi olarak gelen zaman damgası özelliği değerinin _yyyy-mm-ddTHH: mm: ss biçiminde olduğundan emin olun. GÖNDERILDIĞINDE FFFFFFFK BIÇIMINDE_. Örnek: **2008-04-12T12:53Z**.

Zaman damgası özelliği adının büyük/küçük harfe duyarlı olduğunu unutmayın.

Zaman damgası özelliği adınızın yakalanıp düzgün şekilde çalışmasını sağlamanın en kolay yolu Time Series Insights Gezginini kullanmaktır. Time Series Insights Gezgini ' nde, grafiği kullanarak, zaman damgası özellik adını girdikten sonra bir süre seçin. Seçime sağ tıklayın ve ardından **olayları keşfet**' i seçin.

İlk sütun üst bilgisi, zaman damgası özelliğinin adı olmalıdır. Sözcük **zaman damgasının**yanında **($TS)** görüntülenir.

Aşağıdaki değerler gösterilmez:

- *(ABC)*: Time Series Insights, veri değerlerini dizeler olarak okudığını gösterir.
- *Takvim simgesi*: Time Series Insights, veri değerlerini DateTime değerleri olarak okudığını gösterir.
- *#*: Time Series Insights veri değerlerini tamsayılar olarak okudığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Time Series Insights gecikme süresini azaltma](time-series-insights-environment-mitigate-latency.md)hakkında bilgi edinin.

- [Time Series Insights ortamınızı ölçeklendirmeyi](time-series-insights-how-to-scale-your-environment.md)öğrenin.
