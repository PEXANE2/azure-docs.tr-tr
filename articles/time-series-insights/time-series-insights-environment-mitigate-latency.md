---
title: Azaltma nasıl izlenir ve azaltılanın - Azure Zaman Serisi Öngörüleri | Microsoft Dokümanlar
description: Azure Zaman Serisi Öngörüleri'nde gecikme ve azaltmaya neden olan performans sorunlarını nasıl izleyeceğinizi, tanılamayı ve azalttık öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: troubleshooting
ms.date: 01/21/2020
ms.custom: seodec18
ms.openlocfilehash: 245a0b18187ff1c1b226e94b03374f2c071e51c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76314836"
---
# <a name="monitor-and-mitigate-throttling-to-reduce-latency-in-azure-time-series-insights"></a>Azure Zaman Serisi Öngörüleri'nde gecikme süresini azaltmak için azaltmayı izleyin ve azaltın

Gelen veri miktarı ortamınızın yapılandırmasını aştığında, Azure Zaman Serisi Öngörüleri'nde gecikme veya azaltma yla karşılaşabilirsiniz.

Ortamınızı çözümlemek istediğiniz veri miktarı için düzgün bir şekilde yapılandırarak gecikme ve azaltmayı önleyebilirsiniz.

Şunları zaman gecikme ve azaltma deneyimi yaşama olasılığınız en yüksektir:

- Ayrılan giriş hızınızı aşabilecek eski verileri içeren bir olay kaynağı ekleyin (Zaman Serisi Öngörüleri'nin yetişmeniz gerekir).
- Bir ortama daha fazla olay kaynağı ekleyerek ek olaylardan (ortamınızın kapasitesini aşabilir) bir artışa neden olabilir.
- Büyük miktarda tarihi olayı bir etkinlik kaynağına iterek gecikmeyle sonuçlanır (Zaman Serisi Öngörüleri'nin yetişmesi gerekir).
- Telemetri ile referans verilerini birleştirerek daha büyük olay boyutuna yol açacak. Azaltma açısından bakıldığında, paket boyutu 32 KB olan bir girişli veri paketi, her biri 1 KB boyutunda 32 olay olarak kabul edilir. İzin verilen maksimum olay boyutu 32 KB'dir; 32 KB'den büyük veri paketleri kesilir.

## <a name="video"></a>Video

### <a name="learn-about-time-series-insights-data-ingress-behavior-and-how-to-plan-for-itbr"></a>Time Series Insights veri girişi davranışı ve bunun için nasıl plan yapılacağını öğrenin.</br>

> [!VIDEO https://www.youtube.com/embed/npeZLAd9lxo]

## <a name="monitor-latency-and-throttling-with-alerts"></a>Uyarılarla gecikme ve azaltmayı izleme

Uyarılar, ortamınızda oluşan gecikme sorunlarını tanılamanıza ve azaltmanıza yardımcı olabilir.

1. Azure portalında Time Series Öngörüleri ortamınızı seçin. Ardından **Uyarılar'ı**seçin.

   [![Time Series Insights ortamınıza uyarı ekleme](media/environment-mitigate-latency/mitigate-latency-add-alert.png)](media/environment-mitigate-latency/mitigate-latency-add-alert.png#lightbox)

1. **+ Yeni uyarı kuralı**'nı seçin. Daha sonra **Oluştur kuralı** paneli görüntülenir. KOŞUL altında **Ekle'yi** seçin. **CONDITION**

   [![Uyarı bölmesi ekleme](media/environment-mitigate-latency/mitigate-latency-add-pane.png)](media/environment-mitigate-latency/mitigate-latency-add-pane.png#lightbox)

1. Ardından, sinyal mantığı için tam koşulları yapılandırın.

   [![Sinyal mantığını yapılandırma](media/environment-mitigate-latency/configure-alert-rule.png)](media/environment-mitigate-latency/configure-alert-rule.png#lightbox)

   Buradan, aşağıdaki koşullardan bazılarını kullanarak uyarıları yapılandırabilirsiniz:

   |Ölçüm  |Açıklama  |
   |---------|---------|
   |**Giriş Alınan Baytlar**     | Ham bayt sayısı olay kaynaklarından okundu. Ham sayısı genellikle özellik adını ve değerini içerir.  |  
   |**Giriş Alınan Geçersiz İletiler**     | Tüm Azure Etkinlik Hub'larından veya Azure IoT Hub etkinlik kaynaklarından okunan geçersiz ileti sayısı.      |
   |**Alınan İletileri Gir**   | Tüm Olay Hub'larından veya IoT Hub'larından okunan ileti sayısı olay kaynakları.        |
   |**Giriş Depolanan Baytlar**     | Depolanan ve sorgu için kullanılabilir olayların toplam boyutu. Boyut yalnızca özellik değeri yle hesaplanır.        |
   |**Giriş Saklı Etkinlikler**    |   Depolanan ve sorgu için kullanılabilir düzleştirilmiş olayların sayısı.      |
   |**Giriş Alınan İleti Süresi Gecikmesi**   |  İletinin olay kaynağında sıralı olduğu saat ile Giriş'te işlendiği saat arasındaki saniye farkı.      |
   |**Giriş Alınan İleti Sayısı Gecikmesi**   |  Olay kaynağı bölümündeki son sıralanan iletinin sıra numarası ile Giriş'te işlenen iletinin sıra numarası arasındaki fark.      |

   **Done** (Bitti) öğesini seçin.

1. İstenilen sinyal mantığını yapılandırdıktan sonra, seçilen uyarı kuralını görsel olarak gözden geçirin.

   [![Gecikme görünümü ve grafik](media/environment-mitigate-latency/mitigate-latency-view-and-charting.png)](media/environment-mitigate-latency/mitigate-latency-view-and-charting.png#lightbox)

## <a name="throttling-and-ingress-management"></a>Azaltma ve giriş yönetimi

* Eğer daraltılıyorsanız, TIme Series Insights ortamınızın kaç saniye gerisinde olduğunuzu bildiren Ingress *Alınan İleti Zaman Gecikmesi* için bir değer kaydedilir (iletinin olay kaynağına tam olarak vurduğu andan itibaren (appx'in dizin leme süresi hariç). 30-60 saniye).  

  *Giriş Alınan İleti Sayısı Gecikmesi* de bir değere sahip olmalıdır, bu da arkanda kaç ileti olduğunu belirlemenize olanak sağlar.  Yakalanmanın en kolay yolu, çevrenizin kapasitesini aradaki farkı aşmanızı sağlayacak bir boyuta çıkarmaktır.  

  Örneğin, S1 ortamınız 5.000.000 iletide gecikme gösteriyorsa, yakalanmak için ortamınızın boyutunu yaklaşık bir gün boyunca altı üniteye çıkarabilirsiniz.  Daha hızlı yakalamak için daha da artabilir. Bir ortamı başlangıçta sağlarken, özellikle de içinde olaylar olan bir olay kaynağına bağladığınızda veya çok sayıda geçmiş veri yüklediğinizde, yakalama süresi sık karşılaşılan bir durumdur.

* Başka bir teknik, 2 saatlik bir süre için toplam ortam kapasitenizin biraz altında bir eşik >= bir **Giriş Depolanan Olaylar** uyarısı ayarlamaktır.  Bu uyarı, sürekli kapasitede olup olmadığını anlamanıza yardımcı olabilir ve bu da yüksek bir gecikme olasılığını gösterir. 

  Örneğin, üç S1 birimi (veya dakika giriş kapasitesi başına 2100 olay) sağlanmışsa, >= 1900 olayları için 2 saat boyunca Bir **Giriş S1** olayları uyarısı ayarlayabilirsiniz. Bu eşiği sürekli olarak aşıyorsanız ve bu nedenle uyarınızı tetiklerseniz, büyük olasılıkla yetersiz hüküm vermiş sinizdir.  

* Daraltıldığınızdan şüpheleniyorsanız, **Giriş Alınan İletilerinizi** olay kaynağınızın çıkış lı iletileriyle karşılaştırabilirsiniz.  Olay Hub'ınıza **giriş, Giriş Alınan İletilerinizden**daha büyükse, Zaman Serisi Öngörüleriniz büyük olasılıkla daraltılır.

## <a name="improving-performance"></a>Performansı artırma

Azaltmayı veya gecikme süresini azaltmak için, bunu düzeltmenin en iyi yolu ortamınızın kapasitesini artırmaktır.

Ortamınızı çözümlemek istediğiniz veri miktarı için düzgün bir şekilde yapılandırarak gecikme ve azaltmayı önleyebilirsiniz. Ortamınıza kapasite ekleme hakkında daha fazla bilgi için [ortamınızı ölçeklendir'i](time-series-insights-how-to-scale-your-environment.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

- Zaman [Serisi Öngörüleri ortamınızdaki sorunları tanılayın ve çözün.](time-series-insights-diagnose-and-solve-problems.md)

- [Zaman Serisi Öngörüleri ortamınızı nasıl ölçeklendireceklerini](time-series-insights-how-to-scale-your-environment.md)öğrenin.