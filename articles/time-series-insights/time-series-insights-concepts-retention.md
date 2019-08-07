---
title: Azure Time Series Insights ortamınızda veri bekletmesini anlayın | Microsoft Docs
description: Bu makalede, Azure Time Series Insights ortamınızda verilerin bekletilmesini denetleyen iki ayar açıklanmaktadır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 5388b157ebea78a69355eb745492910f260be3ad
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68823653"
---
# <a name="understand-data-retention-in-azure-time-series-insights"></a>Azure Time Series Insights veri saklama süresini anlama

Bu makalede, Azure Time Series Insights ortamınızda veri saklama işlemini etkileyen iki ayar açıklanmaktadır.

## <a name="video"></a>Video

### <a name="the-following-video-summarizes-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>Aşağıdaki videoda, veri saklama Time Series Insights ve bunun nasıl planlanacağı özetlenmektedir.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

Azure zaman serisi ortamlarınızın her biri, **veri saklama süresini**denetleyen bir ayara sahiptir. Değer 1 ile 400 gün arasında yayılır. Veriler ortam depolama kapasitesi veya Bekletme süresine göre silinir, hangisi önce gelir.

Ayrıca, Azure zaman serisi ortamınızda bir **depolama sınırı aşıldı davranış** ayarı vardır. Bir ortamın en fazla kapasitesine ulaşıldığında giriş ve temizleme davranışını denetler. Yapılandırma sırasında aralarından seçim yapabileceğiniz iki davranış vardır:

- **Eski verileri temizle** varsayılanını  
- **Girişi Duraklat**

> [!NOTE]
> Varsayılan olarak, yeni bir ortam oluştururken bekletme **eski verileri temizlemek**üzere yapılandırılır. Bu ayar, Time Series Insights ortamının **yapılandırma** sayfasında Azure Portal kullanılarak oluşturulma zamanından sonra gerektiği şekilde değiştirilebilir.

Bekletme davranışlarını değiştirme hakkında daha fazla bilgi için [Time Series Insights ' de bekletme yapılandırması](time-series-insights-how-to-configure-retention.md)' nı inceleyin.

Veri saklama davranışını karşılaştırın:

## <a name="purge-old-data"></a>Eski verileri temizleme

- Bu davranış, Time Series Insights ortamları için varsayılan davranıştır.  
- Bu davranış, kullanıcılar Time Series Insights ortamlarında *en son verileri* her zaman görmek istedikleri zaman tercih edilir.
- Bu davranış, ortamın sınırları (saklama süresi, boyutu veya sayısı, hangisi önce gelirse) aşıldığında verileri *temizler* . Bekletme varsayılan olarak 30 güne ayarlanır.
- En eski veriler önce temizlenir (FıFO yaklaşımı).

### <a name="example-one"></a>Örnek bir

Bekletme davranışına sahip örnek bir ortam düşünün **ve eski verileri temizleyin**:

**Veri saklama süresi** 400 gün olarak ayarlanır. **Kapasite** , 30 GB toplam kapasiteyi içeren S1 birimi olarak ayarlanır.   Gelen verileri ortalama günde 500 MB 'ye birikme edelim. Bu ortam, en yüksek kapasiteye 60 gün sonra ulaşıldığı için, gelen verilerin oranı verilen verilerin yalnızca 60 gün cinsinden kalmasını sağlayabilir. Gelen verilerin birikme biçimi: 500 MB her gün x 60 gün = 30 GB.

61 günde, ortam en güncel verileri gösterir, ancak 60 günden eski olan en eski verileri temizler. Temizleme işlemi yeni veri akışı için yer açar, böylece yeni veriler araştırılmaya devam edebilir. Kullanıcı verilerin daha uzun süre korumasını istiyorsa, ek birimler ekleyerek veya daha az veri gönderebilmeleri için ortamın boyutunu artırabilir.  

### <a name="example-two"></a>Örnek iki

Bir ortamı aynı zamanda yapılandırılmış saklama davranışını göz önünde bulundurun **ve eski verileri temizleyin**. Bu örnekte, **veri saklama süresi** 180 gün daha düşük bir değere ayarlanır. **Kapasite** , 30 GB toplam kapasiteyi içeren S1 birimi olarak ayarlanır. Günlük giriş, tam 180 güne ait verileri depolamak için günde 0,166 GB (166 MB) değerini aşamaz.  

Bu ortamın günlük giriş oranı günde 0,166 GB 'ı aştığında, bazı veriler temizlendiğinden veriler 180 gün süreyle depolanamaz. Meşgul zaman çerçevesinde aynı ortamı göz önünde bulundurun. Ortamın giriş hızının günde ortalama 0,189 GB 'a artırabileceğiniz varsayılır. Bu meşgul zaman çerçevesinde 158 günlük veriler korunur (30 GB/0.189 = 158,73 gün bekletme). Bu süre, istenen veri saklama süresi çerçevenden daha az.

## <a name="pause-ingress"></a>Duraklatma girişi

- Saklama süresi ayarı, boyut ve sayı sınırlarına bekletme dönemlerinden önce ulaşılırsa verilerin temizlenmemesini sağlamak üzere tasarlanmıştır.  
- Giriş **duraklatma** , bekletme döneminin ihlal olmasından dolayı verilerin temizlenmeden önce kullanıcıların ortamının kapasitesini artırmaları için ek süre sağlar
- Veri kaybından korunmanıza yardımcı olur ancak giriş, olay kaynağınızın bekletme döneminin ötesinde durakladığında en son verilerinizin kaybedilmesi için bir fırsat oluşturabilir.
- Ancak, bir ortama en fazla kapasiteye ulaşıldığında, aşağıdaki ek eylemler gerçekleşene kadar ortam veri girişini duraklatır:

   - [Time Series Insights ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md)bölümünde açıklandığı gibi daha fazla ölçek birimi eklemek için ortamın maksimum kapasitesini artırabilirsiniz.
   - Veri saklama süresine ulaşıldı ve veriler temizlendi ve ortam en yüksek kapasitesinin altına getiriliyor.

### <a name="example-three"></a>Örnek üç

Girişi **duraklatmak**için yapılandırılan bekletme davranışına sahip bir ortam düşünün. Bu örnekte, **veri saklama süresi** 60 gün olarak yapılandırılır. **Kapasite** , üç (3) S1 birimi olarak ayarlanır. Bu ortamın her gün 2 GB veri aldığını varsayın. Bu ortamda, en yüksek kapasiteye ulaşıldığında giriş duraklatılır.

Bu sırada, giriş devam etmeden veya **çalışmaya devam** edene kadar ortam aynı veri kümesini gösterir (yeni veriler için yer açmak üzere eski verileri temizler).

Giriş devam ettiğinde:

- Veri akışları olay kaynağı tarafından alındığı sırada
- Olay kaynağınızda bekletme ilkelerini aşmadığınız takdirde olaylar, zaman damgalarına göre dizinlenir. Olay kaynağı saklama yapılandırması hakkında daha fazla bilgi için [Event HUBS SSS](../event-hubs/event-hubs-faq.md)

> [!IMPORTANT]
> Giriş duraklamasını önlemeye yardımcı olmak için uyarılar ayarlamanız gerekir. Varsayılan saklama, Azure olay kaynakları için 1 gün olduğundan veri kaybı mümkündür. Bu nedenle, giriş duraklatıldıktan sonra ek eylem gerçekleşmediğiniz takdirde büyük olasılıkla en son verileri kaybedersiniz. Veri kaybını önlemek için kapasiteyi artırmanız veya **eski verileri temizlemek** için davranış geçişi yapmanız gerekir.

Etkilenen Event Hubs, Time Series Insights ' de duraklatma girişi gerçekleştiğinde veri kaybını en aza indirmek için **Ileti bekletme** özelliğini ayarlamayı göz önünde bulundurun.

[![Olay Hub 'ı ileti bekletme.](media/time-series-insights-contepts-retention/event-hub-retention.png)](media/time-series-insights-contepts-retention/event-hub-retention.png#lightbox)

Olay kaynağında (`timeStampPropertyName`) bir özellik yapılandırılmamışsa, Time Series Insights varsayılan olarak, Olay Hub 'ında X ekseni olarak gelen varışı zaman damgasına. , Başka bir şey olacak şekilde yapılandırıldıysa, bu ortam, olaylar ayrıştırıldığında `timeStampPropertyName` veri paketinde yapılandırılmış öğesine bakar. `timeStampPropertyName`

Ortamınızı ek kapasiteye uyacak şekilde ölçeklendirmeniz veya saklama süresini artırmanız gerekiyorsa, daha fazla bilgi için bkz. [Time Series Insights ortamınızı ölçeklendirme](time-series-insights-how-to-scale-your-environment.md) .  

## <a name="next-steps"></a>Sonraki adımlar

- Veri saklama ayarlarını yapılandırma veya değiştirme hakkında daha fazla bilgi için [Time Series Insights ' de bekletme yapılandırması](time-series-insights-how-to-configure-retention.md)' nı gözden geçirin.
