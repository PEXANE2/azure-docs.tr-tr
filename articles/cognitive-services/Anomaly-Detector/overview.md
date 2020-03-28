---
title: Anomali Algılayıcısı API'si nedir?
titleSuffix: Azure Cognitive Services
description: Zaman serisi verilerinizdeki anormallikleri belirlemek için Anomali Dedektörü API'sını kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: overview
ms.date: 12/18/2019
ms.author: aahi
ms.openlocfilehash: 9237e670dd8d43c4036f996c477948944718e3aa
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80053708"
---
# <a name="what-is-the-anomaly-detector-api"></a>Anomali Algılayıcısı API'si nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Anomali Dedektörü API'sı, zaman serisi verilerinizdeki anormallikleri makine öğrenimi ile izlemenizi ve algılamanızı sağlar. Anomaly Detector API, endüstri, senaryo veya veri hacminden bağımsız olarak verilerinize en uygun modelleri otomatik olarak tanımlayarak ve uygulayarak uyum sağlar. Zaman serisi verilerinizi kullanarak, API anomali algılama için sınırları, beklenen değerleri ve hangi veri noktalarının anomali olduğunu belirler.

![Hizmet isteklerindeki desen değişikliklerini algılama](./media/anomaly_detection2.png)

Anomaly Detector'u kullanmak makine öğreniminde önceden herhangi bir deneyim gerektirmez ve RESTful API, hizmeti uygulamalarınız ve süreçlerinize kolayca entegre etmenizi sağlar.

## <a name="features"></a>Özellikler

Anomali Dedektörü ile, zaman serisi verilerinizdeki veya gerçek zamanlı olarak meydana gelen anormallikleri otomatik olarak algılayabilirsiniz.

|Özellik  |Açıklama  |
|---------|---------|
|Anomalileri gerçek zamanlı olarak algıla. | En son verilerinizin bir anormallik olup olmadığını belirlemek için daha önce görülen veri noktalarını kullanarak akış verilerinizdeki anormallikleri algıla. Bu işlem, gönderdiğiniz veri noktalarını kullanarak bir model oluşturur ve hedef noktanın bir anormallik olup olmadığını belirler. Oluşturduğunuz her yeni veri noktasıyla API'yi arayarak verilerinizi oluşturulduğu gibi izleyebilirsiniz. |
|Veri setinizdeki anormallikleri toplu olarak algılayın. | Verilerinizboyunca var olabilecek anormallikleri algılamak için zaman serinizi kullanın. Bu işlem, tüm zaman serisi verilerinizi kullanarak bir model oluşturur ve her nokta aynı modelle analiz edilir.         |
| Verileriniz hakkında ek bilgi alın. | Verileriniz ve beklenen değerler, anomali sınırları ve konumları da dahil olmak üzere gözlenen anomaliler hakkında yararlı bilgiler alın. |
| Anomali algılama sınırlarını ayarlayın. | Anomali Dedektörü API otomatik olarak anomali algılama için sınırlar oluşturur. API'nin veri anormalliklerine karşı duyarlılığını artırmak veya azaltmak ve verilerinizi daha iyi sığdırmak için bu sınırları ayarlayın. |

## <a name="demo"></a>Tanıtım

Anomaly Detector'Un nasıl çalıştığını anlamak için bu [etkileşimli demoya](https://aka.ms/adDemo) göz atın.
Demoyu çalıştırmak için bir Anomaly Detector kaynağı oluşturmanız ve API anahtarı nı ve bitiş noktasını almanız gerekir.

## <a name="notebook"></a>Not Defteri

Anomali Dedektörü API'sini nasıl arayacağınızı öğrenmek için bu [Azure Not Defteri'ni](https://aka.ms/adNotebook)deneyin. Web tarafından barındırılan bu Jupyter Notebook, bir API isteğinin nasıl gönderilebildiğini ve sonucu nasıl görselleştirilen işeymenizi gösterir.

Not Defterini çalıştırmak için aşağıdaki adımları tamamlayın:

1. Geçerli bir Anomali Dedektörü API abonelik anahtarı ve BIR API bitiş noktası alın. Aşağıdaki bölümde kaydolmak için talimatlar vardır.
1. Oturum açın ve sağ üst köşede Klon'ı tıklatın.
1. Klon işlemini tamamlamadan önce iletişim kutusundaki "herkese açık" seçeneğini kaldırın, aksi takdirde tüm abonelik anahtarları da dahil olmak üzere not defteriniz herkese açık olacaktır.
1. **Ücretsiz bilgi işlemde Çalıştır'a** tıklayın
1. Not defterlerinden birini seçin.
1. Geçerli Anomali Dedektörü API abonelik `subscription_key` anahtarınızı değişkene ekleyin.
1. Değişkeni `endpoint` bitiş noktanızla değiştirin. Örneğin, `https://westus2.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/last/detect`
1. Üst menü çubuğunda **Hücre'yi**tıklatın ve ardından **Tümlerini Çalıştır'ı tıklatın.**

## <a name="workflow"></a>İş akışı

Anomaly Detector API kolay HTTP istekleri yapabilir ve JSON ayrıştırmak herhangi bir programlama dilinden aramak için yapım, bir RESTful web hizmetidir.

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

Kaydolduktan sonra:

1. Zaman serisi verilerinizi alın ve geçerli bir JSON biçimine dönüştürün. En iyi sonuçları elde etmek için verilerinizi hazırlarken [en iyi uygulamaları](concepts/anomaly-detection-best-practices.md) kullanın.
1. Verilerinizi içeren Anomali Dedektörü API'sine bir istek gönderin.
1. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="algorithms"></a>Algoritmalar

* Kullanılan algoritmalar hakkında bilgi için aşağıdaki teknik bloglara bakın:
    * [Azure Anomali Algılayıcısı API’sine giriş](https://techcommunity.microsoft.com/t5/AI-Customer-Engineering-Team/Introducing-Azure-Anomaly-Detector-API/ba-p/490162)
    * [Azure Anomali Dedektöründe SR-CNN algoritması genel bakış](https://techcommunity.microsoft.com/t5/AI-Customer-Engineering-Team/Overview-of-SR-CNN-algorithm-in-Azure-Anomaly-Detector/ba-p/982798)

Microsoft tarafından geliştirilen SR-CNN algoritmaları hakkında daha fazla bilgi edinmek için [Microsoft'taki Zaman Serisi Anomali Algılama Hizmeti](https://arxiv.org/abs/1906.03821) 'ni (KDD 2019 tarafından kabul edilir) okuyabilirsiniz.


> [!VIDEO https://www.youtube.com/embed/ERTaAnwCarM]

## <a name="join-the-anomaly-detector-community"></a>Anomali Algılayıcısı topluluğuna katılın

* Microsoft [Teams'te Anomali Dedektörü Danışmanları grubuna](https://aka.ms/AdAdvisorsJoin) katılın
* Seçili [kullanıcı tarafından oluşturulan içeriği](user-generated-content.md) görme

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Anomali Dedektörü REST API'yi kullanarak zaman serisi verilerinizdeki anormallikleri tespit edin](quickstarts/detect-data-anomalies-csharp.md)
* Anomali Dedektörü API [online demo](https://notebooks.azure.com/AzureAnomalyDetection/projects/anomalydetector)
* Anomali Dedektörü [REST API referansı](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)
