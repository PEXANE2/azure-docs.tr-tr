---
title: Azure Machine Learning Anomaly Detection API - Ekip Veri Bilimi Süreci
description: Anomali Algılama API' si, zaman serileri verilerindeki anormallikleri zaman içinde eşit aralıklı sayılarla algılayan Microsoft Azure Machine Learning ile oluşturulmuş bir örnektir.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=alokkirpal, previous-ms.author=alok
ms.openlocfilehash: a09094cf0d1bd3c2e299e968d7de8410dcd9c3cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721889"
---
# <a name="machine-learning-anomaly-detection-api"></a>Makine Öğrenimi Anomali Algılama API

> [!NOTE]
> Bu madde bakım altındadır. İş, işletim ve IoT ölçümlerinden kaynaklanan anormallikleri tespit etmek için Azure Bilişsel Hizmetleri altındaki Makine Öğrenimi algoritmaları galerisi tarafından desteklenen [Anomaly Detector API hizmetini](https://azure.microsoft.com/services/cognitive-services/anomaly-detector/) kullanmanızı öneririz.

## <a name="overview"></a>Genel Bakış
[Anomali Algılama API'](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2) si, zaman serileri verilerindeki anormallikleri zaman içinde eşit aralıklı sayılarla algılayan Azure Machine Learning ile oluşturulmuş bir örnektir.

Bu API, zaman serisi verilerinde aşağıdaki anormal desenleri algılayabilir:

* **Pozitif ve olumsuz eğilimler**: Örneğin, bir bellek sızıntısının göstergesi olabileceğinden, bilgi işlemde bellek kullanımını izlerken bir artış eğilimi ilgi çekici olabilir,
* **Dinamik değerler aralığındaki değişiklikler**: Örneğin, bir bulut hizmeti tarafından atılan özel durumları izlerken, dinamik değerler aralığındaki herhangi bir değişiklik hizmetin sağlığındaki kararsızlığı gösterebilir ve
* **Ani Artışlar ve Dipler**: Örneğin, bir e-ticaret sitesindeki oturum açma hatalarının veya ödeme sayısının izlenmesi, ani artışlar veya dipler anormal davranışgösterebilir.

Bu makine öğrenimi dedektörleri zaman içinde değerlerdeki bu değişiklikleri izler ve değerlerinde devam eden değişiklikleri anomali puanları olarak rapor eder. Onlar adhoc eşik atonlama gerektirmez ve puanları yanlış pozitif oranı kontrol etmek için kullanılabilir. Anomali algılama API zaman içinde KP'ler izleyerek hizmet izleme gibi çeşitli senaryolarda yararlıdır, arama sayısı, tıklama sayısı, bellek, CPU, dosya okuma, vb gibi sayaçları aracılığıyla performans izleme gibi ölçümler aracılığıyla kullanım izleme. zaman içinde.

Anomali Algılama sunan başlamak için yararlı araçlar ile birlikte gelir.

* [Web uygulaması,](https://anomalydetection-aml.azurewebsites.net/) verilerinizdeki anomali algılama API'lerinin sonuçlarını değerlendirmenize ve görselleştirmenize yardımcı olur.

> [!NOTE]
> [Bu API](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2) tarafından desteklenen **BT Anomali Öngörüleri çözümdeneyin**
>
<!-- This Solution is no longer available
> To get this end to end solution deployed to your Azure subscription <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank">**Start here >**</a>
-->

## <a name="api-deployment"></a>API Dağıtımı
API'yi kullanabilmek için, bu api'yi Azure Makine Öğrenimi web hizmeti olarak barındırılacak Azure aboneliğinize dağıtmanız gerekir.  Bunu [Azure AI Galerisi'nden](https://gallery.cortanaintelligence.com/MachineLearningAPI/Anomaly-Detection-2)yapabilirsiniz.  Bu, biri mevsimsellik algılamalı anormallik algılama, diğeri mevsimsellik algılaması için olmak üzere azure aboneliğinize iki Azure Machine Learning Studio (klasik) Web Hizmeti (ve bunların ilgili kaynaklarını) dağıtacaktır.  Dağıtım tamamlandıktan sonra, API'lerinizi Azure Machine [Learning Studio (klasik) web hizmetleri](https://services.azureml.net/webservices/) sayfasından yönetebilirsiniz.  Bu sayfadan, uç nokta konumlarınızı, API tuşlarınızı ve API'yi aramak için örnek kodunuzu bulabilirsiniz.  Daha ayrıntılı talimatlar [burada](https://docs.microsoft.com/azure/machine-learning/machine-learning-manage-new-webservice)mevcuttur.

## <a name="scaling-the-api"></a>API'yi ölçekleme
Varsayılan olarak, dağıtımınızda ayda 1.000 işlem ve 2 işlem saati/ay içeren ücretsiz bir Geliştirme/Test faturalandırma planı olacaktır.  İhtiyaçlarınıza göre başka bir plana yükseltebilirsiniz.  Farklı planların fiyatlandırması ile ilgili [ayrıntılara "Üretim](https://azure.microsoft.com/pricing/details/machine-learning/) Web API fiyatlandırması" altında buradan ulaşabilirsiniz.

## <a name="managing-aml-plans"></a>AML Planlarını Yönetme
Faturalandırma planınızı [buradan](https://services.azureml.net/plans/)yönetebilirsiniz.  Plan adı, API'yi dağıtırken seçtiğiniz kaynak grubu adını ve aboneliğinize özgü bir dizeyi temel alır.  Planınızı nasıl yükselteceklerine ilişkin talimatlara ["Faturalandırma](https://docs.microsoft.com/azure/machine-learning/machine-learning-manage-new-webservice) planlarını yönetme" bölümünden buradan ulaşabilirsiniz.

## <a name="api-definition"></a>API Tanımı
Web hizmeti, web veya mobil uygulama, R, Python, Excel, vb. gibi farklı şekillerde tüketilebilen HTTPS üzerinden REST tabanlı bir API sağlar.  Zaman serisi verilerinizi BIR REST API çağrısı yla bu hizmete gönderirsiniz ve bu veriler aşağıda açıklanan üç anomali türünün bir birleşimini çalıştırAr.

## <a name="calling-the-api"></a>API'yi arama
API'yi aramak için bitiş noktası konumunu ve API anahtarını bilmeniz gerekir.  Bu iki gereksinim, API'yi aramak için örnek kodla birlikte [Azure Machine Learning Studio (klasik) web hizmetleri](https://services.azureml.net/webservices/) sayfasından edinilebilir.  İstenilen API'ye gidin ve ardından bunları bulmak için "Tüket" sekmesini tıklatın.  API'yi Swagger API (yani URL parametresi `format=swagger`ile) veya Swagger OLMAYAN API (yani `format` URL parametresi olmadan) olarak arayabilirsiniz.  Örnek kod Swagger biçimini kullanır.  Aşağıda swagger olmayan biçimde bir örnek istek ve yanıt verilmiştir.  Bu örnekler mevsimsellik bitiş noktasına kadardır.  Mevsimsellik dışı bitiş noktası benzerdir.

### <a name="sample-request-body"></a>Örnek İstek Gövdesi
İstek iki nesne `Inputs` içerir: `GlobalParameters`ve .  Aşağıdaki örnek istekte, bazı parametreler açıkça gönderilirken, diğerleri gönderilmez (her bitiş noktası için parametrelerin tam listesi için aşağı kaydırın).  İstekte açıkça gönderilmemiş parametreler aşağıda verilen varsayılan değerleri kullanır.

    {
                "Inputs": {
                        "input1": {
                                "ColumnNames": ["Time", "Data"],
                                "Values": [
                                        ["5/30/2010 18:07:00", "1"],
                                        ["5/30/2010 18:08:00", "1.4"],
                                        ["5/30/2010 18:09:00", "1.1"]
                                ]
                        }
                },
        "GlobalParameters": {
            "tspikedetector.sensitivity": "3",
            "zspikedetector.sensitivity": "3",
            "bileveldetector.sensitivity": "3.25",
            "detectors.spikesdips": "Both"
        }
    }

### <a name="sample-response"></a>Örnek Yanıt
`ColumnNames` Alanı görmek için isteğinize bir `details=true` URL parametresi eklemeniz gerekir.  Bu alanların her birinin arkasındaki anlam için aşağıdaki tablolara bakın.

    {
        "Results": {
            "output1": {
                "type": "table",
                "value": {
                    "Values": [
                        ["5/30/2010 6:07:00 PM", "1", "1", "0", "0", "-0.687952590518378", "0", "-0.687952590518378", "0", "-0.687952590518378", "0"],
                        ["5/30/2010 6:08:00 PM", "1.4", "1.4", "0", "0", "-1.07030497733224", "0", "-0.884548154298423", "0", "-1.07030497733224", "0"],
                        ["5/30/2010 6:09:00 PM", "1.1", "1.1", "0", "0", "-1.30229513613974", "0", "-1.173800281031", "0", "-1.30229513613974", "0"]
                    ],
                    "ColumnNames": ["Time", "OriginalData", "ProcessedData", "TSpike", "ZSpike", "BiLevelChangeScore", "BiLevelChangeAlert", "PosTrendScore", "PosTrendAlert", "NegTrendScore", "NegTrendAlert"],
                    "ColumnTypes": ["DateTime", "Double", "Double", "Double", "Double", "Double", "Int32", "Double", "Int32", "Double", "Int32"]
                }
            }
        }
    }


## <a name="score-api"></a>Puan API'si
Puan API'si, mevsimsel olmayan zaman serileri verilerinde anomali algılaması için kullanılır. API, veriler üzerinde bir dizi anomali dedektörü çalıştırıyor ve anomali skorlarını döndürür.
Aşağıdaki şekil, Puan API'sinin algılayabildiği anormalliklerin bir örneğini gösterir. Bu zaman serisi iki farklı düzey değişiklikleri ve üç ani vardır. Kırmızı nokta, düzey değişikliğinin algılandığı zamanı gösterirken, siyah nokta tespit edilen ani artışları gösterir.
![Puan API'si][1]

### <a name="detectors"></a>Algılayıcılar
Anomali algılama API'si dedektörleri üç geniş kategoride destekler. Her dedektör için belirli giriş parametreleri ve çıkışları hakkında ayrıntılar aşağıdaki tabloda bulunabilir.

| Dedektör Kategorisi | Dedektörü | Açıklama | Giriş Parametreleri | Çıkışlar |
| --- | --- | --- | --- | --- |
| Başak Dedektörleri |TSpike Dedektörü |Birinci ve üçüncü dörtte birolan değerlere göre ani ve dipleri tespit et |*tspikedetector.sensitivity:* aralık 1-10, varsayılan olarak insa değeri alır: 3; Daha yüksek değerler daha fazla ekstrem değerleri yakalar ve böylece daha az hassas hale getirir |TSpike: ikili değerler – '1' bir başak/dip algılanırsa, '0' aksi takdirde |
| Başak Dedektörleri | ZSpike Dedektörü |Veri noktalarının ortalamalarından ne kadar uzakta olduğuna bağlı olarak ani artışları ve dipleri algılama |*zspikedetector.sensitivity:* aralık 1-10, varsayılan olarak insa değeri almak: 3; Daha yüksek değerler daha az hassas hale daha aşırı değerleri yakalar |ZSpike: ikili değerler – '1' bir başak/dip algılanırsa, '0' aksi takdirde |
| Yavaş Trend Dedektörü |Yavaş Trend Dedektörü |Ayarlanan duyarlılığa göre yavaş pozitif eğilimi algılama |*trenddetector.sensitivity:* dedektör puanı eşik (varsayılan: 3.25, 3.25 - 5 seçmek için makul bir aralıktır; Ne kadar yüksek olursa o kadar az hassas) |tscore: eğilim de anomali puanı temsil eden kayan sayı |
| Seviye Değişim Dedektörleri | Çift Yönlü Seviye Değişim Dedektörü |Ayarlanan duyarlılığa göre hem yukarı hem de aşağı düzey değişimini algıla |*bileveldetector.sensitivity:* dedektör puanı eşik (varsayılan: 3.25, 3.25 - 5 seçmek için makul bir aralıktır; Ne kadar yüksek olursa o kadar az hassas) |rpscore: yukarı ve aşağı düzey değişiminde anomali puanını temsil eden kayan sayı |

### <a name="parameters"></a>Parametreler
Bu giriş parametreleri hakkında daha ayrıntılı bilgi aşağıdaki tabloda listelenmiştir:

| Giriş Parametreleri | Açıklama | Varsayılan Ayar | Tür | Geçerli Aralık | Önerilen Aralık |
| --- | --- | --- | --- | --- | --- |
| dedektörler.historywindow |Anomali puan hesaplaması için kullanılan tarih (veri puanlarının #'sında) |500 |integer |10-2000 |Zaman serisi bağımlı |
| dedektörler.spikesdips | Yalnızca ani artışlar algılamak için olsun, sadece dips, ya da her ikisi |Her ikisi de |Numaralandırılmış |Her ikisi de, Sivri, Dips |Her ikisi de |
| bileveldetector.sensitivity |Çift yönlü seviye değiştirme dedektörü için hassasiyet. |3,25 |double |None |3.25-5 (Daha küçük değerler daha hassas demektir) |
| trenddetector.sensitivity |Pozitif trend dedektörü için duyarlılık. |3,25 |double |None |3.25-5 (Daha küçük değerler daha hassas demektir) |
| tspikedetector.sensitivity |TSpike Dedektörü için Hassasiyet |3 |integer |1-10 |3-5 (Daha küçük değerler daha hassas anlamına gelir) |
| zspikedetector.sensitivity |ZSpike Dedektörü için Hassasiyet |3 |integer |1-10 |3-5 (Daha küçük değerler daha hassas anlamına gelir) |
| postprocess.tailRows |Çıktı sonuçlarında tutulacak en son veri noktalarının sayısı |0 |integer |0 (tüm veri noktalarını saklayın) veya sonuçlarda tutmak için puan sayısını belirtin |Yok |

### <a name="output"></a>Çıktı
API, zaman serisi verilerinizdeki tüm dedektörleri çalıştırıyor ve zamandaki her nokta için anomali puanları ve ikili başak göstergeleri döndürür. Aşağıdaki tabloda API çıktıları listeledir.

| Çıkışlar | Açıklama |
| --- | --- |
| Zaman |Ham verilerden gelen zaman damgaları veya toplama (ve/veya) eksik veri imputasyonu uygulandığında toplu (ve/veya) imputed veriler |
| Veri |Ham verilerden veya toplu (ve/veya) toplanan verilerden elde edilen değerler, toplama (ve/veya) eksik veri imputasyonu uygulandığında |
| TSpike |TSpike Dedektörü tarafından bir çivi algılanıp algılmadığını belirtmek için ikili gösterge |
| ZSpike |ZSpike Dedektörü tarafından bir çivi algılanıp algılmadığını gösteren ikili gösterge |
| rpscore |Çift yönlü düzey değişiminde anomali puanını temsil eden kayan bir sayı |
| rpalert |Giriş hassasiyetine göre çift yönlü bir seviye değişikliği anomalisi olduğunu gösteren 1/0 değeri |
| tscore |Pozitif eğilimde anomali puanını temsil eden kayan bir sayı |
| talert |Giriş hassasiyetine dayalı pozitif bir eğilim anomalisi olduğunu gösteren 1/0 değeri |

## <a name="scorewithseasonality-api"></a>PuanSeasonality API ile
ScoreWithSeasonality API mevsimsel desenleri olan zaman serilerinde anomali tespiti çalıştırmak için kullanılır. Bu API mevsimsel desenlerdeki sapmaları algılamak için yararlıdır.
Aşağıdaki şekil mevsimsel zaman serilerinde saptanan anomalilerin bir örneğini göstermektedir. Zaman serisi bir başak (ilk siyah nokta), iki dips (ikinci siyah nokta ve sonunda bir) ve bir seviye değişikliği (kırmızı nokta) vardır. Hem zaman serisinin ortasındaki daldırma hem de seviye değişikliği, mevsimsel bileşenler seriden çıkarıldıktan sonra fark edilebilir.
![Mevsimsellik API'si][2]

### <a name="detectors"></a>Algılayıcılar
Mevsimsellik bitiş noktasındaki dedektörler mevsimsellik dışı uç noktadakilere benzer, ancak biraz farklı parametre adlarıyla (aşağıda listelenmiştir).

### <a name="parameters"></a>Parametreler

Bu giriş parametreleri hakkında daha ayrıntılı bilgi aşağıdaki tabloda listelenmiştir:

| Giriş Parametreleri | Açıklama | Varsayılan Ayar | Tür | Geçerli Aralık | Önerilen Aralık |
| --- | --- | --- | --- | --- | --- |
| preprocess.aggregationInterval |Giriş zaman serilerini toplama için saniye cinsinden toplama aralığı |0 (toplama yapılmaz) |integer |0: atlama toplama, > 0 aksi |5 dakika ile 1 gün arası, zaman serisi bağımlı |
| preprocess.aggregationFunc |Belirtilen Toplama Aralığına veri toplama için kullanılan fonksiyon |ortalama |Numaralandırılmış |ortalama, toplam, uzunluk |Yok |
| önişlem.replaceMissing |Eksik verileri ortaya çıkarmak için kullanılan değerler |lkv (bilinen son değer) |Numaralandırılmış |sıfır, lkv, ortalama |Yok |
| dedektörler.historywindow |Anomali puan hesaplaması için kullanılan tarih (veri puanlarının #'sında) |500 |integer |10-2000 |Zaman serisi bağımlı |
| dedektörler.spikesdips | Yalnızca ani artışlar algılamak için olsun, sadece dips, ya da her ikisi |Her ikisi de |Numaralandırılmış |Her ikisi de, Sivri, Dips |Her ikisi de |
| bileveldetector.sensitivity |Çift yönlü seviye değiştirme dedektörü için hassasiyet. |3,25 |double |None |3.25-5 (Daha küçük değerler daha hassas demektir) |
| postrenddetector.sensitivity |Pozitif trend dedektörü için duyarlılık. |3,25 |double |None |3.25-5 (Daha küçük değerler daha hassas demektir) |
| negtrenddetector.sensitivity |Negatif trend dedektörü için duyarlılık. |3,25 |double |None |3.25-5 (Daha küçük değerler daha hassas demektir) |
| tspikedetector.sensitivity |TSpike Dedektörü için Hassasiyet |3 |integer |1-10 |3-5 (Daha küçük değerler daha hassas anlamına gelir) |
| zspikedetector.sensitivity |ZSpike Dedektörü için Hassasiyet |3 |integer |1-10 |3-5 (Daha küçük değerler daha hassas anlamına gelir) |
| mevsimsellik.enable |Mevsimsellik analizinin yapılıp yapılmayacağı |true |boole |doğru, yanlış |Zaman serisi bağımlı |
| mevsimsellik.numMevsimsellik |Tespit edilecek maksimum periyodik döngü sayısı |1 |integer |1, 2 |1-2 |
| mevsimsellik.transform |Anomali tespiti uygulanmadan önce mevsimsel (ve) trend bileşenlerinin çıkarılıp çıkarılmayacağı |deseason |Numaralandırılmış |yok, deseason, deseasontrend |Yok |
| postprocess.tailRows |Çıktı sonuçlarında tutulacak en son veri noktalarının sayısı |0 |integer |0 (tüm veri noktalarını saklayın) veya sonuçlarda tutmak için puan sayısını belirtin |Yok |

### <a name="output"></a>Çıktı
API, zaman serisi verilerinizdeki tüm dedektörleri çalıştırıyor ve zamandaki her nokta için anomali puanları ve ikili başak göstergeleri döndürür. Aşağıdaki tabloda API çıktıları listeledir.

| Çıkışlar | Açıklama |
| --- | --- |
| Zaman |Ham verilerden gelen zaman damgaları veya toplama (ve/veya) eksik veri imputasyonu uygulandığında toplu (ve/veya) imputed veriler |
| Orijinal Veriler |Ham verilerden veya toplu (ve/veya) toplanan verilerden elde edilen değerler, toplama (ve/veya) eksik veri imputasyonu uygulandığında |
| İşlenmiş Veriler |Aşağıdaki seçeneklerden biri: <ul><li>Önemli mevsimsellik tespit edildiyse ve sezon luk opsiyon seçiliyse mevsimsel olarak ayarlanmış zaman serisi;</li><li>mevsimsellik ayarlanmış ve detrended zaman serisi önemli mevsimsellik tespit edilmiş ve deseasontrend seçeneği seçili</li><li>aksi takdirde, bu seçenek OriginalData ile aynıdır</li> |
| TSpike |TSpike Dedektörü tarafından bir çivi algılanıp algılmadığını belirtmek için ikili gösterge |
| ZSpike |ZSpike Dedektörü tarafından bir çivi algılanıp algılmadığını gösteren ikili gösterge |
| BiLevelChangeScore |Düzey değişikliğinde anomali puanını temsil eden kayan bir sayı |
| BiLevelChangeAlert |Giriş hassasiyetine dayalı bir seviye değişikliği anomalisi olduğunu gösteren 1/0 değeri |
| PosTrendScore |Pozitif eğilimde anomali puanını temsil eden kayan bir sayı |
| PosTrendAlert |Giriş hassasiyetine dayalı pozitif bir eğilim anomalisi olduğunu gösteren 1/0 değeri |
| NegTrendScore |Negatif eğilimde anomali puanını temsil eden kayan bir sayı |
| NegTrendAlert |Giriş hassasiyetine dayalı negatif bir eğilim anomalisi olduğunu gösteren 1/0 değeri |

[1]: ./media/apps-anomaly-detection-api/anomaly-detection-score.png
[2]: ./media/apps-anomaly-detection-api/anomaly-detection-seasonal.png

