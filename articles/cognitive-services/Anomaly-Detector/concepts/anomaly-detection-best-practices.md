---
title: Anomali Algılayıcısı API'sini kullanırken en iyi yöntemler
titleSuffix: Azure Cognitive Services
description: Anomali Dedektörü API ile anomalileri tespit ederken en iyi uygulamalar hakkında bilgi edinin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 9407f2fc9375765efb6eb9688b3ebfeef24ba90a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67721630"
---
# <a name="best-practices-for-using-the-anomaly-detector-api"></a>Anomali Dedektörü API'sını kullanmak için en iyi uygulamalar

Anomali Dedektörü API'si vatansız bir anomali tespit servisidir. Sonuçlarının doğruluğu ve performansı aşağıdakilerden etkilenebilir:

* Zaman serisi verilerinizin nasıl hazırlandığı.
* Kullanılan Anomali Dedektörü API parametreleri.
* API isteğinizdeki veri noktalarının sayısı. 

Verileriniz için en iyi sonuçları almak API'yi kullanmak için en iyi uygulamalar hakkında bilgi edinmek için bu makaleyi kullanın. 

## <a name="when-to-use-batch-entire-or-latest-last-point-anomaly-detection"></a>Toplu iş (tam) veya en son (son) nokta anomali tespiti ne zaman kullanılır

Anomali Dedektörü API'nin toplu algılama bitiş noktası, tüm kez seri verilerinizdeki anormallikleri tespit etmenizi sağlar. Bu algılama modunda, tek bir istatistiksel model oluşturulur ve veri kümesindeki her noktaya uygulanır. Zaman seriniz aşağıdaki özelliklere sahipse, verilerinizi tek bir API çağrısında önizlemek için toplu iş algılamayı kullanmanızı öneririz.

* Mevsim zaman serileri, ara sıra anomaliler.
* Sabit bir trend zaman serisi, zaman zaman ani / dips ile. 

Gerçek zamanlı veri izleme için toplu anormallik algılamayı veya yukarıdaki özelliklere sahip olmayan zaman serisi verilerinde kullanmanızı önermiyoruz. 

* Toplu işlem algılama oluşturur ve sadece bir model uygular, her nokta için algılama tüm serisi bağlamında yapılır. Zaman serisi veri eğilimleri yukarı ve aşağı mevsimsellik olmadan, bazı değişim noktaları (dips ve ani) model tarafından kaçırılmış olabilir. Benzer şekilde, veri kümesinde daha sonra olanlardan daha az önemli olan bazı değişiklik noktaları modele dahil edilecek kadar önemli sayılmayabilir.

* Toplu işlem tespiti, analiz edilen nokta sayısı nedeniyle, gerçek zamanlı veri izleme yaparken en son noktanın anomali durumunu algılamaktan daha yavaştır.

Gerçek zamanlı veri izleme için yalnızca en son veri noktanızın anomali durumunu algılamanızı öneririz. Sürekli olarak en son nokta tespiti uygulanarak, veri akışı daha verimli ve doğru bir şekilde yapılabilir.

Aşağıdaki örnekte, bu algılama modlarının performans üzerindeki etkisi açıklanmaktadır. İlk resim, daha önce görülen 28 veri noktası boyunca anomali durumunun sürekli olarak tespit edilebilmektedir. Kırmızı noktalar anomaliler.

![En son noktayı kullanarak anomali algılamasını gösteren bir görüntü](../media/last.png)

Aşağıda toplu anomali algılama kullanarak aynı veri kümesidir. İşlem için yapılan model, dikdörtgenlerle işaretlenmiş çeşitli anormallikleri göz ardı etti.

![Toplu iş yöntemini kullanarak anomali algılamasını gösteren bir görüntü](../media/entire.png)

## <a name="data-preparation"></a>Veri hazırlama

Anomaly Detector API, JSON istek nesnesine biçimlendirilmiş zaman serisi verilerini kabul eder. Bir zaman serisi sıralı sırayla zaman içinde kaydedilen herhangi bir sayısal veri olabilir. API performansını artırmak için zaman serisi verilerinizin pencerelerini Anomaly Detector API bitiş noktasına gönderebilirsiniz. Gönderebileceğiniz en az veri noktası sayısı 12'dir ve en fazla 8640 puandır. [Parçalılık,](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.granularity?view=azure-dotnet-preview) verilerinizin örneklenmiş olduğu hız olarak tanımlanır. 

Anomali Dedektörü API'sine gönderilen veri noktalarının geçerli bir Eşgüdümlü Evrensel Zaman (UTC) zaman damgası ve sayısal değeri olmalıdır. 

```json
{
    "granularity": "daily",
    "series": [
      {
        "timestamp": "2018-03-01T00:00:00Z",
        "value": 32858923
      },
      {
        "timestamp": "2018-03-02T00:00:00Z",
        "value": 29615278
      },
    ]
}
```

Verileriniz standart olmayan bir zaman aralığında örneklenirse, isteğinize `customInterval` özniteliği ekleyerek verilerinizi belirtebilirsiniz. Örneğin, diziniz her 5 dakikada bir örneklenirse, JSON isteğinize aşağıdakileri ekleyebilirsiniz:

```json
{
    "granularity" : "minutely", 
    "customInterval" : 5
}
```

### <a name="missing-data-points"></a>Eksik veri noktaları

Eksik veri noktaları eşit olarak dağıtılan zaman serisi veri kümelerinde, özellikle ince parçalı olanlarda (küçük bir örnekleme aralığı) yaygındır. Örneğin, veriler birkaç dakikada bir örneklenir). Verilerinizde beklenen puan sayısının %10'undan daha azını kaçırmak, algılama sonuçlarınız üzerinde olumsuz bir etki yaratmamalıdır. Verilerinizdeki boşlukları, daha önceki bir dönemden veri noktalarının yerine, doğrusal enterpolasyon veya hareketli bir ortalamagibi özelliklerine göre doldurmayı düşünün.

### <a name="aggregate-distributed-data"></a>Toplu dağıtılmış veriler

Anomaly Detector API en iyi eşit dağıtılmış zaman serisi üzerinde çalışır. Verileriniz rasgele dağıtılıyorsa, verilerinizi dakika başına, saatlik veya günlük gibi bir zaman birimine göre toplamanız gerekir.

## <a name="anomaly-detection-on-data-with-seasonal-patterns"></a>Mevsimsel desenleri olan verilerde anomali tespiti

Zaman serisi verilerinizin mevsimsel bir deseni (düzenli aralıklarla gerçekleşen) olduğunu biliyorsanız, doğruluğu ve API yanıt süresini artırabilirsiniz. 

JSON `period` isteğinizi ne zaman oluşturabileceğinizi belirtmek, anomali algılama gecikmesüresini %50'ye kadar azaltabilir. Bir `period` deseni yinelemek için zaman serisinin kaç veri noktası gerektiğini kabaca belirten bir tamsayıdır. Örneğin, günde bir veri noktası olan `period` bir zaman `7`serisi bir gibi olurdu , ve saatte bir nokta ile `period` `7*24`bir zaman serisi (aynı haftalık desen ile) bir olurdu . Verilerinizin desenlerinden emin değilseniz, bu parametreyi belirtmeniz gerekmez.

En iyi sonuçlar `period`için, 4 's değerinde veri noktası, artı ek bir tane sağlayın. Örneğin, yukarıda açıklandığı gibi haftalık desenli saatlik veriler istek gövdesinde 673 veri noktası sağlamalıdır (`7 * 24 * 4 + 1`).

### <a name="sampling-data-for-real-time-monitoring"></a>Gerçek zamanlı izleme için veri örneklemesi

Akış verileriniz kısa bir aralıkla (örneğin saniye veya dakika) örneklenirse, önerilen veri noktası nın gönderilmesi Anomali Dedektörü API'sının izin verilen maksimum sayısını (8640 veri noktası) aşabilir. Verileriniz kararlı bir mevsimsel desen gösteriyorsa, saat gibi daha büyük bir zaman aralığında zaman serisi verilerinizin bir örneğini göndermeyi düşünün. Verilerinizin bu şekilde örnekalınması, API yanıt süresini de belirgin bir şekilde artırabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Anomali Algılayıcısı API'si nedir?](../overview.md)
* [Hızlı başlangıç: Anomali Dedektörü REST API'yi kullanarak zaman serisi verilerinizdeki anormallikleri tespit edin](../quickstarts/detect-data-anomalies-csharp.md)
