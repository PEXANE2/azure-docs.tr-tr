---
title: Azure Time Series Insights daraltma nasıl izlenir ve azaltıyor | Microsoft Docs
description: Bu makalede, Azure Time Series Insights gecikme süresi ve azaltmasına neden olan performans sorunlarını izleme, tanılama ve azaltma işlemlerinin nasıl yapılacağı açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 08/27/2019
ms.custom: seodec18
ms.openlocfilehash: 275eff59c56229f45a131e107668b8fefab24536
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70123762"
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights"></a>Azure Time Series Insights gecikme süresini azaltmak için azaltmayı izleyin ve azaltır

Gelen veri miktarı ortamınızın yapılandırmasını aştığında, Azure Time Series Insights gecikme süresi veya azaltma yaşayabilirsiniz.

Ortamınızı çözümlemek istediğiniz veri miktarı için düzgün şekilde yapılandırarak gecikme süresini ve azaltmaktan kaçınabilirsiniz.

Şu durumlarda gecikme süresi ve azaltma deneyiminden karşılaşabilirsiniz:

- Ayrılan giriş hızınızı aşmayacak eski verileri içeren bir olay kaynağı ekleyin (Time Series Insights catch gerekecektir).
- Bir ortama daha fazla olay kaynağı ekleyin ve ek olaylardan bir ani artış elde edin (ortamınızın kapasitesini aşabilir).
- Büyük miktarlarda geçmiş olayları bir olay kaynağına göndererek bir gecikme (Time Series Insights yakalamalı olması gerekir) sonucunu vermez.
- Başvuru verilerini telemetri ile birleştirin ve daha büyük bir olay boyutuna neden olur.  Daraltma açısından, 32 KB 'lik paket boyutuna sahip bir veri paketi, her biri 1 KB boyutunda 32 olay olarak değerlendirilir. İzin verilen en fazla olay boyutu 32 KB; 32 KB 'den büyük veri paketleri kesilir.

## <a name="video"></a>Video

### <a name="learn-about-time-series-insights-data-ingress-behavior-and-how-to-plan-for-itbr"></a>Time Series Insights veri giriş davranışı ve bunun nasıl planlanacağı hakkında bilgi edinin.</br>

> [!VIDEO https://www.youtube.com/embed/npeZLAd9lxo]

## <a name="monitor-latency-and-throttling-with-alerts"></a>Uyarıları izleyen uyarılarla gecikme ve azaltma

Uyarılar, ortamınızın neden olduğu gecikme sorunlarını tanılamanıza ve azaltmanıza yardımcı olabilir.

1. Azure portal, **Uyarılar**' ı seçin.

   [![Larınız](media/environment-mitigate-latency/add-alerts.png)](media/environment-mitigate-latency/add-alerts.png#lightbox)

1. **Kural oluştur** paneli görüntülenir. **Koşul**altında **Ekle** ' yi seçin.

   [![Uyarı Ekle](media/environment-mitigate-latency/alert-pane.png)](media/environment-mitigate-latency/alert-pane.png#lightbox)

1. Sonra, sinyal mantığı için tam koşulları yapılandırın.

   [![Sinyal mantığını Yapılandır](media/environment-mitigate-latency/configure-alert-rule.png)](media/environment-mitigate-latency/configure-alert-rule.png#lightbox)

   Buradan, aşağıdaki koşullardan bazılarını kullanarak uyarılar yapılandırabilirsiniz:

   |Ölçüm  |Açıklama  |
   |---------|---------|
   |**Alınan bayt sayısı**     | Olay kaynaklarından okunan ham bayt sayısı. Ham sayı genellikle özellik adını ve değerini içerir.  |  
   |**Giriş geçersiz Iletiler aldı**     | Tüm Azure Event Hubs veya Azure IoT Hub olay kaynaklarından okunan geçersiz iletilerin sayısı.      |
   |**Giriş alınan Iletiler**   | Tüm Event Hubs veya IoT Hub 'Ları olay kaynaklarından okunan ileti sayısı.        |
   |**Giriş depolanan baytlar**     | Sorgu için depolanan ve kullanılabilir olayların toplam boyutu. Boyut yalnızca özellik değeri üzerinde hesaplanır.        |
   |Giriş **saklı olayları**    |   Depolanan ve sorgu için kullanılabilir düzleştirilmiş olay sayısı.      |
   |**Alınan Ileti zaman gecikmesi alındı**   |  İleti olay kaynağında sıraya alındığı zaman ve giriş sırasında işlendiği zaman arasındaki saniye cinsinden fark.      |
   |Giriş **alınan Ileti sayısı gecikmesi**   |  Olay kaynak bölümünde en son sıraya alınan iletinin sıra numarası ve giriş olarak işlenen iletinin sıra numarası arasındaki fark.      |

   **Done** (Bitti) öğesini seçin.

1. İstenen sinyal mantığını yapılandırdıktan sonra, seçilen uyarı kuralını görsel olarak gözden geçirin.

   [![Giriş](media/environment-mitigate-latency/ingress.png)](media/environment-mitigate-latency/ingress.png#lightbox)

## <a name="throttling-and-ingress-management"></a>Daraltma ve giriş yönetimi

* Kısıtladıysanız, *alınan Ileti zaman gecikmesi*için bir değer görürsünüz ve bu da, örneğin dizin oluşturma süresi dışında bir ileti, olay kaynağını ziyaret eden gerçek zamandan (Appx 'in dizin oluşturma saati hariç). 30-60 saniye).  

  *Alınan Ileti sayısı gecikmesi* de bir değer içermelidir, bu da arkasında kaç ileti olduğunu belirlemenizi sağlar.  En kolay şekilde, ortamınızın kapasitesini, farkı aşmanızı sağlayacak bir boyuta artırmanız gerekir.  

  Örneğin, tek bir birim S1 ortamınız varsa ve bir 5.000.000 ileti gecikmesi olduğunu görürseniz, daha fazla bilgi almak için ortamınızın boyutunu bir gün boyunca altı birim olarak artırabilirsiniz.  Daha hızlı bir şekilde daha fazla yakalamak için daha fazla artırabilirsiniz. Yakalama dönemi, özellikle bir ortamı ilk kez sağlarken, özellikle de olayları zaten olan bir olay kaynağına bağladığınızda veya çok sayıda geçmiş verileri toplu olarak karşıya yüklerken yaygın bir oluşumdır.

* Başka bir yöntem de bir giriş **saklı olayları** ayarlaması > = bir eşik değeri 2 saatlik bir dönem boyunca toplam ortam kapasitenizin altına göre biraz daha düşük.  Bu uyarı, gecikme süresinin yüksek olma olasılığını gösteren kapasiteyi sürekli olarak anlamanıza yardımcı olabilir. 

  Örneğin, üç adet S1 birimi sağlanmışsa (veya bir dakika başına 2100 olay), 2 saat boyunca > = 1900 olay için bir giriş **saklı olayları** uyarısı ayarlayabilirsiniz. Bu eşiği sürekli aşdıysanız ve bu nedenle uyarınızı tetikleyerek, büyük olasılıkla sağlanmış olursunuz.  

* Kısıtladığınızı kuşkulanıyorsanız, giriş **alınan iletilerinizi** olay kaynağınızın yumurtılan iletileriyle karşılaştırabilirsiniz.  Olay Hub 'ınız giriş Alınızdan daha büyükse, Time Series Insights muhtemelenkısıtlanıyor demektir.

## <a name="improving-performance"></a>Performansı artırma

Azaltmayı veya gecikmeyi azaltmak için, bunu düzeltmenin en iyi yolu ortamınızın kapasitesini artırmaktır.

Ortamınızı çözümlemek istediğiniz veri miktarı için düzgün şekilde yapılandırarak gecikme süresini ve azaltmaktan kaçınabilirsiniz. Ortamınıza kapasite ekleme hakkında daha fazla bilgi için bkz. [ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md).

## <a name="next-steps"></a>Sonraki adımlar

- Ek sorun giderme adımları için [Time Series Insights ortamınızdaki sorunları tanılayın ve çözün](time-series-insights-diagnose-and-solve-problems.md).

- Ek Yardım için [MSDN Forumu](https://social.msdn.microsoft.com/Forums/home?forum=AzureTimeSeriesInsights) veya [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-timeseries-insights)bir konuşma başlatın. Ayrıca, Yardımlı Destek seçenekleri için [Azure desteği](https://azure.microsoft.com/support/options/) 'ne de başvurabilirsiniz.
