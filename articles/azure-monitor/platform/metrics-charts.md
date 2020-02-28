---
title: Azure Ölçüm Gezgini gelişmiş özellikleri
description: Azure Izleyici Ölçüm Gezgini gelişmiş özellikleri hakkında bilgi edinin
author: vgorbenko
services: azure-monitor
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: c754f33ab5f4346413b6603ca2cd404acac5443f
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77662409"
---
# <a name="advanced-features-of-azure-metrics-explorer"></a>Azure Ölçüm Gezgini gelişmiş özellikleri

> [!NOTE]
> Bu makalede, Ölçüm Gezgini temel özellikleriyle ilgili bilgi sahibi olduğunuz varsayılır. Yeni bir Kullanıcı ve ilk ölçüm grafiğinizi oluşturmayı öğrenmek istiyorsanız bkz. [Azure Ölçüm Gezgini](metrics-getting-started.md)kullanmaya başlama.

## <a name="metrics-in-azure"></a>Azure 'da ölçümler

[Azure izleyici 'de ölçümler](data-platform-metrics.md) , zaman içinde toplanan ve depolanan ölçülen değer ve sayı serisidir. Standart (veya "Platform") ölçümleri ve özel ölçümler vardır. Standart ölçümler size Azure platformunun kendisi tarafından sunulur. Standart ölçümler, Azure kaynaklarınızın sistem durumunu ve kullanım istatistiklerini yansıtır. Özel ölçümler, [özel olaylar ve ölçümler](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics), [Windows Azure tanılama (wad) uzantısı](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostics-extension-overview)ya da [Azure izleyici REST API](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-store-custom-rest-api)tarafından Application Insights API 'sini kullanarak, uygulamalarınız tarafından Azure 'a gönderilir.

## <a name="create-views-with-multiple-metrics-and-charts"></a>Birden çok ölçüm ve grafik içeren görünümler oluşturma

Birden çok ölçüm satırını çizdirme veya aynı anda birden çok ölçüm grafiği gösteren grafikler oluşturabilirsiniz. Bu işlevsellik şunları yapmanıza olanak sağlar:

- bir değerin birbirleriyle nasıl ilişkili olduğunu görmek için aynı grafikteki ilgili ölçümleri ilişkilendirme
- ölçümleri yakın bir yerde farklı ölçü birimleriyle görüntüle
- birden çok kaynaktan ölçümleri görsel olarak toplama ve karşılaştırma

Örneğin, 5 depolama hesabınız varsa ve aralarındaki toplam alanın kaç kez tüketildiğini öğrenmek istiyorsanız, belirli bir zaman noktasında tüm değerlerin tek bir kısmını ve toplamını gösteren bir (yığılmış) alan grafiği oluşturabilirsiniz.

### <a name="multiple-metrics-on-the-same-chart"></a>Aynı grafikte birden çok ölçüm

İlk olarak [Yeni bir grafik oluşturun](metrics-getting-started.md#create-your-first-metric-chart). **Ölçüm Ekle** ' ye tıklayın ve aynı grafiğe başka bir ölçüm eklemek için adımları tekrarlayın.

   > [!NOTE]
   > Genellikle farklı ölçü birimleri ("milisaniyelik" ve "kilobayt") ile veya bir grafik üzerinde önemli ölçüde farklı ölçekte ölçümlere sahip olmak istemezsiniz. Bunun yerine, birden çok grafik kullanmayı göz önünde bulundurun. Ölçüm Gezgini 'nde birden çok grafik oluşturmak için grafik Ekle düğmesine tıklayın.

### <a name="multiple-charts"></a>Birden çok grafik

**Grafik Ekle** ' ye tıklayın ve farklı bir ölçümle başka bir grafik oluşturun.

### <a name="order-or-delete-multiple-charts"></a>Birden çok grafiği sıralama veya silme

Birden çok grafiği sıralamak veya silmek için üç nokta ( **...** ) simgesine tıklayarak grafik menüsünü açın ve **Yukarı taşı**, **aşağı taşı**veya **Sil**seçeneklerini belirleyin.

## <a name="apply-filters-to-charts"></a>Grafiklere filtre uygulama

Boyutlara sahip ölçümleri gösteren grafiklere filtre uygulayabilirsiniz. Örneğin, "Işlem sayısı" ölçüsünün bir boyutu "yanıt türü" ise, bu boyutun filtrelemesinin başarılı veya başarısız olduğunu belirten bir boyut "yanıt türü" yalnızca başarılı (veya yalnızca başarısız) işlemler için bir grafik satırı çizmez. 

### <a name="to-add-a-filter"></a>Filtre eklemek için

1. Grafiğin üstünde **Filtre Ekle** ' yi seçin

2. Filtrelemek istediğiniz boyutu (özelliği) seçin

   ![ölçüm resmi](./media/metrics-charts/00006.png)

3. Grafiği çizdirme sırasında dahil etmek istediğiniz boyut değerlerini seçin (Bu örnek, başarılı depolama işlemlerinin filtrelenmesini gösterir):

   ![ölçüm resmi](./media/metrics-charts/00007.png)

4. Filtre değerlerini seçtikten sonra, kapatmak için filtre seçicideki dışarıda ' ı tıklatın. Artık grafik, kaç depolama işlemi başarısız olduğunu gösterir:

   ![ölçüm resmi](./media/metrics-charts/00008.png)

5. Aynı grafiklere birden çok filtre uygulamak için 1-4 adımlarını tekrarlayabilirsiniz.



## <a name="apply-splitting-to-a-chart"></a>Grafiğe bölme Uygula

Ölçüm 'in farklı segmentlerinin birbirleriyle nasıl karşılaştırılacağını görselleştirmek ve bir boyutun tek parçalarını belirlemek için bir ölçümü boyuta göre bölebilirsiniz.

### <a name="apply-splitting"></a>Bölmeyi Uygula

1. Grafiğin üzerine **bölme Uygula** ' ya tıklayın.
 
   > [!NOTE]
   > Bölme birden çok ölçüm içeren grafiklerle kullanılamaz. Ayrıca, birden fazla filtreye sahip olabilirsiniz ancak tek bir grafiğe yalnızca bir bölme boyutu uygulanabilir.

2. Grafiğinizi segmentlere ayırmak istediğiniz bir boyut seçin:

   ![ölçüm resmi](./media/metrics-charts/00010.png)

   Artık grafik artık bir boyut segmenti için bir tane olmak üzere birden çok satır gösteriyor:

   ![ölçüm resmi](./media/metrics-charts/00012.png)

3. Bunu kapatmak için **Gruplandırma seçicisindeki** uzakta ' ye tıklayın.

   > [!NOTE]
   > Senaryolarınız için ilgisiz olan kesimleri gizlemek ve grafiklerin okunmasını kolaylaştırmak için aynı boyuttaki filtrelemeyi ve bölmeyi kullanın.

## <a name="lock-boundaries-of-chart-y-axis"></a>Grafik y ekseninin sınırlarını kilitle

Grafik, büyük değerlerin daha küçük dalgalanmalarını gösterdiğinde y ekseni aralığının kilitlenmesi önemli hale gelir. 

Örneğin, başarılı istek hacmi% 99,99 ' dan% 99,5 ' e düştüğünde, hizmet kalitesindeki önemli bir düşüş temsil edebilir. Ancak, yaşıyorsanız küçük bir sayısal değer dalgalanması, varsayılan grafik ayarlarından zor veya imkansız olabilir. Bu durumda, grafiğin en düşük sınırını %99 olarak kilitleyebilir ve bu küçük bir bırakma daha belirgin hale gelir. 

Diğer bir örnek ise, değerin teknik açıdan hiçbir şekilde hiçbir şekilde ulaşmayacağı kullanılabilir belleğin bir dalgalanmasıdır. Aralığın daha yüksek bir değere düzeltilmesi, kullanılabilir belleğin düşün daha kolay olmasını sağlayabilir. 

Y ekseni aralığını denetlemek için "..." öğesini kullanın Grafik menüsü ve gelişmiş grafik ayarlarına erişmek için **grafiği Düzenle** ' yi seçin. Y ekseni Aralık bölümündeki değerleri değiştirin veya varsayılanlara dönmek için **Otomatik** düğmesini kullanın.

![ölçüm resmi](./media/metrics-charts/00014-manually-set-granularity.png)

> [!WARNING]
> Belirli bir süre içinde çeşitli sayıları veya toplamları izleyen grafikler için y ekseninin sınırlarını kilitlemek (ve bu nedenle Count, Sum, minimum veya Maximum toplamaların kullanılması) genellikle otomatik varsayılanlara güvenmek yerine sabit bir zaman ayrıntı düzeyi belirtilmesini gerektirir. Bu gereklidir çünkü, zaman ayrıntı düzeyi Kullanıcı tarayıcı penceresi yeniden boyutlandırılırken veya bir ekran çözünürlüğünden diğerine geçiş yaparken, grafiklerde değerler değişir. Zaman ayrıntı düzeyinde ortaya çıkan değişiklik, grafik görünümünü etkiler ve geçerli y ekseni aralığının geçersiz kılınmasının sonucunu vermez.

## <a name="pin-charts-to-dashboards"></a>Grafikleri panolara sabitleme

Grafikleri yapılandırdıktan sonra, bu panoyu yeniden görüntüleyebilmeniz, belki de diğer izleme telemetrisi bağlamında veya takımınızla paylaşmak üzere panolara eklemek isteyebilirsiniz.

Yapılandırılmış bir grafiği panoya sabitlemek için:

Grafiğinizi yapılandırdıktan sonra grafiğin sağ üst köşesindeki **grafik eylemleri** menüsüne tıklayın ve **panoya sabitle**' ye tıklayın.

![ölçüm resmi](./media/metrics-charts/00013.png)

## <a name="create-alert-rules"></a>Uyarı kuralları oluşturma

Ölçümünüzü ölçüm tabanlı bir uyarı kuralının temeli olarak görselleştirmek için ayarladığınız ölçütü kullanabilirsiniz. Yeni uyarı kuralı, grafiğinizdeki hedef kaynağı, ölçümü, bölmeyi ve filtre boyutlarını içerir. Bu ayarları daha sonra uyarı kuralı oluşturma bölmesinde değiştirebileceksiniz.

### <a name="to-create-a-new-alert-rule-click-new-alert-rule"></a>Yeni bir uyarı kuralı oluşturmak için **Yeni uyarı kuralı** ' na tıklayın.

![Kırmızı renkle vurgulanmış yeni uyarı kuralı düğmesi](./media/metrics-charts/015.png)

Özel uyarı kuralları oluşturulmasını kolaylaştırmak için grafiğinizdeki temeldeki ölçüm boyutları ile uyarı kuralı oluşturma bölmesine yönlendirilirsiniz.

![Uyarı kuralı oluştur](./media/metrics-charts/016.png)

Ölçüm uyarılarını ayarlama hakkında daha fazla bilgi edinmek için bu [makaleye](alerts-metric.md) göz atın.

## <a name="troubleshooting"></a>Sorun giderme

*Grafiğimde hiç veri görmüyorum.*

* Filtreler bölmedeki tüm grafiklere uygulanır. Tek bir grafiğe odaklanırken, diğer tüm verileri dışlayan bir filtre ayarlamadıysanız emin olun.

* Farklı grafiklerde farklı filtreler ayarlamak istiyorsanız bunları farklı dikey pencerelerde oluşturun, bunları ayrı sık kullanılanlar olarak kaydedin. İsterseniz, bunları birbirlerine göre görebilmeniz için panoya sabitleyebilir.

* Bir grafiği ölçümde tanımlı olmayan bir özelliğe göre segmentleyebilirsiniz, grafik üzerinde hiçbir şey olmayacaktır. Segmentleme işlemini temizlemeyi deneyin (bölme) veya farklı bir özellik seçin.

## <a name="next-steps"></a>Sonraki adımlar

  Ölçümlerle eyleme dönüştürülebilir panolar oluşturmaya yönelik en iyi uygulamalar hakkında bilgi edinmek için [özel KPI panoları oluşturma](https://docs.microsoft.com/azure/application-insights/app-insights-tutorial-dashboards) makalesini okuyun.

