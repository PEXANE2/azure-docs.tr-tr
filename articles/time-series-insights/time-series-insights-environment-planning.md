---
title: GA ortamınızı planlayın - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Azure Time Series Insights GA ortamınızı hazırlamak, yapılandırmak ve dağıtmak için en iyi uygulamalar.
services: time-series-insights
ms.service: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/21/2020
ms.custom: seodec18
ms.openlocfilehash: 972bb2a804057037deedb448674abafcc175b21f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76314819"
---
# <a name="plan-your-azure-time-series-insights-ga-environment"></a>Azure Time Series Öngörüleri GA ortamınızı planlayın

Bu makalede, beklenen giriş hızınıza ve veri saklama gereksinimlerinize bağlı olarak Azure Zaman Serisi Öngörüleri (GA) ortamınızı nasıl planlayınız açıklanmaktadır.

## <a name="video"></a>Video

**Azure Time Series Insights'ta veri saklama ve nasıl planlaşılabilen ler hakkında daha fazla bilgi edinmek için bu videoyu izleyin:**<br /><br />

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>En iyi uygulamalar

Azure Zaman Serisi Öngörüleri'ne başlamak için, dakika ya da kaç veri itmenizi beklediğiniz ve verilerinizi ne kadar süreyle depolamanız gerektiğini bilmeniz en iyisidir.  

Hem Time Series Insights SKUs'lar için kapasite ve bekletme hakkında daha fazla bilgi için [Time Series Insights fiyatlandırmasını](https://azure.microsoft.com/pricing/details/time-series-insights/)okuyun.

Zaman Serisi Öngörüleri ortamınızı uzun vadeli başarı için en iyi şekilde planlamak için aşağıdaki özellikleri göz önünde bulundurun:

- [Depolama kapasitesi](#storage-capacity)
- [Veri saklama süresi](#data-retention)
- [Giriş kapasitesi](#ingress-capacity)
- [Etkinliklerinizi şekillendirme](#shape-your-events)
- [Referans verilerinin yerinde olmasını sağlama](#ensure-that-you-have-reference-data)

## <a name="storage-capacity"></a>Depolama kapasitesi

Varsayılan olarak, Time Series Insights, sağlama yaptığınız depolama miktarına (birim başına depolama miktarı &#215; birimler) ve girişe bağlı olarak verileri saklar.

## <a name="data-retention"></a>Veri saklama

Azure Zaman Serisi Öngörüleri ortamınızda **Veri saklama süresi** ayarını değiştirebilirsiniz. 400 güne kadar bekletme olanağı sağlayabilirsiniz. 

Azure Zaman Serisi Öngörüleri'nin iki modu vardır:

* Bir mod en güncel veriler için en iyi duruma getirir. Eski **verileri temizlemek** için bir ilke uygular ve son verileri örnekle birlikte kullanılabilir hale getirmek. Bu mod varsayılan olarak açıktır. 
* Diğeri, verileri yapılandırılan bekletme sınırlarının altında kalacak şekilde en iyi duruma getirir. **Duraklatma,** **Depolama sınırı davranış aşıldığında**seçildiğinde yeni verilerin girilmesini önler.

Azure portalında ortamın yapılandırma sayfasındaki iki mod arasında bekletme ve geçiş ayarlama yı ayarlayabilirsiniz.

> [!IMPORTANT]
> Azure Zaman Serisi Öngörüleri GA ortamınızda en fazla 400 günlük veri saklama yı yapılandırabilirsiniz.

### <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. Azure [portalında](https://portal.azure.com)Time Series Öngörüleri ortamınızı seçin.

1. Zaman **Serisi Öngörüler ortamı** bölmesinde, **Ayarlar**altında, **Depolama yapılandırmasını**seçin.

1. Veri **saklama süresine (gün içinde)** kutusuna 1 ile 400 arasında bir değer girin.

   [![Bekletme yapılandırma](media/data-retention/configure-data-retention.png)](media/data-retention/configure-data-retention.png#lightbox)

> [!TIP]
> Uygun bir veri saklama ilkesinin nasıl uygulanacağı hakkında daha fazla bilgi edinmek [için bekletme yapılandırmayı nasıl okuyabilirsiniz.](./time-series-insights-how-to-configure-retention.md)

## <a name="ingress-capacity"></a>Giriş kapasitesi

[!INCLUDE [Azure Time Series Insights GA limits](../../includes/time-series-insights-ga-limits.md)]

### <a name="environment-planning"></a>Çevre planlama

Time Series Insights ortamınızı planlamak için odaklanılabilmeniz gereken ikinci alan giriş kapasitesidir. Giriş kapasitesi, dakika başına ayırmanın bir türevidir.

Azaltma açısından bakıldığında, paket boyutu 32 KB olan bir girişli veri paketi, her biri 1 KB boyutunda 32 olay olarak kabul edilir. İzin verilen maksimum olay boyutu 32 KB'dir. 32 KB'den büyük veri paketleri kesilir.

Bir S1 veya S2 SKU'nun kapasitesini tek bir ortamda 10 üniteye yükseltebilirsiniz. S1 ortamından S2'ye geçiş yapamazsınız. Bir S2 ortamından S1'e geçiş yapamazsınız.

Giriş kapasitesi için, öncelikle aylık olarak ihtiyacınız olan toplam girişi belirleyin. Ardından, dakika başına ihtiyaçlarınızın ne olduğunu belirleyin. 

Azaltma ve gecikme dakika başına kapasitede rol oynar. Veri girişinizde 24 saatten az süren bir artış varsa, Time Series Insights önceki tabloda listelenen oranların iki katı bir giriş hızında "yetişebilir".

Örneğin, tek bir S1 SKU'nuz varsa, verileri dakikada 720 olay hızında girersiniz ve veri hızı 1.440 veya daha az bir oranda bir saatten daha kısa bir süre için yükselir, ortamınızda belirgin bir gecikme yoktur. Ancak, bir saatten fazla dakika başına 1.440 olayı aşarsanız, büyük olasılıkla görüntülenmiş ve ortamınızda sorgu için kullanılabilir verilerde gecikme deneyimi yaşarsınız.

Ne kadar veri itmeyi beklediğiniz önceden bilmiyor olabilirsiniz. Bu durumda, Azure portal aboneliğinizde [Azure IoT Hub'ı](../iot-hub/iot-hub-metrics.md) ve [Azure Etkinlik Hub'ları](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) için veri telemetrisini bulabilirsiniz. Telemetri, ortamınızı nasıl sağlayabileceğinizi belirlemenize yardımcı olabilir. İlgili etkinlik kaynağının telemetrisini görüntülemesi için Azure portalındaki **Ölçümler** bölmesini kullanın. Etkinlik kaynağı ölçümlerinizi anlıyorsanız, Zaman Serisi Öngörüleri ortamınızı daha etkili bir şekilde planlayabilir ve sağlayabilirsiniz.

### <a name="calculate-ingress-requirements"></a>Giriş gereksinimlerini hesaplama

Giriş gereksinimlerinizi hesaplamak için:

- Giriş kapasitenizin dakika başına ortalama hızınızın üzerinde olduğunu ve ortamınızın bir saatten daha kısa bir süre için kapasitenizin iki katına eşdeğer olan girişi kaldıracak kadar büyük olduğunu doğrulayın.

- 1 saatten uzun süren giriş ani artışları meydana gelirse, ortalama olarak başak hızını kullanın. Ani oranı işlemek için kapasiteye sahip bir ortam sağlama.

### <a name="mitigate-throttling-and-latency"></a>Azaltma azaltma ve gecikme

Azaltma ve gecikmeyi nasıl önleyeceğiniz hakkında bilgi için, [Azaltma gecikmesi ve azaltmayı](time-series-insights-environment-mitigate-latency.md)okuyun.

## <a name="shape-your-events"></a>Etkinliklerinizi şekillendirin

Olayları Time Series Insights'a gönderme şeklinizin, sağladığınız ortamın boyutunu desteklediğinden emin olmak önemlidir. (Tersine, zaman serisi öngörüleri kaç etkinlik okur ve her olayın boyutu için ortamın boyutunu eşleyebilirsiniz.) Verilerinizi sorgularken dilimlemek ve filtrelemek için kullanmak isteyebileceğiniz öznitelikleri de düşünmek önemlidir.

> [!TIP]
> Etkinlik Gönderme'de JSON şekillendirme belgelerini gözden [geçirin.](time-series-insights-send-events.md)

## <a name="ensure-that-you-have-reference-data"></a>Referans verilerine sahip olduğundan emin olun

*Başvuru veri kümesi,* olay kaynağınızdan olayları artıran öğeler topluluğudur. Zaman Serisi Öngörüler giriş motoru, etkinlik kaynağınızdan gelen her olayı referans veri setinizdeki ilgili veri satırıyla birleştirir. Artırılmış olay daha sonra sorgu için kullanılabilir. Birleştirme, başvuru veri setinizde tanımlanan **Birincil Anahtar** sütunlarını temel almaktadır.

> [!NOTE]
> Başvuru verileri geriye dönük olarak birleştirilemez. Yalnızca geçerli ve gelecekteki giriş verileri, yapılandırıldıktan ve yüklendikten sonra eşleştirilir ve başvuru veri kümesine katılır. Time Series Insights'a büyük miktarda geçmiş veri göndermeyi planlıyorsanız ve önce Time Series Insights'a referans verileri yüklemez veya oluşturmazsanız, çalışmanızı yeniden yapmanız gerekebilir (ipucu: eğlenceli değil).  

Zaman Serisi Öngörüleri'nde referans verilerinizi nasıl oluşturup yükleyip yönetecekleriniz hakkında daha fazla bilgi edinmek için [Referans veri seti belgelerimizi](time-series-insights-add-reference-data-set.md)okuyun.

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalında yeni bir Time Series Öngörüleri ortamı oluşturarak](time-series-insights-get-started.md)başlayın.

- Zaman Serisi [Öngörüleri'ne Etkinlik Hub'ları etkinlik kaynağını nasıl ekleyeceğinizi](time-series-insights-how-to-add-an-event-source-eventhub.md) öğrenin.

- [Bir IoT Hub olay kaynağını nasıl yapılandırılabildiğini](time-series-insights-how-to-add-an-event-source-iothub.md)okuyun.
