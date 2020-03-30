---
title: Azure Akış Analizi'nde Makine Öğrenimi işlevlerini ölçeklendirin
description: Bu makalede, bölümleme ve akış birimlerini yapılandırarak Makine Öğrenimi işlevlerini kullanan Akış Analizi işlerinin nasıl ölçeklendirilenanlatılmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: 5b08625d055063b3804a35a3344ff01c7edb79de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066998"
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-studio-classic-functions"></a>Azure Machine Learning Studio (klasik) işlevleriyle Stream Analytics işinizi ölçeklendirin

> [!TIP]
> Gelişmiş performans ve güvenilirlik için Azure Machine Learning Studio (klasik) UDF yerine [Azure Machine Learning UDF'leri](machine-learning-udf.md) kullanmanız önerilir.

Bu makalede, Azure Makine Öğrenimi işlevlerini kullanan Azure Akış Analizi işlerinin nasıl verimli bir şekilde ölçeklendirilenir. Genel olarak Stream Analytics işlerinin nasıl ölçeklendirilenhakkında bilgi için makale [ölçeklendirme işlerine](stream-analytics-scale-jobs.md)bakın.

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Stream Analytics'te Azure Machine Learning işlevi nedir?

Stream Analytics'teki Makine Öğrenimi işlevi, Stream Analytics sorgu dilinde normal bir işlev çağrısı gibi kullanılabilir. Ancak, bu işlev çağrıları aslında Azure Machine Learning Web Service istekleridir.

Aynı web hizmeti API çağrısında birden çok satırı bir araya getirerek Machine Learning web hizmeti isteklerinin iş kısmını geliştirebilirsiniz. Bu gruplandırmaya mini toplu iş denir. Daha fazla bilgi için [Azure Machine Learning Studio (klasik) Web Hizmetleri'ne](../machine-learning/studio/consume-web-services.md)bakın. Stream Analytics'teki Azure Machine Learning Studio (klasik) desteği önizlemede.

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>Machine Learning işlevleriyle Bir Akış Analizi işini yapılandırma

Stream Analytics işiniz tarafından kullanılan Machine Learning işlevini yapılandırmak için iki parametre vardır:

* Machine Learning işlevinin toplu boyutu çağırır.
* Stream Analytics işi için sağlanan Akış Birimi (SUs) sayısı.

SUs'lar için uygun değerleri belirlemek için, Stream Analytics işinin gecikmesini mi yoksa her SU'nun masını mı optimize etmek istediğinize karar verin. SUs'lar, iyi bölümlenmiş bir Akış Analizi sorgusunun verisini artırmak için her zaman bir işe eklenebilir. Ek SUs'lar işi çalıştırma maliyetini artırır.

Stream Analytics işiniz için gecikme *toleransını* belirleyin. Toplu iş boyutunu artırmak, Azure Machine Learning isteklerinizin gecikmesini ve Stream Analytics işinin gecikmesini artırır.

Toplu iş boyutunun artırılması, Akış Analizi işinin **aynı sayıda** Machine Learning web hizmeti isteğiyle daha fazla **olayı** işlemesine olanak tanır. Machine Learning web hizmeti gecikmesinin artması genellikle toplu iş boyutunun artmasına doğru değildir. 

Herhangi bir durumda bir Machine Learning web hizmeti için en uygun maliyetli toplu iş boyutunu göz önünde bulundurmak önemlidir. Web hizmeti istekleri için varsayılan toplu iş boyutu 1000'dir. [Stream Analytics REST API'sini](https://docs.microsoft.com/previous-versions/azure/mt653706(v=azure.100) "Akış Analizi REST API") veya Stream [Analytics için PowerShell istemcisini](stream-analytics-monitor-and-manage-jobs-use-powershell.md)kullanarak bu varsayılan boyutu değiştirebilirsiniz.

Toplu iş boyutuna karar verdikten sonra, işlevin saniyede işlemesi gereken olay sayısına bağlı olarak akış birimi (SUs) sayısını ayarlayabilirsiniz. Akış birimleri hakkında daha fazla bilgi için [Bkz. Akış Analizi ölçeğinde işler.](stream-analytics-scale-jobs.md)

Her 6 SUs, Machine Learning web hizmetine 20 eşzamanlı bağlantı alır. Ancak, 1 SU iş ve 3 SU işleri 20 eşzamanlı bağlantıları olsun.  

Uygulamanız saniyede 200.000 olay oluşturuyorsa ve toplu iş boyutu 1000 ise, ortaya çıkan web hizmeti gecikme süresi 200 ms'dir. Bu oran, her bağlantının Makine Öğrenimi web hizmetine saniyede beş istekte bulunabileceği anlamına gelir. 20 bağlantıyla Stream Analytics işi 200 ms'de 20.000, saniyede 100.000 etkinlik işleyebilir.

Saniyede 200.000 olayı işlemek için Stream Analytics işinin 12 SUs'a ulaşan 40 eşzamanlı bağlantıya ihtiyacı vardır. Aşağıdaki diyagram, Stream Analytics işinden Machine Learning web hizmeti bitiş noktasına olan istekleri göstermektedir – Her 6 SUs'un Machine Learning web hizmetine en üst düzeyde 20 eşzamanlı bağlantısı vardır.

![Makine Öğrenimi Fonksiyonları ile Ölçek Akışı Analizi iki iş örneği](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "Makine Öğrenimi Fonksiyonları ile Ölçek Akışı Analizi iki iş örneği")

Genel olarak, toplu boyut için ***B,*** b toplu boyutunda web hizmeti gecikmesi milisaniye cinsinden ***L,*** ***N*** SUs ile bir Stream Analytics işinin iş akışı:

![Makine Öğrenme Fonksiyonları Formülü ile Ölçek Akışı Analizi](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Makine Öğrenme Fonksiyonları Formülü ile Ölçek Akışı Analizi")

Machine Learning web hizmetindeki 'maksimum eşzamanlı çağrıları' da yapılandırabilirsiniz. Bu parametreyi maksimum değere (şu anda 200) ayarlamak önerilir.

Bu ayar hakkında daha fazla bilgi için Machine [Learning Web Services için Ölçekleme makalesini](../machine-learning/studio/scaling-webservice.md)inceleyin.

## <a name="example--sentiment-analysis"></a>Örnek – Duygu Analizi
Aşağıdaki örnek, [Akış Analizi Makine Öğrenme entegrasyon öğreticisinde](stream-analytics-machine-learning-integration-tutorial.md)açıklandığı gibi, duygu analizi Machine Learning işlevine sahip bir Stream Analytics işini içerir.

Sorgu, aşağıdaki örnekte gösterildiği gibi, **duygu** işlevi tarafından izlenen basit bir tam bölümlü sorgudur:

```SQL
    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery
```

Saniyede 10.000 tweet hızında tweetlerin duyarlılık analizini yapan bir Stream Analytics işi oluşturmak için gerekli yapılandırmayı inceleyelim.

1 SU kullanarak, bu Stream Analytics işi trafiği idare edebilir mi? İş, varsayılan 1000 toplu iş boyutunu kullanarak girişi takip edebilir. Duygu analizi Machine Learning web hizmetinin varsayılan gecikmesi (varsayılan toplu iş boyutu 1000 olan) bir saniyeden fazla gecikme yaratmaz.

Akış Analizi işinin **genel** veya uçlardan uca gecikmesi genellikle birkaç saniye olur. Bu Stream Analytics işine, *özellikle* de Machine Learning işlev çağrılarına daha ayrıntılı bir göz atın. 1000 toplu iş boyutuyla, 10.000 etkinlik iş bir iş bölümü web hizmeti için yaklaşık 10 istek alır. Bir SU ile bile, bu giriş trafiğini karşılamak için yeterli eşzamanlı bağlantı vardır.

Giriş olay hızı 100 x artarsa, Akış Analizi işinin saniyede 1.000.000 tweet işlemesi gerekir. Artan ölçeği gerçekleştirmek için iki seçenek vardır:

1. Toplu iş boyutunu artırın.
2. Olayları paralel olarak işlemek için giriş akışını bölümle.

İlk seçenekle, iş **gecikmesi** artar.

İkinci seçenekle, daha eşzamanlı Machine Learning web hizmeti isteklerine sahip olmak için daha fazla SUs sağlamanız gerekecektir. Bu daha fazla sayıda SUs, iş **maliyetini**artırır.

Her toplu iş boyutu için aşağıdaki gecikme sonu ölçümlerini kullanarak ölçeklemeye bakalım:

| Gecikme süresi | Toplu iş boyutu |
| --- | --- |
| 200 ms | 1000-olay toplu veya altında |
| 250 ms | 5.000 etkinliklik toplu iş |
| 300 ms | 10.000 etkinliklik toplu iş |
| 500 ms | 25.000 etkinliklik toplu iş |

1. İlk seçeneği kullanma (daha fazla SUs**sağlamamak).** Toplu iş boyutu **25.000'e**çıkarılabilir. Toplu iş boyutunu bu şekilde artırmak, işin Machine Learning web hizmetine 20 eşzamanlı bağlantıyla (arama başına 500 ms gecikmeyle) 1.000.000 olayı işlemesine olanak sağlar. Yani Machine Learning web hizmeti isteklerine karşı duygu fonksiyonu istekleri nedeniyle Stream Analytics işinin ek gecikme **si 200 ms'den 500** **ms'e**çıkarılacak. Ancak, Machine Learning web hizmetleri bir isteğin taşıma boyutunun 4 MB veya daha küçük olmasını gerektirdiğinden ve web hizmeti 100 saniye çalışmadan sonra zaman adedi istediği için toplu iş boyutu sonsuz olarak **artırılamaz.**
1. İkinci seçeneği kullanarak, toplu iş boyutu 1000 olarak bırakılır, 200-ms web hizmeti gecikme sayarı ile, web hizmeti ne 20 eşzamanlı bağlantıları 1000 * 20 * 5 olaylar = 100.000 saniyede işlemek mümkün olacaktır. Yani saniyede 1.000.000 olayı işlemek için, işin 60 SUs'a ihtiyacı var. İlk seçenekle karşılaştırıldığında, Stream Analytics işi daha fazla web hizmeti toplu iş isteği nde bulunarak daha yüksek bir maliyet oluşturur.

Aşağıda, farklı SUs'lar ve toplu iş boyutları (saniyede olay sayısında) için Stream Analytics işinin iş akışı için bir tablo yer almaktadır.

| toplu iş boyutu (ML gecikme) | 500 (200 ms) | 1.000 (200 ms) | 5.000 (250 ms) | 10.000 (300 ms) | 25.000 (500 ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5.000 |20.000 |30,000 |50.000 |
| **3 SUs** |2,500 |5.000 |20.000 |30,000 |50.000 |
| **6 SUs** |2,500 |5.000 |20.000 |30,000 |50.000 |
| **12 SUs** |5.000 |10,000 |40,000 |60.000 |100.000 |
| **18 SUs** |7.500 |15.000 |60.000 |90,000 |150.000 |
| **24 SUs** |10,000 |20.000 |80,000 |120,000 |200,000 |
| **…** |… |… |… |… |… |
| **60 SUs** |25,000 |50.000 |200,000 |300,000 |500.000 |

Şimdiye kadar, Stream Analytics'te Machine Learning'in nasıl çalıştığını iyi anlamış olmalısınız. Akış Analizi'nin veri kaynaklarından verileri "çektiğini" ve her "çekmenin" Akış Analizi işinin işlemesi için bir dizi olayı döndürtettiğini de büyük olasılıkla anlayabilirsiniz. Bu çekme modeli Machine Learning web hizmeti isteklerini nasıl etkiler?

Normalde, Machine Learning işlevleri için belirlediğimiz toplu iş boyutu, her Stream Analytics işi "çekme" tarafından döndürülen olay sayısıyla tam olarak bölünemez. Bu durumda, Machine Learning web hizmeti "kısmi" toplu işlerle çağrılır. Kısmi toplu iş partilerinin kullanılması, çekmeden çekmeye kadar olan olayları biraraya getirmek için ek iş gecikmesi yüküne neden olur.

## <a name="new-function-related-monitoring-metrics"></a>Yeni işlevle ilgili izleme ölçümleri
Bir Akış Analizi işinin Monitör alanında, işleve ilişkin üç ek ölçüm eklendi. Aşağıdaki grafikte gösterildiği gibi **Fonksiyon İsteKLerİ,** **İşLEV EtkİnLİkLerİ** ve **BAŞARıSıZ İşLEV İsteMLerİdir.**

![Makine Öğrenme Fonksiyonları Ölçümleri ile Ölçek Akışı Analizi](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Makine Öğrenme Fonksiyonları Ölçümleri ile Ölçek Akışı Analizi")

Aşağıdaki gibi tanımlanır:

**İşLEV İsteKLerİ**: İşlev isteklerinin sayısı.

**İşLEV EtkİnLİkLerİ**: İşlev isteklerindeki sayı olayları.

**BAŞARıSıZ İşLEV İsteKLerİ**: Başarısız işlev isteklerinin sayısı.

## <a name="key-takeaways"></a>Anahtar Paketler

Bir Akış Analizi işini Machine Learning işlevleriyle ölçeklendirmek için aşağıdaki faktörleri göz önünde bulundurun:

1. Giriş olay hızı.
2. Çalışan Stream Analytics işi için tolere edilen gecikme (ve böylece Machine Learning web hizmeti isteklerinin toplu boyutu).
3. Sağlanan Akış Analizi SUs'ları ve Machine Learning web hizmeti isteklerinin sayısı (işleçle ilgili ek maliyetler).

Örnek olarak tam bölümlü bir Akış Analizi sorgusu kullanılmıştır. Daha karmaşık bir sorgu gerekiyorsa, Azure Akışı Analizi forumu Stream [Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) ekibinden ek yardım almak için harika bir kaynaktır.

## <a name="next-steps"></a>Sonraki adımlar
Stream Analytics hakkında daha fazla bilgi edinmek için bkz:

* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
