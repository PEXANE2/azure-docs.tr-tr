---
title: Sorguları izleme
titleSuffix: Azure Cognitive Search
description: Performans ve iş elde etmek için sorgu ölçümlerini izleyin. Tanılama günlüklerinde sorgu dize girişlerini toplayın ve analiz edin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: a3a313ef9cd74ba901f5a6a2d82a18e3c21145dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77462539"
---
# <a name="monitor-query-requests-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da sorgu isteklerini izleme

Bu makalede, ölçümler ve tanılama günlüğe kaydetme kullanarak sorgu performansı ve birim nasıl ölçülecek açıklanmaktadır. Ayrıca, arama korpusunun yarar ve etkinliğini değerlendirmeniz gerektiğinde, sorgularda kullanılan giriş terimlerinin nasıl toplandığını da açıklar.

Ölçümlere beslenen geçmiş veriler 30 gün boyunca korunur. Daha uzun bekletme veya operasyonel veriler ve sorgu dizeleri hakkında rapor vermek için, günlüğe kaydedilmiş olayları ve ölçümleri kalıcı hale getirmek için bir depolama seçeneği belirten bir [tanılama ayarını](search-monitor-logs.md) etkinleştirdiğinden emin olun.

Veri ölçümünün bütünlüğünü en üst düzeye çıkaran koşullar şunlardır:

+ Faturalandırılabilir bir hizmet (Temel veya Standart katmanda oluşturulan bir hizmet) kullanın. Ücretsiz hizmet birden çok abone tarafından paylaşılır ve bu da yükler değiştikçe belirli bir miktar değişkenlik sağlar.

+ İçe ve yalıtılmış bir ortam oluşturmak için mümkünse tek bir yineleme ve bölüm kullanın. Birden çok yineleme kullanıyorsanız, sorgu ölçümleri birden çok düğüm arasında ortalamaolarak alınır ve bu da sonuçların kesinliğini düşürebilir. Benzer şekilde, birden çok bölüm, dizin oluşturma da devam ediyorsa, bazı bölümlerin farklı verilere sahip olabileceği potansiyeliyle birlikte verilerin bölündüğü anlamına gelir. Sorgu performansını alarken, tek bir düğüm ve bölüm sınama için daha kararlı bir ortam sağlar.

> [!Tip]
> Ek istemci tarafı kodu ve Uygulama Öngörüleri ile, uygulama kullanıcılarınızın ilgisini çeken şeyin ne olduğunu daha derinlemesine anlamak için tıklama verilerini de yakalayabilirsiniz. Daha fazla bilgi için [bkz.](search-traffic-analytics.md)

## <a name="query-volume-qps"></a>Sorgu hacmi (QPS)

Birim, bir dakika içinde çalıştırılan sorguların ortalama, sayı, minimum veya maksimum değerleri olarak bildirilebilen yerleşik bir metrik **olan, Saniye Başına Arama Sorguları** (QPS) olarak ölçülür. Ölçümler için bir dakikalık aralıklar (TimeGrain = "PT1M") sistem içinde sabitlenir.

Sorguların milisaniye cinsinden yürütülmesi yaygındır, bu nedenle yalnızca saniye olarak ölçen sorgular ölçümlerde görünür.

| Toplama Türü | Açıklama |
|------------------|-------------|
| Ortalama | Sorgu yürütmesinin gerçekleştiği bir dakika içindeki ortalama saniye sayısı.|
| Sayı | Bir dakika aralığında günlüğe yayılan ölçümlerin sayısı. |
| Maksimum | Bir dakika içinde kaydedilmiş saniyede en yüksek arama sorgusu sayısı. |
| Minimum | Saniyebaşına en düşük arama sorgusu sayısı bir dakika içinde kaydedilir.  |
| Toplam | Dakika içinde yürütülen tüm sorguların toplamı.  |

Örneğin, bir dakika içinde, bunun gibi bir desen olabilir: SearchQueriesPerSecond için maksimum yüksek yük bir saniye, ortalama yük 58 saniye takip ve son olarak bir saniye sadece bir sorgu ile, en az.

Başka bir örnek: bir düğüm, her bir metnin değerinin 40 olduğu 100 ölçüm yayıbiliyorsa, "Sayım" 100, "Toplam" 4000, "Ortalama" 40 ve "Max" 40'tır.

## <a name="query-performance"></a>Sorgu performansı

Hizmet genelinde, sorgu performansı arama gecikmesi (sorgunun tamamlanması ne kadar sürer) ve kaynak çekişmesi sonucunda bırakılan daraltılmış sorgular olarak ölçülür.

### <a name="search-latency"></a>Arama gecikmesi

| Toplama Türü | Gecikme süresi | 
|------------------|---------|
| Ortalama | Milisaniye cinsinden ortalama sorgu süresi. | 
| Sayı | Bir dakika aralığında günlüğe yayılan ölçümlerin sayısı. |
| Maksimum | Örnekteki en uzun süre çalışan sorgu. | 
| Minimum | Örnekteki en kısa çalışan sorgu.  | 
| Toplam | Örnekteki tüm sorguların toplam yürütme süresi, aralık içinde yürütülme (bir dakika).  |

**Arama Gecikmesi** ölçümlerinin aşağıdaki örneğini göz önünde bulundurun: Ortalama 23,26 milisaniye lik bir süreyle 86 sorgu örneklendi. En az 0, bazı sorguların bırakıldığını gösterir. En uzun süren sorgunun tamamlanması 1000 milisaniye sürdü. Toplam yürütme süresi 2 saniye ydi.

![Gecikme agregaları](./media/search-monitor-usage/metrics-latency.png "Gecikme agregaları")

### <a name="throttled-queries"></a>Daraltılmış sorgular

Daraltılmış sorgular, işlem yerine bırakılan sorguları ifade eder. Çoğu durumda, azaltma hizmeti çalıştırmanın normal bir parçasıdır.  Bu mutlaka bir yanlış bir gösterge değildir.

Azaltma, şu anda işlenen istek sayısı kullanılabilir kaynakları aştığında oluşur. Yineleme döndürme den çıkarıldığında veya dizin oluşturma sırasında daraltılmış isteklerde bir artış görebilirsiniz. Hem sorgu hem de dizin oluşturma istekleri aynı kaynak kümesi tarafından işlenir.

Hizmet, kaynak tüketimine göre isteklerin bırakılıp bırakılmayacağını belirler. Bellek, CPU ve disk IO'da tüketilen kaynakların yüzdesi belirli bir süre boyunca ortalamaolarak alınır. Bu yüzde bir eşiği aşarsa, istek hacmi azalana kadar dizindeki tüm istekler daraltır. 

Müşterinize bağlı olarak, daraltılmış bir istek şu şekilde belirtilebilir:

+ Bir hizmet bir hata döndürür "Çok fazla istek gönderiyorsunuz. Lütfen daha sonra tekrar deneyin." 
+ Bir hizmet, hizmetin şu anda kullanılamamış olduğunu belirten bir 503 hata kodu döndürür. 
+ Portalı kullanıyorsanız (örneğin, Arama Gezgini), sorgu sessizce bırakılır ve yeniden Ara'yı tıklatmanız gerekir.

Daraltılmış sorguları onaylamak **için, Daraltılmış arama sorguları** ölçüsükullanın. Portaldaki ölçümleri keşfedebilir veya bu makalede açıklandığı gibi bir uyarı ölçümü oluşturabilirsiniz. Örnekleme aralığıiçinde bırakılan sorgular için, yürütülmeyen sorguların yüzdesini almak için *Toplam'ı* kullanın.

| Toplama Türü | Azaltma |
|------------------|-----------|
| Ortalama | Aralık içinde bırakılan sorguyüzdesi. |
| Sayı | Bir dakika aralığında günlüğe yayılan ölçümlerin sayısı. |
| Maksimum | Aralık içinde bırakılan sorguyüzdesi.|
| Minimum | Aralık içinde bırakılan sorguyüzdesi. |
| Toplam | Aralık içinde bırakılan sorguyüzdesi. |

**Daraltılmış Arama Sorguları Yüzdesi**için, en az, en yüksek, ortalama ve toplam, hepsi aynı değere sahiptir: bir dakika boyunca toplam arama sorgusu sayısından daraltılmış arama sorgularının yüzdesi.

Aşağıdaki ekran görüntüsünde, ilk sayı sayımdır (veya günlüğe gönderilen metrik sayısı). En üstte veya metrin üzerinde gezinirken görünen ek toplamalar ortalama, maksimum ve toplamiçerir. Bu örnekte, hiçbir istek bırakılmadı.

![Daraltılmış toplamalar](./media/search-monitor-usage/metrics-throttle.png "Daraltılmış toplamalar")

## <a name="explore-metrics-in-the-portal"></a>Portaldaki ölçümleri keşfedin

Geçerli sayılara hızlı bir bakış için, hizmete Genel Bakış sayfasındaki **İzleme** sekmesi, toplama türünü değiştirme seçeneğiyle, saat, gün ve hafta cinsinden ölçülen sabit aralıklarla üç ölçüm **(Arama gecikmesi,** **saniye başına arama sorguları (arama birimi başına)**, **Daraltılmış Arama Sorguları Yüzdesi)** gösterir.

Daha derin bir araştırma için, eğilimleri veya anormallikleri keşfetmek için verileri katmanlayabilmeniz, yakınlaştırabilmeniz ve görselleştirebilmeniz için **İzleme** menüsünden açık ölçümler kaşifi yapın. Bir metrik grafiği oluşturma yla ilgili bu [öğreticiyi](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-metrics-explorer)tamamlayarak ölçümler explorer hakkında daha fazla bilgi edinin.

1. İzleme bölümünde, arama hizmetinize ayarlanan kapsamla ölçüler gezginini açmak için **Ölçümler'i** seçin.

1. Metrik altında, açılır listeden birini seçin ve tercih edilen bir tür için kullanılabilir toplama listesini gözden geçirin. Toplama, toplanan değerlerin her zaman aralığında nasıl örnekleneceğini tanımlar.

   ![QPS ölçümü için ölçümler kaşifi](./media/search-monitor-usage/metrics-explorer-qps.png "QPS ölçümü için ölçümler kaşifi")

1. Sağ üst köşede, zaman aralığını ayarlayın.

1. Bir görselleştirme seçin. Varsayılan bir çizgi grafiğidir.

1. **Metrik ekle'yi** seçerek ve farklı toplamaları seçerek ek toplamaları katmanlayın.

1. Çizgi grafiğinde ilgi çeken bir alana yakınlaştırın. Fare işaretçisini alanın başına koyun, sol fare düğmesini tıklatın ve basılı tutun, alanın diğer tarafına sürükleyin ve düğmeyi bırakın. Grafik bu zaman aralığını yakınlaştırır.

## <a name="identify-strings-used-in-queries"></a>Sorgularda kullanılan dizeleri tanımlama

Tanılama günlüğe kaydetmeyi etkinleştirdiğinizde, sistem **Azure Diagnostics** tablosunda sorgu isteklerini yakalar. Ön koşul olarak, günlük analitiği çalışma alanı veya başka bir depolama seçeneği belirterek [tanılama günlüğünü](search-monitor-logs.md)zaten etkinleştirmiş olmalısınız.

1. İzleme bölümünde, Log Analytics'te boş bir sorgu penceresi açmak için **Günlükler'i** seçin.

1. Sorgu sorgusu.Arama işlemleri için aşağıdaki ifadeyi çalıştırın, işlem adı, sorgu dizesi, sorgulanan dizin ve bulunan belge sayısından oluşan bir tablo sonucu kümesi döndürün. Son iki deyim, sonuçlarınızdaki gürültüyü azaltan bir örnek dizini üzerinde boş veya belirtilmemiş bir aramadan oluşan sorgu dizelerini hariç tutar.

   ```
   AzureDiagnostics
   | project OperationName, Query_s, IndexName_s, Documents_d
   | where OperationName == "Query.Search"
   | where Query_s != "?api-version=2019-05-06&search=*"
   | where IndexName_s != "realestate-us-sample-index"
   ```

1. İsteğe bağlı olarak, belirli bir sözdizimi veya dize üzerinde arama yapmak için *Query_s* bir Sütun filtresi ayarlayın. Örneğin, üzerinde filtre olabilir `?api-version=2019-05-06&search=*&%24filter=HotelName` *eşittir).*

   ![Günlüğe kaydedilmiş sorgu dizeleri](./media/search-monitor-usage/log-query-strings.png "Günlüğe kaydedilmiş sorgu dizeleri")

Bu teknik geçici araştırma için çalışırken, bir rapor oluşturmak sorgu dizelerini çözümlemeye daha elverişli bir düzende birleştirmenize ve sunmanıza olanak tanır.

## <a name="identify-long-running-queries"></a>Uzun süren sorguları belirleme

Sadece metrik olarak alınanların değil, tüm sorguların numaralarını almak için süre sütununa ekleyin. Bu verileri sıralamak, hangi sorguların tamamlanmasının en uzun süre olduğunu gösterir.

1. İzleme bölümünde, günlük bilgileri için sorgu yapmak için **Günlükler'i** seçin.

1. Süreye göre milisaniye cinsinden sıralanmış sorguları döndürmek için aşağıdaki sorguyu çalıştırın. En uzun süren sorgular en üsttedir.

   ```
   AzureDiagnostics
   | project OperationName, resultSignature_d, DurationMs, Query_s, Documents_d, IndexName_s
   | where OperationName == "Query.Search"
   | sort by DurationMs
   ```

   ![Sorguları süreye göre sıralama](./media/search-monitor-usage/azurediagnostics-table-sortby-duration.png "Sorguları süreye göre sıralama")

## <a name="create-a-metric-alert"></a>Metrik uyarı oluşturma

Metrik uyarı, bir bildirim alacağınız veya önceden tanımladığınız düzeltici eylemi tetiklediğiniz bir eşik belirler. 

Bir arama hizmeti için, arama gecikmesi ve daraltılmış sorgular için bir metrik uyarı oluşturmak yaygındır. Sorguların ne zaman bırakıldığını biliyorsanız, yükü azaltan veya kapasiteyi artıran çözümler arayabilirsiniz. Örneğin, dizin oluşturma sırasında daraltılmış sorgular artarsa, sorgu etkinliği dinlenene kadar erteleyebilirsiniz.

Belirli bir çoğaltma bölümü yapılandırmasının sınırlarını zorlarken, sorgu birimi eşikleri (QPS) için uyarılar ayarlamak da yararlıdır.

1. İzleme bölümü altında **Uyarılar'ı** seçin ve ardından **+ Yeni uyarı kuralını**tıklatın. Arama hizmetinizin kaynak olarak seçildiğinden emin olun.

1. Koşul altında **Ekle'yi**tıklatın.

1. Sinyal mantığını yapılandırın. Sinyal türü için **ölçümleri** seçin ve ardından sinyali seçin.

1. Sinyali seçtikten sonra, ayarlama koşullarına nasıl devam edileceğine ilişkin bilinçli bir karar için geçmiş verileri görselleştirmek için bir grafik kullanabilirsiniz.

1. Ardından, Uyarı mantığına doğru aşağı kaydırın. Kavram kanıtı için, test amacıyla yapay olarak düşük bir değer belirtebilirsiniz.

   ![Uyarı mantığı](./media/search-monitor-usage/alert-logic-qps.png "Uyarı mantığı")

1. Ardından, bir Eylem Grubu belirtin veya oluşturun. Bu, eşik karşılandığında çağrılması gereken yanıttır. Anında iletme bildirimi veya otomatik bir yanıt olabilir.

1. Son olarak, Uyarı ayrıntılarını belirtin. Uyarıyı adlandırın ve açıklayın, önem derecesi atayın ve kuralı etkin veya devre dışı bırakılmış bir durumda oluşturup oluşturmayacağını belirtin.

   ![Uyarı ayrıntıları](./media/search-monitor-usage/alert-details.png "Uyarı ayrıntıları")

Bir e-posta bildirimi belirtirseniz, "Azure: Etkinleştirilen Önem Derecesi: 3" `<your rule name>`konu satırıiçeren "Microsoft Azure"dan bir e-posta alırsınız.

<!-- ## Report query data

Power BI is an analytical reporting tool useful for visualizing data, including log information. If you are collecting data in Blob storage, a Power BI template makes it easy to spot anomalies or trends. Use this link to download the template. -->

## <a name="next-steps"></a>Sonraki adımlar

Bunu daha önce yapmadıysanız, tüm gözetim yetenekleri hakkında bilgi edinmek için arama hizmeti izlemenin temellerini gözden geçirin.

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama'daki işlemleri ve etkinlikleri izleme](search-monitor-usage.md)