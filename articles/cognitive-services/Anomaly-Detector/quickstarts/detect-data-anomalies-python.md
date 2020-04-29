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
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 684aba561dc50b64dd7cc564cff8e55229ce1429
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80239039"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-python"></a>Hızlı başlangıç: anomali algılayıcısı REST API ve Python kullanarak zaman serisi verilerinizde bozukluklar Algıla

Bu hızlı başlangıcı kullanarak, zaman serisi verilerinizde bozukluklar olup olmadığı konusunda anomali algılayıcı API 'sinin iki algılama modunu kullanmaya başlayın. Bu Python uygulaması JSON biçimli zaman serisi verilerini içeren iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıkışı                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Bir toplu iş olarak anomali algılama                        | Zaman serisi verilerinde her bir veri noktasının anomali durumunu (ve diğer verileri) ve algılanan tüm anormalilerin konumlarını içeren JSON yanıtı. |
| En son veri noktasının anomali durumunu Algıla | Zaman serisi verilerinde en son veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı.                                                                                                                                         |

 Bu uygulama Python 'da yazıldığı sırada API, çoğu programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir. Bu hızlı başlangıç için kaynak kodunu [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/python-detect-anomalies.py)'da bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- [Python 2. x veya 3. x](https://www.python.org/downloads/)
- Anomali algılayıcı anahtarı ve uç noktası
- Python için [istekler kitaplığı](https://pypi.org/project/requests/)

- Zaman serisi veri noktalarını içeren bir JSON dosyası. Bu hızlı başlangıçta örnek veriler [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json)'da bulunabilir.

### <a name="create-an-anomaly-detector-resource"></a>Anomali algılayıcısı kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]


## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarmaları ekleyin.

    [!code-python[import statements](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=imports)]

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Anomali algılama için kullanabileceğiniz URI 'Ler aşağıda verilmiştir. Bunlar, daha sonra API isteği URL 'Leri oluşturmak için hizmet uç noktanıza eklenecektir.

    |Algılama yöntemi  |URI  |
    |---------|---------|
    |Toplu iş algılama    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |En son veri noktasında algılama     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    [!code-python[initial endpoint and key variables](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=vars)]

3. JSON veri dosyasında dosyayı açıp kullanarak `json.load()`okuyun.

    [!code-python[Open JSON file and read in the data](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=fileLoad)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan adlı `send_request()` yeni bir işlev oluşturun. Ardından aşağıdaki adımları gerçekleştirin.

2. İstek üstbilgileri için bir sözlük oluşturun. ' I ' olarak ayarlayın ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin. `Content-Type` `application/json`

3. Kullanarak `requests.post()`isteği gönderin. Tam istek URL 'si için uç nokta ve anomali algılama URL 'nizi birleştirip üst bilgilerinizi ve json istek verilerini ekleyin. Ardından yanıtı geri döndürün.

    [!code-python[request method](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=request)]

## <a name="detect-anomalies-as-a-batch"></a>Bir toplu iş olarak anomali algılama

1. Veri genelinde bir toplu `detect_batch()` iş olarak bozukluklar tespit etmek için adlı bir yöntem oluşturun. Uç nokta `send_request()` , URL, abonelik anahtarı ve JSON verileriniz ile yukarıda oluşturulan yöntemi çağırın.

2. Bunu `json.dumps()` biçimlendirmek için sonucu çağırın ve konsola yazdırın.

3. Yanıt alanı içeriyorsa `code` , hata kodunu ve hata iletisini yazdırın.

4. Aksi takdirde, veri kümesindeki anormalilerin konumlarını bulabilirsiniz. Yanıtın `isAnomaly` alanı, belirli bir veri noktasının bir anomali olup olmadığı ile ilgili bir Boole değeri içerir. Listede yineleme yapın ve herhangi bir `True` değerin dizinini yazdırın. Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

    [!code-python[detection as a batch](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=detectBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

1. Zaman serinizdeki en `detect_latest()` son veri noktasının bir anomali olup olmadığını anlamak için adlı bir yöntem oluşturun. Uç nokta `send_request()` , URL, abonelik anahtarı ve JSON verileri ile yukarıdaki yöntemi çağırın. 

2. Bunu `json.dumps()` biçimlendirmek için sonucu çağırın ve konsola yazdırın.

    [!code-python[Latest point detection](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=detectLatest)]

## <a name="send-the-request"></a>İsteği gönder

Yukarıda oluşturulan anomali algılama yöntemlerini çağırın.

[!code-python[Method calls](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=methodCalls)]

### <a name="example-response"></a>Örnek yanıt

JSON biçiminde başarılı bir yanıt döndürülür. GitHub 'da JSON yanıtını görüntülemek için aşağıdaki bağlantılara tıklayın:
* [Örnek toplu iş algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Örnek en son nokta algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
