---
title: Ortamınızdaki veri saklamayı anlama - Azure Time Series Insight | Microsoft Dokümanlar
description: Bu makalede, Azure Zaman Serisi Öngörüleri ortamınızda veri saklamayı kontrol eden iki ayar açıklanmaktadır.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/10/2020
ms.custom: seodec18
ms.openlocfilehash: 34cf1e91b1fe5aae516c77bf2c280dfe70000611
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894748"
---
# <a name="understand-data-retention-in-azure-time-series-insights"></a>Azure Zaman Serisi Öngörüleri'nde veri saklamayı anlama

Bu makalede, Azure Zaman Serisi Öngörüleri ortamınızda veri saklamayı etkileyen iki birincil ayar açıklanmaktadır.

## <a name="video"></a>Video

### <a name="the-following-video-summarizes-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>Aşağıdaki videoda Time Series Insights veri saklama ve bunun nasıl planlanması özetlenmiştir.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

Azure Zaman Serisi Öngörüleri ortamlarınızın her birinde **Veri saklama süresini**kontrol eden bir ayar vardır. Değer 1 ile 400 güne kadar uzanır. Veriler, hangisi önce gerçekleşirse, ortam depolama kapasitesine veya bekletme süresine göre silinir.

Ayrıca, Azure Zaman Serisi Öngörüleri ortamınızda davranış ayarı **aşıldı** bir Depolama sınırı vardır. Bir ortamın maksimum kapasitesine ulaşıldığında giriş ve temizleme davranışını denetler. Yapılandırırken seçebileceğiniz iki davranış vardır:

- **Eski verileri temizleme** (varsayılan)  
- **Duraklatma girişi**

> [!NOTE]
> Varsayılan olarak, yeni bir ortam oluştururken, bekletme **eski verileri temizlemek**için yapılandırılır. Bu ayar, Zaman Serisi Öngörüleri ortamının **Yapılandırma** sayfasında, Azure portalı kullanılarak oluşturma süresinden sonra gerektiği gibi değiştirilebilir.
> * Bekletme ilkelerinin nasıl yapılandırılabildiğini öğrenmek [için, Zaman Serisi Öngörülerinde Yapılandırma bekletme'yi](time-series-insights-how-to-configure-retention.md)okuyun.

Her iki veri saklama ilkesi aşağıda daha ayrıntılı olarak açıklanmıştır.

## <a name="purge-old-data"></a>Eski verileri temizleme

- **Eski verileri temizleme,** Azure Zaman Serisi Öngörüleri ortamları için varsayılan ayardır.  
- Kullanıcılar zaman serisi öngörüleri ortamında her zaman *en son verilerine* sahip olmak istediklerinde **eski verileri temizleme** tercih edilir.
- **Temizleme eski veri** ayarı, ortamın sınırlarına (saklama süresi, boyut veya sayım, hangisi önce gelirse) ulaşıldıktan sonra verileri *temizler.* Bekletme varsayılan olarak 30 gün olarak ayarlanır.
- En eski alınan veriler ilk olarak temizlenir ("İlk Çıkışta İlk" yaklaşımı).

### <a name="example-one"></a>Örnek bir

Bekletme davranışı yla örnek bir ortam düşünün **Eski verileri temizlemeye devam edin ve temize alın:**

**Veri saklama süresi** 400 güne ayarlanır. **Kapasite,** toplam 30 GB kapasite içeren S1 ünitesine ayarlanır. Gelen verilerin her gün ortalama 500 MB'a topolduğunu varsayalım. Maksimum kapasiteye 60 gün ulaşıldığı için, gelen veri oranı göz önüne alındığında, bu ortam yalnızca 60 günlük veri tutabilir. Gelen veriler şu şekilde birikir: 500 MB her gün x 60 gün = 30 GB.

61. günde, ortam en taze verileri gösterir, ancak 60 günden eski en eski verileri temizler. Temizleme, yeni verilerin keşfedilmeyi sürdürmesi için yeni veri akışına yer sağlar. Kullanıcı verileri daha uzun süre saklamak isterse, ek birimler ekleyerek ortamın boyutunu artırabilir veya daha az veri itebilir.  

### <a name="example-two"></a>Örnek iki

Bir ortam da yapılandırılmış bekletme davranışı düşünün **Devam ve eski verileri temizleme**. Bu örnekte, **Veri saklama süresi** 180 gün daha düşük bir değere ayarlanır. **Kapasite,** toplam 30 GB kapasite içeren S1 ünitesine ayarlanır. Tam 180 gün veri depolamak için günlük giriş günlük 0,166 GB'ı (166 MB) geçemez.  

Bu ortamın günlük giriş hızı günde 0,166 GB'ı aştığında, bazı veriler tasfiye edildiğinden veriler 180 gün boyunca depolanamaz. Yoğun bir zaman dilimi içinde bu ortamı göz önünde bulundurun. Ortamın giriş hızının günde ortalama 0,189 GB'a yükselebileceğini varsayalım. Bu yoğun zaman diliminde, yaklaşık 158 günlük veri korunur (30GB/0.189 = 158.73 gün bekletme). Bu süre istenen veri saklama zaman diliminden daha azdır.

## <a name="pause-ingress"></a>Duraklatma girişi

- **Duraklatma** ayarı, bekletme sürelerinden önce boyut ve sayım sınırlarına ulaşılırsa verilerin temizlenmemesini sağlamak için tasarlanmıştır.  
- **Duraklatma,** kullanıcıların bekletme süresinin ihlali nedeniyle veriler temizlenmeden önce ortamlarının kapasitesini artırmaları için ek süre sağlar.
- Veri kaybından korunmanıza yardımcı olur, ancak giriş olay kaynağınızın bekletme süresinin ötesinde duraklatılırsa en son verilerinizin kaybı için bir fırsat oluşturabilir.
- Ancak, bir ortamın maksimum kapasitesine ulaşıldıktan sonra, ortam aşağıdaki ek eylemler gerçekleşene kadar veri girişini duraklatır:

   - [Zaman Serisi Öngörüler ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md)de açıklandığı gibi daha fazla ölçek birimi eklemek için ortamın maksimum kapasitesini artırırsınız.
   - Veri saklama süresine ulaşılır ve veriler temizlenir ve ortam maksimum kapasitesinin altına iner.

### <a name="example-three"></a>Örnek üç

Girişi **duraklatmak**için yapılandırılmış bekletme davranışı olan bir ortam düşünün. Bu örnekte, **Veri saklama süresi** 60 güne yapılandırılmıştır. **Kapasite** üç (3) adet S1 birimine ayarlanır. Bu ortamın her gün 2 GB veri girişi olduğunu varsayalım. Bu ortamda, maksimum kapasiteye ulaşıldıktan sonra giriş duraklatılır.

O zaman, ortam giriş devam edene veya **devam girişi** etkinleştirilene kadar aynı veri kümesini gösterir (bu da eski verileri yeni verilere yer açmak için temizler).

Giriş devam ettiğinde:

- Olay kaynağı tarafından alındığı sırada veri akışları
- Olay kaynağınızdaki bekletme ilkelerini aşmadığınız sürece, olaylar zaman damgası esas alınca dizine alınır. Olay kaynağı tutma yapılandırması hakkında daha fazla bilgi için, [Olay Hub'ları SSS](../event-hubs/event-hubs-faq.md)

> [!IMPORTANT]
> Girişin duraklatılmamasını önlemek için bildirim de verecek uyarılar ayarlamanız gerekir. Varsayılan bekletme Azure olay kaynakları için 1 gün olduğundan veri kaybı mümkündür. Bu nedenle, giriş duraklatıldıktan sonra, ek eylem yapılmadığı sürece büyük olasılıkla en son verileri kaybedersiniz. Veri kaybı potansiyelini önlemek için **eski verileri temizlemek** için kapasiteyi artırmanız veya davranışı değiştirmeniz gerekir.

Etkilenen Olay Hub'larında, Zaman Serisi Öngörüleri'nde duraklama girişi gerçekleştiğinde veri kaybını en aza indirmek için **İleti Bekletme** özelliğini ayarlamayı düşünün.

[![Olay hub iletisi bekletme.](media/time-series-insights-concepts-retention/event-hub-retention.png)](media/time-series-insights-concepts-retention/event-hub-retention.png#lightbox)

Olay kaynağında hiçbir özellik yapılandırılmamışsa (`timeStampPropertyName`), Zaman Serisi Öngörüleri varsayılan olarak x ekseni olarak olay hub'ındaki varış zaman damgasına dönüşür. `timeStampPropertyName` Başka bir şey olarak yapılandırılırsa, olaylar `timeStampPropertyName` ayrıştırıldığında ortam veri paketinde yapılandırılan yapıyı arar.

[Zaman Serisi Öngörüler ortamınızı](time-series-insights-how-to-scale-your-environment.md) ek kapasiteye uyacak veya bekletme süresini artırmak için ortamınızı ölçeklendirmek için nasıl ölçeklendirilir'i okuyun.

## <a name="next-steps"></a>Sonraki adımlar

- Veri saklama ayarlarını yapılandırma veya değiştirme hakkında bilgi [için, Zaman Serisi Öngörülerinde bekletme yapılandırmayı](time-series-insights-how-to-configure-retention.md)gözden geçirin.

- Azure [Zaman Serisi Öngörüleri'nde gecikme süresini azaltma](time-series-insights-environment-mitigate-latency.md)hakkında bilgi edinin.
