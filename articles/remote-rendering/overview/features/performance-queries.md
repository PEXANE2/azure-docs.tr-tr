---
title: Sunucu tarafı performans sorguları
description: API çağrıları aracılığıyla sunucu tarafı performans sorguları yapma
author: florianborn71
ms.author: flborn
ms.date: 02/10/2020
ms.topic: article
ms.openlocfilehash: 2e843216bf973033868e75c027b11d27ddfe2e93
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83757475"
---
# <a name="server-side-performance-queries"></a>Sunucu tarafı performans sorguları

Sunucu üzerinde iyi işleme performansı, kararlı çerçeve ücretleri ve iyi bir kullanıcı deneyimi için önemlidir. Sunucu üzerindeki performans özelliklerini dikkatle izlemek ve gerektiğinde iyileştirmek önemlidir. Performans verileri, adanmış API işlevleri aracılığıyla sorgulanabilir.

İşleme performansı için en etkili model giriş verileri ' dir. [Model dönüştürmeyi yapılandırma](../../how-tos/conversion/configure-model-conversion.md)bölümünde açıklandığı gibi giriş verilerini ince ayar yapabilirsiniz.

İstemci tarafı uygulama performansı bir performans sorunu olabilir. İstemci tarafı performansının derinlemesine çözümlenmesi için bir [performans izlemesi](../../how-tos/performance-tracing.md)yapmanız önerilir.

## <a name="clientserver-timeline"></a>İstemci/sunucu zaman çizelgesi

Çeşitli gecikme değerleriyle ilgili ayrıntılarla karşılaşmadan önce, zaman çizelgesinde istemci ve sunucu arasındaki eşitleme noktalarına göz atalım:

![Ardışık düzen zaman çizelgesi](./media/server-client-timeline.png)

Çizimde şunları gösterilmektedir:

* bir *poz tahmini* , istemci tarafından sürekli olarak 60-Hz kare hızına göre yapılır (her 16,6 MS)
* daha sonra sunucu, poza göre işlemeye başlar
* sunucu, kodlanmış video görüntüsünü geri gönderiyor
* istemci görüntünün kodunu çözer, üzerine bazı CPU ve GPU işleri gerçekleştirir ve ardından görüntüyü görüntüler

## <a name="frame-statistics-queries"></a>Çerçeve istatistikleri sorguları

Çerçeve istatistikleri, son çerçeve için gecikme gibi bazı üst düzey bilgiler sağlar. Yapıda belirtilen veriler `FrameStatistics` istemci tarafında ölçülür, bu nedenle API zaman uyumlu bir çağrıdır:

```cs
void QueryFrameData(AzureSession session)
{
    FrameStatistics frameStatistics;
    if (session.GraphicsBinding.GetLastFrameStatistics(out frameStatistics) == Result.Success)
    {
        // do something with the result
    }
}
```

```cpp
void QueryFrameData(ApiHandle<AzureSession> session)
{
    FrameStatistics frameStatistics;
    if (*session->GetGraphicsBinding()->GetLastFrameStatistics(&frameStatistics) == Result::Success)
    {
        // do something with the result
    }
}
```

Alınan `FrameStatistics` nesne aşağıdaki üyeleri barındırır:

| Üye | Açıklama |
|:-|:-|
| Latşifreli Posetoreceive | Bu poza yönelik bir sunucu çerçevesi istemci uygulaması tarafından tam olarak kullanılabilir olana kadar, kamera, istemci cihazında tahmine göre gecikme süresi. Bu değer, ağ gidiş dönüş, sunucu işleme süresi, video kod çözme ve değişim dengelemesi içerir. **Yukarıdaki çizimde bulunan Aralık 1 ' i inceleyin.**|
| Latşifreli Receivetopyeniden gönderildi | İstemci uygulaması CPU üzerinde PresentFrame çağrılana kadar alınan bir uzak çerçevenin kullanılabilirliğine gecikme süresi. |
| latencyPresentToDisplay  | Ekran ışıkları bitene kadar CPU üzerinde bir çerçeve sunumu gecikme süresi. Bu değer, istemci GPU zamanını, işletim sistemi tarafından gerçekleştirilen tüm çerçeve ara belleği, donanım yeniden projeksiyonu ve cihaza bağlı görüntü tarama zamanı içerir. **Yukarıdaki çizimde bulunan Aralık 2 '** ye bakın.|
| timeSinceLastPresent | CPU üzerinde PresentFrame için sonraki çağrılar arasındaki süre. Görüntüleme süresinden daha büyük değerler (örneğin, 60-Hz istemci cihazında 16,6 MS), istemci uygulamanın zaman içinde CPU iş yükünü bitirmediğinden kaynaklanan sorunları gösterir. **Yukarıdaki çizimde bulunan Aralık 3 ' ü inceleyin.**|
| Videoframpaalındı | Son saniye içinde sunucudan alınan çerçeve sayısı. |
| Videofoymereusedcount | Son saniye içinde, cihazda birden çok kez kullanılan alınan çerçeve sayısı. Sıfır olmayan değerler, çerçevelerin ağ değişimi veya aşırı sunucu işleme süresi nedeniyle yeniden kullanılması gerektiğini ve yeniden yansıtıldığını gösterir. |
| Videofınmesatlandı | Son saniye içinde kodu çözülen, ancak daha yeni bir çerçeve geldiği için görüntülenmemiş olan alınan çerçeve sayısı. Sıfır olmayan değerler, ağın birden çok çerçeveye ertelenmesini ve daha sonra istemci cihaza bir veri bloğu içinde birlikte ulaştığını gösterir. |
| Videofınmesatılır | **Videofesmesatine**çok benzer, ancak atılma nedeni, bir karenin daha geç bir şekilde verilmemesine neden olan herhangi bir bekleyen pozla ilişkilendirilmeyecektir. Bu durumda, önemli bir ağ çekişmesi vardır.|
| videoFrameMinDelta | Son saniye içinde gelen ardışık iki kare arasındaki en az süre. VideoFrameMaxDelta ile birlikte, bu Aralık ağ veya video codec bileşeni tarafından neden olan bir değişim göstergesi sağlar. |
| videoFrameMaxDelta | Son saniye boyunca ardışık iki kare arasındaki en fazla zaman miktarı. VideoFrameMinDelta ile birlikte, bu Aralık ağ veya video codec bileşeni tarafından neden olan bir değişim göstergesi sağlar. |

Tüm gecikme değerlerinin toplamı, genellikle 60 Hz 'deki kullanılabilir kare süresinden çok daha büyüktür. Bu, çok sayıda çerçeve paralel olarak uçuş aşamasında olduğundan ve çizimde gösterildiği gibi, yeni çerçeve isteklerinin istenen kare hızında kapalı olması nedeniyle Tamam ' dır. Ancak gecikme çok büyük olursa, [geç aşama yeniden projeksiyonunun](../../overview/features/late-stage-reprojection.md)kalitesini etkiler ve genel deneyimle tehlikeye atabilir.

`videoFramesReceived`, `videoFrameReusedCount` ve `videoFramesDiscarded` Ağ ve sunucu performansını ölçmek için kullanılabilir. `videoFramesReceived`Düşükse ve `videoFrameReusedCount` yüksekse bu, ağ tıkanıklığını veya zayıf sunucu performansını gösterebilir. Yüksek bir `videoFramesDiscarded` değer de ağ tıkanıklığını gösterir.

Son olarak,, `timeSinceLastPresent` `videoFrameMinDelta` , ve `videoFrameMaxDelta` gelen video çerçevelerinin ve yerel mevcut çağrıların varyansı hakkında fikir verir. Yüksek Varyans, çerçeve hızının anlamına gelir.

Yukarıdaki değerlerden hiçbiri, sunucunun işleme meşgul olduğu tam sürenin gidiş dönüş değerinden çıkarılmasıyla emin olması nedeniyle saf ağ gecikmesi (çizimdeki kırmızı oklar) üzerinde net bir işaret vermez `latencyPoseToReceive` . Genel gecikme süresinin sunucu tarafı kısmı, istemci için kullanılamayan bir bilgi. Bununla birlikte, sonraki paragraf, bu değerin sunucudan alınan ek giriş ile nasıl yaklaşık olarak yapıldığını açıklar ve değer aracılığıyla gösterilir `networkLatency` .

## <a name="performance-assessment-queries"></a>Performans değerlendirmesi sorguları

*Performans değerlendirmesi sorguları* , sunucusundaki CPU ve GPU iş yükü hakkında daha ayrıntılı bilgi sağlar. Veriler sunucudan istendiğinden, bir performans anlık görüntüsünü sorgulamak olağan zaman uyumsuz örüntüyi izler:

```cs
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

```cpp
void QueryPerformanceAssessment(ApiHandle<AzureSession> session)
{
    ApiHandle<PerformanceAssessmentAsync> assessmentQuery = *session->Actions()->QueryServerPerformanceAssessmentAsync();
    assessmentQuery->Completed([] (ApiHandle<PerformanceAssessmentAsync> res)
    {
        // do something with the result:
        PerformanceAssessment result = *res->Result();
        // ...

    });
}
```

Nesnenin aksine `FrameStatistics` , `PerformanceAssessment` nesne sunucu tarafı bilgilerini içerir:

| Üye | Açıklama |
|:-|:-|
| timeCPU | Kare başına ortalama sunucu CPU süresi (milisaniye cinsinden) |
| timeGPU | Çerçeve başına ortalama sunucu GPU süresi (milisaniye) |
| Kullanımı Zalationcpu | Toplam sunucu CPU kullanımı yüzdesi |
| kullanımı Zationgpu | Toplam sunucu GPU kullanımı yüzdesi |
| memoryCPU | Toplam sunucu ana bellek kullanımı yüzdesi |
| memoryGPU | Sunucu GPU 'SU yüzdesi cinsinden toplam adanmış video belleği kullanımı |
| networkLatency | Milisaniye cinsinden yaklaşık ortalama gidiş dönüş ağ gecikmesi. Yukarıdaki çizimde, kırmızı okların toplamına karşılık gelir. Değer, gerçek sunucu işleme zamanının değerinden çıkarılmasıyla hesaplanır `latencyPoseToReceive` `FrameStatistics` . Bu yaklaşık değer doğru olmasa da, istemci üzerinde hesaplanan gecikme değerlerinden yalıtılmış olan ağ gecikmesi hakkında bir gösterge sağlar. |
| polygonsRendered | Bir çerçevede işlenen üçgenin sayısı. Bu sayı Ayrıca, işleme sırasında daha sonra gelen üçgenler de içerir. Yani bu sayı, farklı kamera konumlarında çok fazla farklılık göstermez, ancak üçgen yüzey kaldırma hızına bağlı olarak performans büyük ölçüde farklılık gösterebilir.|

Değerleri değerlendirmenize yardımcı olmak için her bölüm **harika**, **iyi**, **mediocre**veya **kötü**gibi bir kalite sınıflandırmasıyla gelir.
Bu değerlendirme ölçümü sunucunun sistem durumunu kabaca bir şekilde belirtir, ancak mutlak olarak görülmemelidir. Örneğin, GPU süresi için bir ' mediocre ' puanı gördüğünü varsayın. Bu, genel çerçeve süresi bütçesi sınırına yakın olduğundan mediocre olarak kabul edilir. Ancak, bir karmaşık modeli işlemekte olduğunuz için Nonetheless iyi bir değer olabilir.

## <a name="statistics-debug-output"></a>İstatistik hata ayıklama çıkışı

Sınıfı, `ARRServiceStats` hem çerçeve istatistikleri hem de performans değerlendirmesi sorguları etrafında kaydırılan ve istatistikleri toplanmış değerler olarak veya önceden oluşturulmuş bir dize olarak döndürmek için uygun işlevselliği sağlayan bir C# sınıfıdır. Aşağıdaki kod, istemci uygulamanızda sunucu tarafı istatistiklerini göstermek için en kolay yoldur.

```cs
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

Yukarıdaki kod metin etiketini aşağıdaki metinle doldurur:

![ArrServiceStats String çıktısı](./media/arr-service-stats.png)

`GetStatsString`API, tüm değerlerin bir dizesini biçimlendirir, ancak her bir değer aynı zamanda örnekten programlı bir şekilde sorgulanabilir `ARRServiceStats` .

Ayrıca üyelerin, zaman içinde değerleri toplayan çeşitleri vardır. , Veya sonekine sahip üyelere bakın `*Avg` `*Max` `*Total` . Üye, `FramesUsedForAverage` Bu toplama için kaç çerçeve kullanıldığını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

* [Performans izlemeleri oluşturma](../../how-tos/performance-tracing.md)
* [Model dönüştürmeyi yapılandırma](../../how-tos/conversion/configure-model-conversion.md)
