---
title: "Hızlı başlangıç: Anomali Dedektörü REST API ve Java'yı kullanarak zaman serisi verilerinizdeki anormallikleri tespit edin"
titleSuffix: Azure Cognitive Services
description: Veri serinizdeki anormallikleri toplu olarak veya veri akışı nda algılamak için Anomali Dedektörü API'sını nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 4e7cc969ff2c36415b8fd1cebafa1873f9efd38c
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80239063"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-java"></a>Hızlı başlangıç: Anomali Dedektörü REST API ve Java'yı kullanarak zaman serisi verilerinizdeki anormallikleri tespit edin

Zaman serisi verilerinizdeki anormallikleri tespit etmek için Anomali Dedektörü API'sının iki algılama modunu kullanmaya başlamak için bu hızlı başlangıcı kullanın. Bu Java uygulaması JSON biçimlendirilmiş zaman serisi verilerini içeren iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıktısı                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Anomalileri toplu olarak algılama                        | Zaman serisi verilerindeki her veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı ve algılanan anomalilerin konumları. |
| En son veri noktasının anomali durumunu algılama | Zaman serisi verilerindeki en son veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı.                                                                                                                                         |

 Bu uygulama Java'da yazılmış olsa da, API çoğu programlama diliyle uyumlu bir RESTful web hizmetidir. Bu hızlı başlatmanın kaynak kodunu [GitHub'da](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/java-detect-anomalies.java)bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- [Java&trade; Geliştirme Kiti(JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya daha sonra.
- Anomali dedektörü anahtarı ve bitiş noktası
- Bu kütüphaneleri Maven Deposu'ndan alma
    - [Java paketinde JSON](https://mvnrepository.com/artifact/org.json/json)
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) paketi

- Zaman serisi veri noktaları içeren bir JSON dosyası. Bu hızlı başlatma için örnek veriler [GitHub'da](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json)bulunabilir.

### <a name="create-an-anomaly-detector-resource"></a>Anomali Dedektörü kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları aktarın.
    
    [!code-java[Import statements](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=imports)]

2. Abonelik anahtarınız ve bitiş noktanız için değişkenler oluşturun. Aşağıda anomali tespiti için kullanabileceğiniz ÜRB'ler yer almaktadır. Bunlar, API istek URL'lerini oluşturmak için daha sonra hizmet bitiş noktanıza eklenir.

    |Algılama yöntemi  |URI  |
    |---------|---------|
    |Toplu işlem algılama    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |En son veri noktasında algılama     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    [!code-java[Initial key and endpoint variables](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=vars)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan yeni `sendRequest()` bir işlev oluşturun. Ardından aşağıdaki adımları gerçekleştirin.

2. API'ye istek gönderebilecek bir `CloseableHttpClient` nesne oluşturun. Son noktanızı `HttpPost` ve Anomali Dedektörü URL'nizi birleştirerek isteği bir istek nesnesine gönderin.

3. Üstbilgiyi `application/json`ayarlamak `setHeader()` ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye eklemek için isteğin işlevini kullanın. `Content-Type`

4. Gönderilecek veriler için `setEntity()` isteğin işlevini kullanın.

5. İstem göndermek `execute()` ve bir `CloseableHttpResponse` nesneye kaydetmek için istemcinin işlevini kullanın.

6. Yanıt `HttpEntity` içeriğini depolamak için bir nesne oluşturun. İçeriği `getEntity()`' ile alın. Yanıt boş değilse, iade edin.

    [!code-java[API request method](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=request)]

## <a name="detect-anomalies-as-a-batch"></a>Anomalileri toplu olarak algılama

1. Veriler deki `detectAnomaliesBatch()` anormallikleri toplu olarak algılamak için çağrılan bir yöntem oluşturun. Bitiş `sendRequest()` noktanız, url'niz, abonelik anahtarınız ve json verilerinizle yukarıda oluşturulan yöntemi arayın. Sonucu alın ve konsola yazdırın.

2. Yanıt alanı `code` içeriyorsa, hata kodunu ve hata iletisini yazdırın.

3. Aksi takdirde, veri kümesindeki anomalilerin konumlarını bulun. Yanıtın `isAnomaly` alanı, belirli bir veri noktasının bir anormallik olup olmadığına ilişkin bir boolean değeri içerir. JSON dizisini alın ve herhangi bir `true` değerin dizinini yazdırarak içinden geçirin. Varsa, bu değerler anormal veri noktalarıdizine karşılık gelir.

    [!code-java[Method for batch detection](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=detectBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu algılama

Veri kümesindeki `detectAnomaliesLatest()` son veri noktasının anomali durumunu algılamak için çağrılan bir yöntem oluşturun. Bitiş `sendRequest()` noktanız, url'niz, abonelik anahtarınız ve json verilerinizle yukarıda oluşturulan yöntemi arayın. Sonucu alın ve konsola yazdırın.

[!code-java[Latest point detection method](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=detectLatest)]

## <a name="load-your-time-series-data-and-send-the-request"></a>Zaman serisi verilerinizi yükleyin ve isteği gönderin

1. Uygulamanızın ana yönteminde, isteklere eklenecek verileri içeren JSON dosyasında okuyun.

2. Yukarıda oluşturulan iki anomali algılama fonksiyonlarını arayın.

    [!code-java[Main method](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=main)]

### <a name="example-response"></a>Örnek yanıt

Başarılı bir yanıt JSON biçiminde döndürülür. GitHub'da JSON yanıtını görüntülemek için aşağıdaki bağlantıları tıklayın:
* [Örnek toplu iş algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Örnek son nokta algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
