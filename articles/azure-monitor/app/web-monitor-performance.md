---
title: Uygulama Öngörüleri ile uygulamanızın durumunu ve kullanımını izleyin
description: Uygulama Öngörüleri ile başlayın. Şirket içi veya Microsoft Azure uygulamalarınızın kullanımını, kullanılabilirliğini ve performansını analiz edin.
ms.topic: conceptual
ms.date: 05/10/2018
ms.reviewer: sdash
ms.openlocfilehash: 873fc41585c387246d83008a8f97d6c4d9a32c3b
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985074"
---
# <a name="monitor-performance-in-web-applications"></a>Web uygulamalarının performansını izleme


Uygulamanızın iyi performans gösterdiğinden emin olun ve hataları hızlı bir şekilde öğrenin. [Uygulama Öngörüleri][start] size performans sorunları ve özel durumları hakkında bilgi verir ve temel nedenleri bulmanıza ve tanılamanıza yardımcı olur.

Application Insights hem Java'yı hem de web uygulamaları ve hizmetlerini, WCF hizmetlerini ASP.NET izleyebilir. Şirket içinde, sanal makinelerde veya Microsoft Azure web siteleri olarak barındırılabilirler. 

İstemci tarafında, Application Insights web sayfalarından ve iOS, Android ve Windows Mağazası uygulamaları da dahil olmak üzere çok çeşitli cihazlardan telemetri alabilir.

## <a name="set-up-performance-monitoring"></a><a name="setup"></a>Performans izleme yi ayarlama
Projenize henüz Uygulama Öngörüleri eklemediyseniz (diğer bir şekilde ApplicationInsights.config yoksa), başlamak için şu yollardan birini seçin:

* [ASP.NET web uygulamaları](../../azure-monitor/app/asp-net.md)
  * [Özel durum izleme ekleme](../../azure-monitor/app/asp-net-exceptions.md)
  * [Bağımlılık izleme ekleme](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Java EE web uygulamaları](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent)

## <a name="exploring-performance-metrics"></a><a name="view"></a>Performans ölçümlerini keşfetme
[Azure portalında,](https://portal.azure.com)uygulamanız için ayarladığınız Application Insights kaynağına göz atın. Genel bakış bıçağı temel performans verilerini gösterir:

Daha fazla ayrıntı ve daha uzun bir süre için sonuçları görmek için herhangi bir grafiği tıklatın. Örneğin, İstekler döşemesini tıklatın ve ardından bir zaman aralığı seçin:

![Daha fazla veri için tıklayın ve bir zaman aralığı seçin](./media/web-monitor-performance/appinsights-48metrics.png)

Hangi ölçümleri görüntüleyeceğini seçmek için bir grafiği tıklatın veya yeni bir grafik ekleyin ve ölçümlerini seçin:

![Ölçümleri seçmek için grafiği tıklatın](./media/web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> Kullanılabilir seçimin tamamını görmek için **tüm ölçümlerin denetimini kaldırın.** Ölçümler gruplara ayrılır; bir grubun herhangi bir üyesi seçildiğinde, yalnızca o grubun diğer üyeleri görünür.

## <a name="what-does-it-all-mean-performance-tiles-and-reports"></a><a name="metrics"></a>Bütün bunlar ne anlama geliyor? Performans kutucukları ve raporları
Alabileceğiniz çeşitli performans ölçümleri vardır. Uygulama bıçağında varsayılan olarak görünenlerle başlayalım.

### <a name="requests"></a>İstekler
Belirli bir dönemde alınan HTTP isteklerinin sayısı. Yük değişirken uygulamanızın nasıl hissettiğini görmek için bunu diğer raporlardaki sonuçlarla karşılaştırın.

HTTP istekleri, sayfalar, veriler ve resimler için tüm GET veya POST isteklerini içerir.

Belirli URL'ler için sayımlar almak için döşemeyi tıklatın.

### <a name="average-response-time"></a>Ortalama yanıt süresi
Uygulamanızı giren bir web isteği ile yanıtın döndürülen yanıtı arasındaki süreyi ölçer.

Puanlar hareketli bir ortalama gösteriyor. Çok sayıda istek varsa, grafikte belirgin bir tepe noktası veya daldırma olmadan ortalamadan sapan bazı olabilir.

Sıra dışı zirvelere bakın. Genel olarak, isteklerde bir artış ile yanıt süresi nin artmasını bekleyin. Artış orantısızsa, uygulamanız CPU veya kullandığı bir hizmetin kapasitesi gibi bir kaynak sınırına çarpabilir.

Belirli URL'ler için zaman almak için döşemeyi tıklatın.

![](./media/web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>En yavaş istekler
![](./media/web-monitor-performance/appinsights-44slowest.png)

Hangi isteklerin performans atonuna gerek duyabileceğini gösterir.

### <a name="failed-requests"></a>Başarısız istekler
![](./media/web-monitor-performance/appinsights-46failed.png)

Yakalanmayan özel durumlar atan istek sayısı.

Belirli hataların ayrıntılarını görmek için döşemeyi tıklatın ve ayrıntılarını görmek için tek bir istek seçin. 

Hataların yalnızca temsili bir örneği bireysel denetim için saklanır.

### <a name="other-metrics"></a>Diğer ölçümler
Hangi ölçümleri görüntüleyebilirsiniz görmek için bir grafiği tıklatın ve ardından kullanılabilir tam kümeyi görmek için tüm ölçümleri seçin. Her ölçümün tanımını görmek için (i) tuşuna tıklayın.

![Tüm kümeyi görmek için tüm ölçümleri seçin](./media/web-monitor-performance/appinsights-62allchoices.png)

Herhangi bir metrik seçilmesi, aynı grafikte görünamayan diğer lerini devre dışı kılabilir.

## <a name="set-alerts"></a>Uyarı ayarlama
Herhangi bir metnin olağandışı değerlerine e-posta ile bildirilmek için bir uyarı ekleyin. E-postayı hesap yöneticilerine veya belirli e-posta adreslerine göndermeyi seçebilirsiniz.

![](./media/web-monitor-performance/appinsights-413setMetricAlert.png)

Kaynağı diğer özelliklerden önce ayarlayın. Performans veya kullanım ölçümlerine ilişkin uyarıları ayarlamak istiyorsanız webtest kaynaklarını seçmeyin.

Eşik değerini girmeniz istenen birimleri not almaya dikkat edin.

*Uyarı Ekle düğmesini göremiyorum.* - Bu, salt okunur erişime sahip olduğunuz bir grup hesabı mı? Hesap yöneticisine danışın.

## <a name="diagnosing-issues"></a><a name="diagnosis"></a>Sorunları tanılama
Performans sorunlarını bulmak ve tanılamanız için birkaç ipucu aşağıda verilmiştir:

* Web siteniz inip yanlış veya yavaş yanıt verirse uyarılacak [web testlerini][availability] ayarlayın. 
* Hataların veya yavaş yanıtın yükle ilişkili olup olmadığını görmek için İstek sayısını diğer ölçümlerle karşılaştırın.
* Sorunları belirlemenize yardımcı olmak için kodunuzda [izleme ifadeleri ekleyin ve arayın.][diagnostic]
* [Canlı Ölçümler Akışı][livestream]ile Web uygulamanızı işletimde izleyin.
* [Snapshot Debugger][snapshot]ile .NET uygulamanızın durumunu yakalayın.

## <a name="find-and-fix-performance-bottlenecks-with-performance-investigation-experience"></a>Performans araştırması deneyimiyle performans darboğazlarını bulma ve düzeltme

Web uygulamanızda yavaş performans gösteren işlemleri incelemek için performans araştırması deneyimini kullanabilirsiniz. Belirli bir yavaş işlemi hızlı bir şekilde seçebilir ve yavaş işlemlerin koda indirgenmesine neden olmak için [Profiler'ı](../../azure-monitor/app/profiler.md) kullanabilirsiniz. Seçili işlem için gösterilen yeni süre dağılımını kullanarak, bir bakışta hızlı bir şekilde müşterileriniz için deneyimin ne kadar kötü olduğunu değerlendirebilirsiniz. Her yavaş işlem için kaç kullanıcı etkileşiminizin etkilendiğini görebilirsiniz. Aşağıdaki örnekte, GET Customers/Details işlemiyle ilgili deneyime daha yakından bakmaya karar verdik. Süre dağılımında, üç artış olduğunu görebiliriz. Leftmost başak yaklaşık 400 ms ve büyük duyarlı bir deneyim temsil eder. Orta başak yaklaşık 1.2 s ve vasat bir deneyim temsil eder. Son olarak 3.6'larda, müşterilerimizin memnun kalmalarına neden olabilecek yüzde 99'luk deneyimi temsil eden küçük bir artış daha var. Bu deneyim, aynı operasyon için büyük deneyimden on kat daha yavaştır. 

![GET Müşteriler/ Ayrıntılar üç süre ani](./media/web-monitor-performance/PerformanceTriageViewZoomedDistribution.png)

Bu işlem için kullanıcı deneyimlerini daha iyi anlamak için daha geniş bir zaman aralığı seçebiliriz. Daha sonra, işlemin yavaş olduğu belirli bir zaman penceresinde de zaman dilimini daraltabiliriz. Aşağıdaki örnekte, varsayılan 24 saat aralığından 7 günlük zaman aralığına geçtik ve 12.si ile 13'üncü Su arasındaki 9:47 ile 12:47 zaman aralığına yakınlaştırdık. Hem süre dağılımı hem de örnek ve profilci izleme sayısı sağda güncellenmiştir.

![Get Customers/Details bir zaman penceresi ile 7 günlük aralıkta üç süre ani](./media/web-monitor-performance/PerformanceTriageView7DaysZoomedTrend.png)

Yavaş deneyimleri daraltmak için, bir sonraki 95 ve 99 yüzdelik arasında düşen süreleri yakınlaştırmak. Bunlar, yavaş olan kullanıcı etkileşimlerinin %4'ünü temsil eder.

![Get Customers/Details bir zaman penceresi ile 7 günlük aralıkta üç süre ani](./media/web-monitor-performance/PerformanceTriageView7DaysZoomedTrendZoomed95th99th.png)

Artık Profilci izlemeleri düğmesine tıklayarak, Örnekler düğmesine veya temsilci profil oluşturucu izlerine tıklayarak temsili örneklere bakabiliriz. Bu örnekte, ilgi alanı ve aralık süresi içinde GET Customers/Details için toplanan dört izleme vardır.

Bazen sorun kodunuzda değil, kodlarınızın aradığı bir bağımlılıkta olur. Bu tür yavaş bağımlılıkları araştırmak için performans triaj görünümünde Bağımlılıklar sekmesine geçebilirsiniz. Varsayılan olarak performans görünümü ortalamaları eğilim, ancak gerçekten bakmak istediğiniz 95 yüzdelik (veya 99, durumda olgun bir hizmet izliyor). Aşağıdaki örnekte, PUT fabrikamaccount adını verdiğimiz yavaş Azure BLOB bağımlılığına odaklandık. Aynı bağımlılık için yavaş aramalar üç kat daha yavaş iken, 120 ms etrafında kümeleme, 40 ms etrafında iyi deneyimler kümeleme. İlgili işlemin belirgin bir şekilde yavaşlamasına neden olmak için bu çağrıların çoğunu niçin eklemeniz gerekmiyor. Tıpkı İşlemler sekmesinde yaptığınız gibi temsili örnekleri ve profil oluşturucu izlerini delip izleyebilirsiniz.

![Get Customers/Details bir zaman penceresi ile 7 günlük aralıkta üç süre ani](./media/web-monitor-performance/SlowDependencies95thTrend.png)

Performans araştırması deneyimi, odaklanmaya karar verdiniz örnek setin yanı sıra alakalı öngörüler de gösterir. Mevcut tüm öngörülere bakmanın en iyi yolu 30 günlük bir zaman aralığına geçmek ve ardından son bir aydaki tüm işlemlerde öngörüleri görmek için Genel'i seçmektir.

![Get Customers/Details bir zaman penceresi ile 7 günlük aralıkta üç süre ani](./media/web-monitor-performance/Performance30DayOveralllnsights.png)


## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar
[Web testleri][availability] - Dünyanın dört bir yanından düzenli aralıklarla uygulamanız için web istekleri gönderilmesini sağlar.

[Tanılama izlemelerini yakalayın ve arayın][diagnostic] - Sorunları saptamak için izleme çağrıları ekleyin ve sonuçları elemek.

[Kullanım izleme][usage] - İnsanların uygulamanızı nasıl kullandığını öğrenin.

[Sorun Giderme][qna] - ve Q & A



<!--Link references-->

[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[greenbrown]: ../../azure-monitor/app/asp-net.md
[qna]: ../../azure-monitor/app/troubleshoot-faq.md
[redfield]: ../../azure-monitor/app/monitor-performance-live-website-now.md
[start]: ../../azure-monitor/app/app-insights-overview.md
[usage]: usage-overview.md
[livestream]: ../../azure-monitor/app/live-stream.md
[snapshot]: ../../azure-monitor/app/snapshot-debugger.md



