---
title: 'Hızlı başlangıç: anomali algılayıcısının REST API ve Java kullanarak zaman serisi verilerinizde bozukluklar algılama'
titleSuffix: Azure Cognitive Services
description: Bir toplu iş ya da akış verileri olarak veri serinizdeki anormal verileri algılamak için anomali algılayıcı API 'sini nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 06/30/2020
ms.author: aahi
ms.openlocfilehash: 0021548779409272dee40021dc5f56eb76aa7d96
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85985593"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-java"></a>Hızlı başlangıç: anomali algılayıcısının REST API ve Java kullanarak zaman serisi verilerinizde bozukluklar algılama

Bu hızlı başlangıcı kullanarak, zaman serisi verilerinizde bozukluklar olup olmadığı konusunda anomali algılayıcı API 'sinin iki algılama modunu kullanmaya başlayın. Bu Java uygulaması, JSON biçimli zaman serisi verilerini içeren iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıkışı                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Bir toplu iş olarak anomali algılama                        | Zaman serisi verilerinde her bir veri noktasının anomali durumunu (ve diğer verileri) ve algılanan tüm anormalilerin konumlarını içeren JSON yanıtı. |
| En son veri noktasının anomali durumunu Algıla | Zaman serisi verilerinde en son veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı.                                                                                                                                         |

 Bu uygulama Java 'da yazıldığı sırada, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir. Bu hızlı başlangıç için kaynak kodunu [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/java-detect-anomalies.java)'da bulabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtar ve uç noktanıza ulaşmak için Azure Portal bir anomali algılayıcı kaynağı oluşturun. Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    - Uygulamanızı anomali algılayıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
- [Java &trade; Development kıt (JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya üzeri.
- Bu kitaplıkları Maven deposundan içeri aktar
    - [Java paketindeki JSON](https://mvnrepository.com/artifact/org.json/json)
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) paketi

- Zaman serisi veri noktalarını içeren bir JSON dosyası. Bu hızlı başlangıçta örnek veriler [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json)'da bulunabilir.

[!INCLUDE [anomaly-detector-environment-variables](../includes/environment-variables.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.
    
    [!code-java[Import statements](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=imports)]

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Anomali algılama için kullanabileceğiniz URI 'Ler aşağıda verilmiştir. Bunlar, daha sonra API isteği URL 'Leri oluşturmak için hizmet uç noktanıza eklenecektir.

    |Algılama yöntemi  |URI  |
    |---------|---------|
    |Toplu iş algılama    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |En son veri noktasında algılama     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    [!code-java[Initial key and endpoint variables](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=vars)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan adlı yeni bir işlev oluşturun `sendRequest()` . Ardından aşağıdaki adımları gerçekleştirin.

2. `CloseableHttpClient`API 'ye istek gönderebilen bir nesne oluşturun. `HttpPost`Uç noktanızı ve anomali algılayıcı URL 'sini birleştirerek isteği bir istek nesnesine gönderin.

3. `setHeader()`Üstbilgiyi ayarlamak için isteğin işlevini kullanın `Content-Type` `application/json` ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin.

4. `setEntity()`Gönderilecek verilere isteğin işlevini kullanın.

5. `execute()`İsteği göndermek ve bir nesnesine kaydetmek için istemcinin işlevini kullanın `CloseableHttpResponse` .

6. `HttpEntity`Yanıt içeriğini depolamak için bir nesne oluşturun. İçeriğini ile alın `getEntity()` . Yanıt boş değilse, döndürün.

    [!code-java[API request method](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=request)]

## <a name="detect-anomalies-as-a-batch"></a>Bir toplu iş olarak anomali algılama

1. `detectAnomaliesBatch()`Veri genelinde bir toplu iş olarak bozukluklar tespit etmek için adlı bir yöntem oluşturun. `sendRequest()`Uç nokta, URL, abonelik anahtarı ve JSON verileriniz ile yukarıda oluşturulan yöntemi çağırın. Sonucu alın ve konsola yazdırın.

2. Yanıt `code` alanı içeriyorsa, hata kodunu ve hata iletisini yazdırın.

3. Aksi takdirde, veri kümesindeki anormalilerin konumlarını bulabilirsiniz. Yanıtın alanı, `isAnomaly` belirli bir veri noktasının bir anomali olup olmadığı ile ilgili bir Boole değeri içerir. JSON dizisini alın ve herhangi bir değerin dizinini yazdırarak bu diziyi yineleyin `true` . Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

    [!code-java[Method for batch detection](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=detectBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

`detectAnomaliesLatest()`Veri kümesindeki son veri noktasının anomali durumunu algılamak için adlı bir yöntem oluşturun. `sendRequest()`Uç nokta, URL, abonelik anahtarı ve JSON verileriniz ile yukarıda oluşturulan yöntemi çağırın. Sonucu alın ve konsola yazdırın.

[!code-java[Latest point detection method](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=detectLatest)]

## <a name="load-your-time-series-data-and-send-the-request"></a>Zaman serisi verilerinizi yükleyin ve isteği gönderin

1. Uygulamanızın ana yönteminde, isteklere eklenecek verileri içeren JSON dosyasını okuyun.

2. Yukarıda oluşturulan iki anomali algılama işlevini çağırın.

    [!code-java[Main method](~/samples-anomaly-detector/quickstarts/java-detect-anomalies.java?name=main)]

### <a name="example-response"></a>Örnek yanıt

JSON biçiminde başarılı bir yanıt döndürülür. GitHub 'da JSON yanıtını görüntülemek için aşağıdaki bağlantılara tıklayın:
* [Örnek toplu iş algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Örnek en son nokta algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
