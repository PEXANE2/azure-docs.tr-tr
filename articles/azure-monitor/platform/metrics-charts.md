---
title: Azure Ölçüm Gezgini’nin gelişmiş özellikleri
description: Azure Monitör Ölçümleri Gezgini'nin gelişmiş özellikleri hakkında bilgi edinin
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 2df1e0bb7d586edb13dc86e163f0e5728608d2a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371595"
---
# <a name="advanced-features-of-azure-metrics-explorer"></a>Azure Ölçüm Gezgini’nin gelişmiş özellikleri

> [!NOTE]
> Bu makalede, Ölçümler Explorer'ın temel özelliklerini bildiğinizi varsayar. Yeni bir kullanıcıysanız ve ilk metrik grafiğinizi nasıl oluşturacağınızhakkında bilgi edinmek istiyorsanız, [Azure Ölçümleri Gezgini ile başlarken](metrics-getting-started.md)bkz.

## <a name="metrics-in-azure"></a>Azure'da Ölçümler

[Azure Monitor'daki ölçümler,](data-platform-metrics.md) zaman içinde toplanan ve depolanan ölçülen değerler ve sayımlar dizisidir. Standart (veya "platform") ölçümleri ve özel ölçümler vardır. Standart ölçümler size Azure platformunun kendisi tarafından sağlanır. Standart ölçümler, Azure kaynaklarınızın sistem durumu ve kullanım istatistiklerini yansıtır. Özel ölçümler uygulamalarınız tarafından Azure'a özel [olaylar ve ölçümler için Uygulama Öngörüleri API'si](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics), [Windows Azure Tanılama (WAD) uzantısı](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview)veya Azure Monitor REST [API](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-store-custom-rest-api)kullanılarak gönderilir.

## <a name="create-views-with-multiple-metrics-and-charts"></a>Birden çok ölçüm ve grafikle görünüm oluşturma

Birden çok metrik satırı çizen veya aynı anda birden çok metrik grafik gösteren grafikler oluşturabilirsiniz. Bu işlevsellik şunları yapmanızı sağlar:

- bir değerin diğerinle nasıl ilişkili olduğunu görmek için aynı grafikteki ilgili ölçümleri ilişkilendirin
- yakın mesafede farklı ölçü birimleriile görüntüleme ölçümleri
- birden çok kaynaktan gelen ölçümleri görsel olarak toplama ve karşılaştırma

Örneğin, 5 depolama hesabınız varsa ve bunlar arasında ne kadar toplam alan tüketildiğinizi bilmek istiyorsanız, belirli noktalardaki tüm değerlerin tek tek ve toplamını gösteren (yığılmış) bir alan grafiği oluşturabilirsiniz.

### <a name="multiple-metrics-on-the-same-chart"></a>Aynı grafikte birden çok ölçüm

İlk olarak, [yeni bir grafik oluşturun.](metrics-getting-started.md#create-your-first-metric-chart) **Metrik Ekle'yi** tıklatın ve aynı grafiğe başka bir metrik eklemek için adımları yineleyin.

   > [!NOTE]
   > Genellikle farklı ölçü birimlerine (örneğin"milisaniye" ve "kilobaytlar") veya bir grafikte önemli ölçüde farklı ölçekte ölçümler emerek olmasını istemezsinuz. Bunun yerine, birden çok grafik kullanmayı düşünün. Metrikler gezgininde birden çok grafik oluşturmak için Grafik Ekle düğmesini tıklatın.

### <a name="multiple-charts"></a>Birden çok grafik

Grafik **Ekle'yi** tıklatın ve farklı bir metrikle başka bir grafik oluşturun.

### <a name="order-or-delete-multiple-charts"></a>Birden çok grafiği sipariş edin veya silin

Birden çok grafiği sıralamak veya silmek için, grafik menüsünü açmak için elips ( **...** ) simgesine tıklayın ve **Yukarı Taşı**, Aşağı **Taşı**veya **Sil**'in uygun menü öğesini seçin.

## <a name="apply-filters-to-charts"></a>Filtreleri grafiklere uygulama

Ölçüleri ölçülerle gösteren grafiklere filtreler uygulayabilirsiniz. Örneğin, "Hareket sayısı" ölçütü, işlemlerden gelen yanıtın başarılı veya başarısız olup olmadığını gösteren "Yanıt türü" boyutuna sahipse, bu boyutta filtreleme yalnızca başarılı (veya yalnızca başarısız) hareketler için bir grafik satırı çizer. 

### <a name="to-add-a-filter"></a>Filtre eklemek için

1. Grafiğin üstüne **filtre ekle'yi** seçin

2. Filtrelemek istediğiniz boyutu (özelliği) seçin

   ![ölçüm resmi](./media/metrics-charts/00006.png)

3. Grafiği çizerken hangi boyut değerlerini eklemek istediğinizi seçin (bu örnekte başarılı depolama hareketlerini filtreleme gösterilmektedir):

   ![ölçüm resmi](./media/metrics-charts/00007.png)

4. Filtre değerlerini seçtikten sonra, kapatmak için Filtre Seçici'den uzağa tıklayın. Şimdi grafik kaç depolama işleminin başarısız olduğunu gösterir:

   ![ölçüm resmi](./media/metrics-charts/00008.png)

5. Aynı grafiklere birden çok filtre uygulamak için 1-4 adımlarını yineleyebilirsiniz.



## <a name="apply-splitting-to-a-chart"></a>Grafiğe bölme uygulama

Ölçümün farklı segmentlerinin birbiriyle nasıl karşılaştırın ve bir boyutun dış kesimlerini tanımlamak için bir ölçüciyi boyuta bölebilirsiniz.

### <a name="apply-splitting"></a>Bölme uygula

1. Grafiğin üzerine **Uygula'yı** tıklatın.
 
   > [!NOTE]
   > Bölme, birden çok ölçüme sahip grafiklerle kullanılamaz. Ayrıca, birden çok filtre niz olabilir, ancak herhangi bir tek grafiğe yalnızca bir bölme boyutu uygulanır.

2. Grafiğinizi segmente etmek istediğiniz bir boyut seçin:

   ![ölçüm resmi](./media/metrics-charts/00010.png)

   Şimdi grafik şimdi birden çok satır, boyut her segment için bir gösterir:

   ![ölçüm resmi](./media/metrics-charts/00012.png)

3. Kapatmak için **Seçici Gruplandırma'dan** uzaklaşın.

   > [!NOTE]
   > Senaryonuz için alakasız olan bölümleri gizlemek ve grafiklerin okunmasını kolaylaştırmak için hem Filtreleme hem de Bölme'yi aynı boyutta kullanın.

## <a name="lock-boundaries-of-chart-y-axis"></a>Grafik y ekseninin kilit sınırları

Grafik daha büyük değerlerin daha küçük dalgalanmaları gösterdiğinde y ekseninin aralığını kilitlemek önemli hale gelir. 

Örneğin, başarılı isteklerin hacmi %99,99'dan %99,5'e düştüğünde, hizmet kalitesinde önemli bir azalma olabilir. Ancak, küçük bir sayısal değer dalgalanması fark etmek varsayılan grafik ayarlarından zor ve hatta imkansız olacaktır. Bu durumda grafiğin en düşük sınırını %99'a kilitleyebilir, bu da bu küçük düşüşü daha belirgin hale getirebilir. 

Başka bir örnek, kullanılabilir bellekteki bir dalgalanmadır ve değer teknik olarak asla 0'a ulaşamaz. Aralığın daha yüksek bir değere sabitlenmesi, kullanılabilir bellekteki düşüşlerin fark edilebilebilir olmasını kolaylaştırabilir. 

y eksenaralığını denetlemek için "..." grafik menüsünü seçin ve gelişmiş grafik ayarlarına erişmek için **grafiği Edit'i** seçin. Y Eksen Aralığı bölümündeki değerleri değiştirin veya varsayılan değerlere dönmek için **Otomatik** düğmesini kullanın.

![ölçüm resmi](./media/metrics-charts/00014-manually-set-granularity.png)

> [!WARNING]
> Belirli bir süre içinde çeşitli sayıları veya toplamları izleyen (ve böylece sayım, toplam, minimum veya maksimum toplamaları kullanan) grafikler için y ekseninin sınırlarını kilitlemek genellikle otomatik varsayılanlara güvenmek yerine sabit bir zaman parçalı lık belirtmeyi gerektirir. Bunun nedeni, kullanıcı tarafından tarayıcı penceresini yeniden boyutlandırma veya bir ekran çözünürlüğünden diğerine giden zaman parçalı bir şekilde otomatik olarak değiştirildiğinde grafiklerdeki değerlerin değişmesidir. Zaman tanecikliliğin ortaya çıkan değişimi, y ekseni aralığının geçerli seçimini geçersiz kakarak grafiğin görünümünü etkiler.

## <a name="change-colors-of-chart-lines"></a>Grafik satırlarının renklerini değiştirme

Grafikleri yapılandırıldıktan sonra, grafik satırlarına varsayılan bir paletten otomatik olarak bir renk atanır. Bu renkleri değiştirebilirsin.

Grafik satırının rengini değiştirmek için, grafiğe karşılık gelen göstergedeki renkli çubuğu tıklatın. Renk seçici iletişim kutusu açılır. Çizginin rengini yapılandırmak için renk seçiciyi kullanın.

Grafik renkleri yapılandırıldıktan sonra, grafiği bir panoya sabitlediğinizde bu şekilde kalır. Aşağıdaki bölümde grafiği nasıl sabitlediğinizi gösterilmektedir.

> [!NOTE]
> Sürüm ve yayımlama programımızın kısıtlamaları nedeniyle, grafik satırlarının renklerini değiştirmek geçici olarak azure portalı [https://portal.azure.com/?feature.colorpicker=true](https://portal.azure.com/?feature.colorpicker=true)başlatırken özel bir parametre **?feature.colorpicker=true** geçmeyi gerektirir. Bu sınırlama yakında kaldırılacak. 

![ölçüm resmi](./media/metrics-charts/018.png)

## <a name="pin-charts-to-dashboards"></a>Grafikleri panolara sabitleme

Grafikleri yapılandırıldıktan sonra, panoları yeniden görüntüleyebilmeniz için panolara eklemek isteyebilirsiniz, böylece, büyük olasılıkla diğer izleme telemetrisi bağlamında yeniden görüntüleyebilirsiniz veya ekibinizle paylaşabilirsiniz.

Yapılandırılmış bir grafiği panoya sabitlemek için:

Grafiğinizi yapılandırdıktan sonra grafiğin sağ üst köşesindeki **Grafik Eylemleri** menüsüne tıklayın ve **panoya Sabitle'yi**tıklatın.

![ölçüm resmi](./media/metrics-charts/00013.png)

## <a name="create-alert-rules"></a>Uyarı kuralları oluşturma

Metrik tabanlı uyarı kuralının temeli olarak ölçümlerinizi görselleştirmek için belirlediğiniz ölçütleri kullanabilirsiniz. Yeni uyarı kuralı, hedef kaynağınızı, metrik, bölme ve filtre boyutlarınızı grafiğinizden içerir. Bu ayarları daha sonra uyarı kuralı oluşturma bölmesinde değiştirebilirsiniz.

### <a name="to-create-a-new-alert-rule-click-new-alert-rule"></a>Yeni bir uyarı kuralı oluşturmak için **Yeni Uyarı kuralını** tıklatın

![Yeni uyarı kuralı düğmesi kırmızı ile vurgulandı](./media/metrics-charts/015.png)

Özel uyarı kuralları oluşturmayı kolaylaştırmak için grafiğinizden önceden doldurulmuş temel metrik boyutları olan uyarı kuralı oluşturma bölmesine götürülürnüz.

![Uyarı kuralı oluşturma](./media/metrics-charts/016.png)

Metrik uyarıları ayarlama hakkında daha fazla bilgi edinmek için bu [makaleye](alerts-metric.md) göz atın.

## <a name="troubleshooting"></a>Sorun giderme

*Grafiğimde veri göremiyorum.*

* Filtreler bölmedeki tüm grafiklere uygulanır. Bir grafiğe odaklanırken, diğerindeki tüm verileri hariç tutan bir filtre ayarlamadığınızdan emin olun.

* Farklı grafiklerde farklı filtreler ayarlamak istiyorsanız, bunları farklı bıçaklarda oluşturun ve bunları ayrı sık kullanılanlar olarak kaydedin. İsterseniz, onları panoya sabitleyebilirsiniz, böylece onları yan yana görebilirsiniz.

* Bir grafiği metrikte tanımlanmamış bir özelliğe göre bölümlere bölerseniz, grafikte hiçbir şey olmaz. Segmentasyon (bölme) temizlemeyi deneyin veya farklı bir özellik seçin.

## <a name="next-steps"></a>Sonraki adımlar

  Ölçümlerle işlem görebilen panolar oluşturmak için en iyi uygulamalar hakkında bilgi edinmek için [özel KPI panoları](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) oluşturma'yı okuyun.

