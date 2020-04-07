---
title: Sunucu tarafı performans sorguları
description: API aramaları ile sunucu tarafı performans sorguları nasıl yapılır?
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.openlocfilehash: 9a28dee2d1e6d1355b729a56e8eeb8447e4ed8c8
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80682032"
---
# <a name="server-side-performance-queries"></a>Sunucu tarafı performans sorguları

Sunucuda iyi görüntüleme performansı kararlı kare hızları ve iyi bir kullanıcı deneyimi için çok önemlidir. Sunucudaki performans özelliklerini dikkatle izlemek ve gerektiğinde optimize etmek önemlidir. Performans verileri özel API işlevleri aracılığıyla sorgulanabilir.

Oluşturma performansı için en etkili model giriş verileridir. [Model dönüştürmesini yapılandırmada](../../how-tos/conversion/configure-model-conversion.md)açıklandığı gibi giriş verilerini değiştirebilirsiniz.

İstemci tarafı uygulama performansı da bir darboğaz olabilir. İstemci tarafı performansının derinlemesine analizi [için, bir performans izleme](../../how-tos/performance-tracing.md)alması önerilir.

## <a name="clientserver-timeline"></a>İstemci/sunucu zaman çizelgesi

Çeşitli gecikme değerleri yle ilgili ayrıntılara girmeden önce, zaman çizelgesinde istemci ve sunucu arasındaki eşitleme noktalarına göz atmak faydalıdır:

![Boru hattı zaman çizelgesi](./media/server-client-timeline.png)

Resimde nasıl gösterilmektedir:

* bir *Poz tahmini* sabit 60-Hz kare hızında (her 16,6 ms) istemci tarafından başladı
* sunucu daha sonra pozdayalı, render başlar
* sunucu kodlanmış video görüntüsünü geri gönderir
* istemci görüntüyü çözer, üzerinde bazı CPU ve GPU çalışmaları gerçekleştirir ve ardından görüntüyü görüntüler

## <a name="frame-statistics-queries"></a>Çerçeve istatistik sorguları

Çerçeve istatistikleri, son kare için gecikme süresi gibi bazı üst düzey bilgiler sağlar. `FrameStatistics` Yapıda sağlanan veriler istemci tarafında ölçülür, bu nedenle API eşzamanlı bir çağrıdır:

````c#
void QueryFrameData(AzureSession session)
{
    FrameStatistics frameStatistics;
    if (session.GraphicsBinding.GetLastFrameStatistics(out frameStatistics) == Result.Success)
    {
        // do something with the result
    }
}
````

Alınan `FrameStatistics` nesne aşağıdaki üyeleri tutar:

| Üye | Açıklama |
|:-|:-|
| gecikmePoseToReceive | Bu poz için bir sunucu çerçevesi istemci uygulaması için tamamen kullanılabilir olana kadar kameradan gecikme tahmini istemci aygıtında poz. Bu değer, ağ gidiş-dönüş, sunucu oluşturma süresi, video çözme ve jitter telafiiçerir. **Yukarıdaki resimde aralık 1'e bakın.**|
| gecikmeReceiveToPresent | İstemci uygulaması CPU'da PresentFrame'i çağırana kadar alınan uzak bir çerçevenin kullanılabilirliğinden gecikme süresi. |
| gecikmePresentToDisplay  | Ekran yanana kadar CPU'da bir çerçeve sunmanın gecikme süresi. Bu değer istemci GPU süresini, işletim sistemi tarafından gerçekleştirilen herhangi bir çerçeve arabelleğesini, donanım yeniden projeksiyonunu ve aygıta bağımlı ekran tarama süresini içerir. **Yukarıdaki resimde 2 aralığına bakın.**|
| timeSinceLastPresent | CPU'da PresentFrame sonraki aramalar arasındaki süre. Görüntü süresinden büyük değerler (örneğin, 60-Hz istemci aygıtındaki 16,6 ms) istemci uygulamasının CPU iş yükünü zamanında tamamlamaması nedeniyle oluşan sorunları gösterir. **Yukarıdaki resimde 3 aralığına bakın.**|
| videoFramesAlınan | Son saniyede sunucudan alınan kare sayısı. |
| videoFrameReusedSayısı | Aygıtta birden fazla kez kullanılan son saniyede alınan kare sayısı. Sıfır olmayan değerler, ağ gerginliği veya aşırı sunucu oluşturma süresi nedeniyle çerçevelerin yeniden kullanılması ve yeniden yansıtılması gerektiğini gösterir. |
| videoFramesAted | Son saniyede alınan ve kodlanmış, ancak yeni bir çerçeve geldiği için ekranda gösterilmeyen kare sayısı. Sıfır olmayan değerler, ağ titremesinin birden çok karenin gecikmesine ve ardından istemci aygıtına bir patlamada birlikte gelmesine neden olduğunu gösterir. |
| videoFramesDiscarded | Çok **videoFramesBenzer**, ama atılması nedeni bir çerçeve bile artık herhangi bir bekleyen poz ile ilişkili olamaz o kadar geç geldi olmasıdır. Bu durumda, bazı ciddi ağ çekişmesi vardır.|
| videoFrameMinDelta | Son saniye de gelen iki ardışık kare arasında minimum süre. VideoFrameMaxDelta ile birlikte, bu aralık ağ veya video codec neden gerginlik bir göstergesi verir. |
| videoFrameMaxDelta | Son saniye de gelen iki ardışık kare arasındaki maksimum süre. VideoFrameMinDelta ile birlikte, bu aralık ağ veya video codec neden gerginlik bir göstergesi verir. |

Tüm gecikme değerlerinin toplamı genellikle 60 Hz'deki kullanılabilir çerçeve süresinden çok daha büyüktür. Bu tamam, çünkü birden çok kare paralel olarak uçar ve yeni kare istekleri resimde gösterildiği gibi istenilen kare hızında atılır. Ancak gecikme çok büyük olursa, geç aşama [yeniden projeksiyon](../../overview/features/late-stage-reprojection.md)kalitesini etkiler ve genel deneyimi tehlikeye atabilir.

`videoFramesReceived`, `videoFrameReusedCount`ve `videoFramesDiscarded` ağ ve sunucu performansını ölçmek için kullanılabilir. Düşükse `videoFramesReceived` `videoFrameReusedCount` ve yüksekse, bu ağ tıkanıklığını veya kötü sunucu performansını gösterebilir. Yüksek `videoFramesDiscarded` bir değer de ağ tıkanıklığı gösterir.

Son`timeSinceLastPresent`olarak, `videoFrameMinDelta`, `videoFrameMaxDelta` , ve gelen video çerçeveleri ve yerel mevcut çağrıların varyans bir fikir vermek. Yüksek varyans kararsız kare hızı anlamına gelir.

Yukarıdaki değerlerin hiçbiri saf ağ gecikmesi (resimde kırmızı oklar) net bir gösterge verir, çünkü sunucu nun tam olarak zaman işleme `latencyPoseToReceive`meşgul tur değeri çıkarılmalıdır. Genel gecikmenin sunucu tarafındaki kısmı istemci tarafından kullanılamayan bilgilerdir. Ancak, bir sonraki paragrafta bu değerin sunucudan gelen ek giriş `networkLatency` yoluyla nasıl yaklaşık olarak açıklanır ve değer üzerinden açığa çıkar.

## <a name="performance-assessment-queries"></a>Performans değerlendirme sorguları

*Performans değerlendirme sorguları,* sunucudaki CPU ve GPU iş yükü hakkında daha ayrıntılı bilgi sağlar. Veriler sunucudan istendiğinden, performans anlık görüntüsünü sorgulamak olağan async deseni izler:

``` cs
PerformanceAssessmentAsync _assessmentQuery = null;

void QueryPerformanceAssessment(AzureSession session)
{
    _assessmentQuery = session.Actions.QueryServerPerformanceAssessmentAsync();
    _assessmentQuery.Completed += (PerformanceAssessmentAsync res) =>
    {
        // do something with the result:
        PerformanceAssessment result = res.Result;
        // ...

        _assessmentQuery = null;
    };
}
```

Nesnenin `FrameStatistics` `PerformanceAssessment` aksine, nesne sunucu tarafı bilgileri içerir:

| Üye | Açıklama |
|:-|:-|
| timeCPU | Milisaniye cinsinden kare başına ortalama sunucu CPU süresi |
| timeGPU | Milisaniye cinsinden kare başına ortalama sunucu GPU süresi |
| kullanımCPU | Yüzde toplam sunucu CPU kullanımı |
| kullanımıGPU | Yüzde toplam sunucu GPU kullanımı |
| memoryCPU | Toplam sunucu ana bellek kullanımı yüzde |
| memoryGPU | Sunucu GPU yüzdesi toplam özel video bellek kullanımı |
| ağLatency | Milisaniye cinsinden yaklaşık ortalama gidiş dönüş ağı gecikmesi. Yukarıdaki resimde, bu kırmızı okların toplamına karşılık gelir. Değer, gerçek sunucu oluşturma süresinin değeri nden `latencyPoseToReceive` `FrameStatistics`çıkarılarak hesaplanır. Bu yaklaşım doğru olmasa da, istemcide hesaplanan gecikme değerlerinden yalıtılmış ağ gecikmesinin bir göstergesidir. |
| çokgenlerRendered | Bir karede işlenen üçgen sayısı. Bu sayı, daha sonra işleme sırasında itlaf edilen üçgenleri de içerir. Bu, bu sayının farklı kamera konumları arasında çok fazla değişiklik olmadığı anlamına gelir, ancak performans üçgen toplama hızına bağlı olarak büyük ölçüde değişebilir.|

Eğer değerleri değerlendirmek yardımcı olmak için, her porsiyon **Büyük**gibi bir kalite sınıflandırması ile birlikte gelir , **İyi**, **Vasat**, veya **Kötü**.
Bu değerlendirme ölçümü sunucunun durumunun kabaca bir göstergesidir, ancak mutlak olarak görülmemelidir. Örneğin, GPU zamanı için 'vasat' bir puan gördüğünüzü varsayalım. Bu genel çerçeve zaman bütçesi için sınıra yakın alır çünkü vasat olarak kabul edilir. Ancak sizin durumunuzda, karmaşık bir model oluşturmadığınız için yine de iyi bir değer olabilir.

## <a name="statistics-debug-output"></a>İstatistikler hata ayıklama çıktısı

Sınıf `ARRServiceStats` hem çerçeve istatistiklerini hem de performans değerlendirme sorgularını sarar ve istatistikleri toplu değerler olarak veya önceden oluşturulmuş bir dize olarak döndürmek için kullanışlı işlevsellik sağlar. Aşağıdaki kod, istemci uygulamanızda sunucu tarafındaki istatistikleri göstermenin en kolay yoludur.

``` cs
ARRServiceStats _stats = null;

void OnConnect()
{
    _stats = new ARRServiceStats();
}

void OnDisconnect()
{
    _stats = null;
}

void Update()
{
    if (_stats != null)
    {
        // update once a frame to retrieve new information and build average values
        _stats.Update(Service.CurrentActiveSession);

        // retrieve a string with relevant stats information
        InfoLabel.text = _stats.GetStatsString();
    }
}
```

Yukarıdaki kod, metin etiketini aşağıdaki metinle doldurur:

![ArrServiceStats dize çıkışı](./media/arr-service-stats.png)

`GetStatsString` API tüm değerlerin bir dize biçimlendirin, ancak her bir değer `ARRServiceStats` de örnekten programlı olarak sorgulanabilir.

Üyelerin zaman içinde değerleri bir araya getiren türevleri de vardır. Sonek `*Avg`, , `*Max`veya `*Total`. Üye, `FramesUsedForAverage` bu toplama için kaç çerçeve kullanıldığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

* [Performans izlemeleri oluşturma](../../how-tos/performance-tracing.md)
* [Model dönüşümyapılandırma](../../how-tos/conversion/configure-model-conversion.md)
