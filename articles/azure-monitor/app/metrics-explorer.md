---
title: Azure Uygulama Öngörülerinde Ölçümleri Keşfetmek | Microsoft Dokümanlar
description: Metrik explorer'daki grafiklerin nasıl yorumlanacağı ve metrik explorer bıçaklarının nasıl özelleştirileni.
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: f85a8fe79e7f4f820d7c0e5b942730305e892095
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275899"
---
# <a name="exploring-metrics-in-application-insights"></a>Uygulama Öngörülerinde Ölçümleri Keşfetme
[Uygulama Öngörüleri'ndeki][start] ölçümler, uygulamanızdan telemetri yle gönderilen olayların ölçülü değerleri ve sayısıdır. Bunlar, uygulamanızın nasıl kullanıldığına dair performans sorunlarını algılamanıza ve eğilimleri izlemenize yardımcı olur. Çok çeşitli standart ölçümler vardır ve kendi özel ölçümlerinizi ve etkinliklerinizi de oluşturabilirsiniz.

> [!NOTE]
> Bu makalede, şu anda amortismana kaldırılmış olan ve sonunda kullanımdan kaldırılacak olan klasik ölçümler explorer deneyimi açıklanmaktadır. Bu makalede açıklanan yeni deneyime dikkat etmenizi [öneririz.](../platform/metrics-charts.md)

Ölçümler ve olay sayıları, toplamlar, ortalamalar veya sayımlar gibi birleştirilmiş değerlerin grafiklerinde görüntülenir.

Aşağıda örnek bir grafik kümesi verem:

![](./media/metrics-explorer/01-overview.png)

Uygulama Öngörüleri portalında her yerde metrik grafikleri bulabilirsiniz. Çoğu durumda, bunlar özelleştirilebilir ve bıçak için daha fazla grafik ekleyebilirsiniz. Genel Bakış bıçağından, daha ayrıntılı grafiklere ("Sunucular" gibi **başlıklara** sahip) tıklayın veya özel grafikler oluşturabileceğiniz yeni bir bıçak açmak için Metrics Explorer'ı tıklatın.

## <a name="time-range"></a>Zaman aralığı
Herhangi bir bıçaküzerindeki grafiklerin veya ızgaraların kapsadığı Zaman aralığını değiştirebilirsiniz.

![Azure portalında uygulamanızın genel bakış bıçaklarını açın](./media/metrics-explorer/03-range.png)

Henüz görünmemiş bazı veriler bekliyorsanız, Yenile'yi tıklatın. Grafikler aralıklarla kendilerini yeniler, ancak aralıklar daha büyük zaman aralıkları için daha uzundur. Verilerin analiz boru hattından bir grafiğe gelmesi biraz zaman alabilir.

Grafiğin bir kısmını yakınlaştırmak için grafiğin üzerine sürükleyin:

![Grafiğin bir bölümüboyunca sürükleyin.](./media/metrics-explorer/12-drag.png)

Geri yüklemek için Yakınlaştır düğmesini geri a tıklayın.

## <a name="granularity-and-point-values"></a>Taneciklilik ve nokta değerleri
Bu noktada ölçümlerin değerlerini görüntülemek için farenizi grafiğin üzerine titretin.

![Fareyi bir grafiğin üzerine tover](./media/metrics-explorer/02-focus.png)

Belirli bir noktadaki ölçümün değeri, önceki örnekleme aralığında toplanır.

Numune alma aralığı veya "taneciklilik" bıçağın üst kısmında gösterilir.

![Bir bıçağın başlığı.](./media/metrics-explorer/11-grain.png)

Zaman aralığı bıçağındaki tanecikleri ayarlayabilirsiniz:

![Bir bıçağın başlığı.](./media/metrics-explorer/grain.png)

Kullanılabilir parçalı lıklar seçtiğiniz zaman aralığına bağlıdır. Açık tanecikler, zaman aralığı için "otomatik" tanecikliliğe alternatiflerdir.


## <a name="editing-charts-and-grids"></a>Grafikleri ve ızgaraları düzenleme
Bıçak için yeni bir grafik eklemek için:

![Metrikler Gezgini'nde Grafik Ekle'yi seçin](./media/metrics-explorer/04-add.png)

Ne gösterdiğini deletmek için varolan veya yeni bir grafikte **Edit'i** seçin:

![Bir veya daha fazla ölçüm seçin](./media/metrics-explorer/08-select.png)

Birlikte görüntülenebilen kombinasyonlarla ilgili kısıtlamalar olsa da, grafikte birden fazla metrik görüntüleyebilirsiniz. Bir metrik seçtiğiniz de, diğerlerinden bazıları devre dışı bırakılır.

Uygulamanıza [özel ölçümler][track] kodladıysanız (TrackMetric ve TrackEvent çağrıları) burada listelenir.

## <a name="segment-your-data"></a>Verilerinizi segmente
Bir metriyi özelliğe göre bölebilirsiniz - örneğin, farklı işletim sistemleri olan istemcilerin sayfa görünümlerini karşılaştırmak için.

Bir grafik veya ızgara seçin, gruplandırmayı açın ve gruplandırmaiçin bir özellik seçin:

![Gruplandırma'yı seçin, ardından Group By'de bir özellik seçin](./media/metrics-explorer/15-segment.png)

> [!NOTE]
> Gruplandırma kullandığınızda, Alan ve Çubuk grafik türleri yığılmış bir ekran sağlar. Bu, Toplama yönteminin Toplam olduğu durumlarda uygundur. Ancak toplama türü Ortalama olduğunda, Çizgi veya Izgara ekran türlerini seçin.
>
>

Uygulamanıza [özel ölçümler][track] kodlarsanız ve bunlar özellik değerlerini içeriyorsa, listedeki özelliği seçebilirsiniz.

Grafik, parçalı veriler için çok mu küçük? Yüksekliğini ayarlayın:

![Kaydırıcıyı ayarlama](./media/metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Toplama türleri
Varsayılan olarak yan taraftaki gösterge genellikle grafik dönemi boyunca toplanan değeri gösterir. Grafiğin üzerinde gezinirseniz, bu noktadadeğeri gösterir.

Grafikteki her veri noktası, önceki örnekleme aralığında veya "parçalı" olarak alınan veri değerlerinin toplamıdır. Parçalı lık bıçağın üst kısmında gösterilir ve grafiğin genel zaman ölçeğine göre değişir.

Ölçümler farklı şekillerde toplanabilir:

* **Sayım,** örnekleme aralığında alınan olayların sayısıdır. İstek gibi olaylar için kullanılır. Grafiğin yüksekliğindeki varyasyonlar, olayların oluşma hızındaki varyasyonları gösterir. Ancak örnekleme aralığını değiştirdiğinizde sayısal değerin değiştiğini unutmayın.
* **Özetle,** örnekleme aralığı veya grafik dönemi boyunca alınan tüm veri noktalarının değerlerini toplar.
* **Ortalama,** Toplamı aralıkta alınan veri noktalarının sayısına böler.
* **Benzersiz** sayımlar, kullanıcı ve hesap sayımları için kullanılır. Örnekleme aralığı nda veya grafik dönemi boyunca, şekil o zaman içinde görülen farklı kullanıcıların sayısını gösterir.
* **%**- Her toplama yüzdesi sürümleri yalnızca parçalı grafiklerle kullanılır. Toplam her zaman %100'e kadar gelir ve grafik, toplamın farklı bileşenlerinin göreli katkısını gösterir.

    ![Yüzde toplama](./media/metrics-explorer/percentage-aggregation.png)

### <a name="change-the-aggregation-type"></a>Toplama türünü değiştirme

![Grafiği edin ve ardından Toplama'yı seçin](./media/metrics-explorer/05-aggregation.png)

Her metrik için varsayılan yöntem, yeni bir grafik oluşturduğunuzda veya tüm ölçümler deselected olduğunda gösterilir:

![Varsayılanları görmek için tüm ölçümleri seçin](./media/metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>Y eksenini sabitleme 
Varsayılan olarak bir grafik, değerlerin kuantum görsel bir gösterimi vermek için, veri aralığında maksimum değerler sıfırdan başlayan Y ekseni değerleri gösterir. Ama bazı durumlarda kuantumdan daha fazla değerdeki küçük değişiklikleri görsel olarak incelemek ilginç olabilir. Bu gibi özelleştirmeler için Y ekseni aralığı düzenleme özelliğini kullanarak Y ekseninin minimum veya maksimum değerini istenilen yere sabitleyin.
Y ekseni aralığı Ayarlarını açmak için "Gelişmiş Ayarlar" onay kutusuna tıklayın

![Gelişmiş Ayarlar'ı tıklatın, Özel aralığı seçin ve min max değerlerini belirtin](./media/metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Verilerinizi filtreleme
Seçili özellik değerleri kümesiiçin yalnızca ölçümleri görmek için:

![Filtrele'yi tıklatın, özelliği genişletin ve bazı değerleri denetleyin](./media/metrics-explorer/19-filter.png)

Belirli bir özellik için herhangi bir değer seçmezseniz, bu, bunların hepsini seçmekle aynıdır: bu özellikte filtre yoktur.

Her özellik değerinin yanında olay sayısına dikkat edin. Bir özelliğin değerlerini seçtiğinizde, diğer özellik değerleriyle birlikte sayımlar ayarlanır.

Filtreler bir bıçak üzerindeki tüm grafiklere uygulanır. Farklı grafiklere farklı filtreler uygulanmasını istiyorsanız, farklı ölçümler bıçakları oluşturun ve kaydedin. İsterseniz, farklı bıçaklardan gösterge paneline grafikleri sabitleyebilirsiniz, böylece onları yan yana görebilirsiniz.

### <a name="remove-bot-and-web-test-traffic"></a>Bot ve web test trafiğini kaldırma
**Filtre Gerçek veya sentetik trafik** kullanın ve **Real**kontrol edin.

Ayrıca **sentetik trafik kaynağına**göre filtreleyebilirsiniz.

### <a name="to-add-properties-to-the-filter-list"></a>Filtre listesine özellik eklemek için
Kendi seçtiğiniz bir kategoride telemetri filtrelemek ister misiniz? Örneğin, kullanıcılarınızı farklı kategorilere bölmek ve verilerinizi bu kategorilere göre bölmek istersiniz.

[Kendi mülkünüzü oluşturun.](../../azure-monitor/app/api-custom-events-metrics.md#properties) Farklı SDK modülleri tarafından gönderilen standart telemetri de dahil olmak üzere tüm telemetrilerde görünmesi için bir [Telemetri Initializer'ine](../../azure-monitor/app/api-custom-events-metrics.md#defaults) ayarlayın.

## <a name="edit-the-chart-type"></a>Grafik türünü edin
Izgaralar ve grafikler arasında geçiş yapabileceğinize dikkat edin:

![Bir ızgara veya grafik seçin, ardından bir grafik türü seçin](./media/metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>Ölçüm bıçağınızı kaydedin
Bazı grafikler oluşturduğınızda, bunları sık kullanılan olarak kaydedin. Kuruluş hesabı kullanıyorsanız, bunu diğer ekip üyeleriyle paylaşıp paylaşmayacağınızı seçebilirsiniz.

![Sık Kullanılanı Seçin](./media/metrics-explorer/21-favorite-save.png)

Bıçağı tekrar görmek **için genel bakış bıçağına gidin** ve Sık Kullanılanlar'ı açın:

![Genel Bakış bıçağında Sık Kullanılanlar'ı seçin](./media/metrics-explorer/22-favorite-get.png)

Kaydettiğiniz zaman Göreli zaman aralığını seçtiyseniz, bıçak en son ölçümlerle güncelleştirilir. Mutlak zaman aralığını seçtiyseniz, her seferinde aynı verileri gösterir.

## <a name="reset-the-blade"></a>Bıçağı sıfırla
Bir bıçağı yeniden ayarlayıp orijinal kaydedilmiş sete geri dönmek istiyorsanız, Sıfırla'yı tıklatın.

![Metric Explorer'ın üst kısmındaki düğmelerde](./media/metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Canlı ölçümler akışı

Telemetrinizin çok daha hızlı bir görünümü için [Canlı Yayın'ı](live-stream.md)açın. Çoğu ölçümün görünmesi, toplama işlemi nedeniyle birkaç dakika sürer. Bunun aksine, canlı ölçümler düşük gecikme sonu için optimize edilebiyi gösterir. 

## <a name="set-alerts"></a>Uyarı ayarlama
Herhangi bir metnin olağandışı değerlerine e-posta ile bildirilmek için bir uyarı ekleyin. E-postayı hesap yöneticilerine veya belirli e-posta adreslerine göndermeyi seçebilirsiniz.

![Metrikler Gezgini'nde Uyarı kurallarını seçin, Uyarı Ekle](./media/metrics-explorer/appinsights-413setMetricAlert.png)

[Uyarılar hakkında daha fazla bilgi edinin.][alerts]


## <a name="continuous-export"></a>Sürekli İhracat
Verileri dışarıdan işleyebilirsiniz, böylece sürekli dışa aktarın istiyorsanız, [Sürekli dışa aktarma](../../azure-monitor/app/export-telemetry.md)yı kullanmayı düşünün.

### <a name="power-bi"></a>Power BI
Verilerinizin daha da zengin görünümlerini istiyorsanız, [Power BI'ye dışa aktarabilirsiniz.](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)

## <a name="analytics"></a>Analiz
[Analytics,](../../azure-monitor/app/analytics.md) güçlü bir sorgu dili kullanarak telemetrinizi analiz etmenin daha çok yönlü bir yoludur. Ölçümlerden elde edilen sonuçları birleştirmek veya hesaplamak veya uygulamanızın son performansını derinlemesine bir şekilde araştırmak istiyorsanız kullanın. 

Bir metrik grafikten, doğrudan eşdeğer Analytics sorgusuna ulaşmak için Analytics simgesini tıklatabilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme
*Grafiğimde veri göremiyorum.*

* Filtreler bıçaktaki tüm grafiklere uygulanır. Bir grafiğe odaklanırken, diğerindeki tüm verileri hariç tutan bir filtre ayarlamadığınızdan emin olun.

    Farklı grafiklerde farklı filtreler ayarlamak istiyorsanız, bunları farklı bıçaklarda oluşturun ve bunları ayrı sık kullanılanlar olarak kaydedin. İsterseniz, onları panoya sabitleyebilirsiniz, böylece onları yan yana görebilirsiniz.
* Bir grafiği metrikte tanımlanmamış bir özelliğe göre gruplarsanız, grafikte hiçbir şey olmaz. 'Grupla gruplandırmayı' deneyin veya farklı bir gruplandırma özelliği seçin.
* Performans verileri (CPU, IO oranı vb.) Java web hizmetleri, Windows masaüstü uygulamaları, [iIS web uygulamaları ve hizmetleri için kullanılabilir durumdadır, ve](../../azure-monitor/app/monitor-performance-live-website-now.md) [Azure Bulut Hizmetleri.](../../azure-monitor/app/app-insights-overview.md) Azure web siteleri için kullanılamaz.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama Öngörüleri ile kullanımı izleme](../../azure-monitor/app/usage-overview.md)
* [Tanılama Arama'yı Kullanma](../../azure-monitor/app/diagnostic-search.md)

<!--Link references-->

[alerts]: ../../azure-monitor/app/alerts.md
[start]: ../../azure-monitor/app/app-insights-overview.md
[track]: ../../azure-monitor/app/api-custom-events-metrics.md
