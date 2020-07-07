---
title: 'Hızlı başlangıç: anomali algılayıcısı REST API ve Python kullanarak bir Batch olarak anomali algılama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta bir toplu iş veya akış verileri olarak veri serinizdeki normalleştirimler algılamak için anomali algılayıcı API 'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 06/30/2020
ms.author: aahi
ms.custom: tracking-python
ms.openlocfilehash: 81145dd6409bf93195f6b805ed260d945e7738f2
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85982042"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-python"></a>Hızlı başlangıç: anomali algılayıcısı REST API ve Python kullanarak zaman serisi verilerinizde bozukluklar Algıla

Bu hızlı başlangıcı kullanarak, zaman serisi verilerinizde bozukluklar olup olmadığı konusunda anomali algılayıcı API 'sinin iki algılama modunu kullanmaya başlayın. Bu Python uygulaması JSON biçimli zaman serisi verilerini içeren iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıkışı                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Bir toplu iş olarak anomali algılama                        | Zaman serisi verilerinde her bir veri noktasının anomali durumunu (ve diğer verileri) ve algılanan tüm anormalilerin konumlarını içeren JSON yanıtı. |
| En son veri noktasının anomali durumunu Algıla | Zaman serisi verilerinde en son veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı.                                                                                                                                         |

 Bu uygulama Python 'da yazıldığı sırada API, çoğu programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir. Bu hızlı başlangıç için kaynak kodunu [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/python-detect-anomalies.py)'da bulabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtar ve uç noktanıza ulaşmak için Azure Portal bir anomali algılayıcı kaynağı oluşturun. Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    - Uygulamanızı anomali algılayıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
- [Python 2. x veya 3. x](https://www.python.org/downloads/)
- Python için [istekler kitaplığı](https://pypi.org/project/requests/)

- Zaman serisi veri noktalarını içeren bir JSON dosyası. Bu hızlı başlangıçta örnek veriler [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json)'da bulunabilir.

[!INCLUDE [anomaly-detector-environment-variables](../includes/environment-variables.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarmaları ekleyin.

    [!code-python[import statements](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=imports)]

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Anomali algılama için kullanabileceğiniz URI 'Ler aşağıda verilmiştir. Bunlar, daha sonra API isteği URL 'Leri oluşturmak için hizmet uç noktanıza eklenecektir.

    |Algılama yöntemi  |URI  |
    |---------|---------|
    |Toplu iş algılama    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |En son veri noktasında algılama     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    [!code-python[initial endpoint and key variables](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=vars)]

3. JSON veri dosyasında dosyayı açıp kullanarak okuyun `json.load()` .

    [!code-python[Open JSON file and read in the data](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=fileLoad)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan adlı yeni bir işlev oluşturun `send_request()` . Ardından aşağıdaki adımları gerçekleştirin.

2. İstek üstbilgileri için bir sözlük oluşturun. `Content-Type` `application/json` ' I ' olarak ayarlayın ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin.

3. Kullanarak isteği gönderin `requests.post()` . Tam istek URL 'si için uç nokta ve anomali algılama URL 'nizi birleştirip üst bilgilerinizi ve json istek verilerini ekleyin. Ardından yanıtı geri döndürün.

    [!code-python[request method](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=request)]

## <a name="detect-anomalies-as-a-batch"></a>Bir toplu iş olarak anomali algılama

1. `detect_batch()`Veri genelinde bir toplu iş olarak bozukluklar tespit etmek için adlı bir yöntem oluşturun. `send_request()`Uç nokta, URL, abonelik anahtarı ve JSON verileriniz ile yukarıda oluşturulan yöntemi çağırın.

2. `json.dumps()`Bunu biçimlendirmek için sonucu çağırın ve konsola yazdırın.

3. Yanıt `code` alanı içeriyorsa, hata kodunu ve hata iletisini yazdırın.

4. Aksi takdirde, veri kümesindeki anormalilerin konumlarını bulabilirsiniz. Yanıtın alanı, `isAnomaly` belirli bir veri noktasının bir anomali olup olmadığı ile ilgili bir Boole değeri içerir. Listede yineleme yapın ve herhangi bir değerin dizinini yazdırın `True` . Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

    [!code-python[detection as a batch](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=detectBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

1. `detect_latest()`Zaman serinizdeki en son veri noktasının bir anomali olup olmadığını anlamak için adlı bir yöntem oluşturun. `send_request()`Uç nokta, URL, abonelik anahtarı ve JSON verileri ile yukarıdaki yöntemi çağırın. 

2. `json.dumps()`Bunu biçimlendirmek için sonucu çağırın ve konsola yazdırın.

    [!code-python[Latest point detection](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=detectLatest)]

## <a name="send-the-request"></a>İsteği gönder

Yukarıda oluşturulan anomali algılama yöntemlerini çağırın.

[!code-python[Method calls](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=methodCalls)]

### <a name="example-response"></a>Örnek yanıt

JSON biçiminde başarılı bir yanıt döndürülür. GitHub 'da JSON yanıtını görüntülemek için aşağıdaki bağlantılara tıklayın:
* [Örnek toplu iş algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Örnek en son nokta algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
