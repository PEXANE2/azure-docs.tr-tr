---
title: 'Quickstart: Anomali Dedektörü REST API ve Python kullanarak bir toplu olarak anomalileri tespit'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta veri serinizdeki anormallikleri toplu olarak veya veri akışı nda algılamak için Anomali Dedektörü API'sını kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 684aba561dc50b64dd7cc564cff8e55229ce1429
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80239039"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-python"></a>Hızlı başlangıç: Anomali Dedektörü REST API ve Python'u kullanarak zaman serisi verilerinizdeki anormallikleri tespit edin

Zaman serisi verilerinizdeki anormallikleri tespit etmek için Anomali Dedektörü API'sının iki algılama modunu kullanmaya başlamak için bu hızlı başlangıcı kullanın. Bu Python uygulaması JSON biçimlendirilmiş zaman serisi verilerini içeren iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıktısı                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Anomalileri toplu olarak algılama                        | Zaman serisi verilerindeki her veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı ve algılanan anomalilerin konumları. |
| En son veri noktasının anomali durumunu algılama | Zaman serisi verilerindeki en son veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı.                                                                                                                                         |

 Bu uygulama Python dilinde yazılmış olsa da, API çoğu programlama diliyle uyumlu bir RESTful web hizmetidir. Bu hızlı başlatmanın kaynak kodunu [GitHub'da](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/python-detect-anomalies.py)bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- [Python 2.x veya 3.x](https://www.python.org/downloads/)
- Anomali dedektörü anahtarı ve bitiş noktası
- Python için [İstekler kitaplığı](https://pypi.org/project/requests/)

- Zaman serisi veri noktaları içeren bir JSON dosyası. Bu hızlı başlatma için örnek veriler [GitHub'da](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json)bulunabilir.

### <a name="create-an-anomaly-detector-resource"></a>Anomali Dedektörü kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]


## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Yeni bir python dosyası oluşturun ve aşağıdaki içeri almaları ekleyin.

    [!code-python[import statements](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=imports)]

2. Abonelik anahtarınız ve bitiş noktanız için değişkenler oluşturun. Aşağıda anomali tespiti için kullanabileceğiniz ÜRB'ler yer almaktadır. Bunlar, API istek URL'lerini oluşturmak için daha sonra hizmet bitiş noktanıza eklenir.

    |Algılama yöntemi  |URI  |
    |---------|---------|
    |Toplu işlem algılama    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |En son veri noktasında algılama     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    [!code-python[initial endpoint and key variables](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=vars)]

3. JSON veri dosyasını açarak ve `json.load()`kullanarak okuyun.

    [!code-python[Open JSON file and read in the data](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=fileLoad)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan yeni `send_request()` bir işlev oluşturun. Ardından aşağıdaki adımları gerçekleştirin.

2. İstek üstbilgisi için bir sözlük oluşturun. 'yi `Content-Type` `application/json`ayarlayın ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin.

3. İsteği kullanarak `requests.post()`gönder. Tam istek URL'si için uç nokta ve anomali algılama URL'nizi birleştirin ve üstbilgilerinizi ve json istek verilerinizi ekleyin. Ve sonra yanıtı geri ver.

    [!code-python[request method](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=request)]

## <a name="detect-anomalies-as-a-batch"></a>Anomalileri toplu olarak algılama

1. Veriler deki `detect_batch()` anormallikleri toplu olarak algılamak için çağrılan bir yöntem oluşturun. Bitiş `send_request()` noktanız, url'niz, abonelik anahtarınız ve json verilerinizle yukarıda oluşturulan yöntemi arayın.

2. Biçimiiçin sonucu çağırın `json.dumps()` ve konsola yazdırın.

3. Yanıt alanı `code` içeriyorsa, hata kodunu ve hata iletisini yazdırın.

4. Aksi takdirde, veri kümesindeki anomalilerin konumlarını bulun. Yanıtın `isAnomaly` alanı, belirli bir veri noktasının bir anormallik olup olmadığına ilişkin bir boolean değeri içerir. Liste yi yineleyin ve tüm `True` değerlerin dizinini yazdırın. Varsa, bu değerler anormal veri noktalarıdizine karşılık gelir.

    [!code-python[detection as a batch](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=detectBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu algılama

1. Zaman serinizdeki `detect_latest()` en son veri noktasının bir anormallik olup olmadığını belirlemek için çağrılan bir yöntem oluşturun. Bitiş `send_request()` noktanız, url'niz, abonelik anahtarınız ve json verilerinizle yukarıdaki yöntemi arayın. 

2. Biçimiiçin sonucu çağırın `json.dumps()` ve konsola yazdırın.

    [!code-python[Latest point detection](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=detectLatest)]

## <a name="send-the-request"></a>İsteği gönderme

Yukarıda oluşturulan anomali algılama yöntemlerini arayın.

[!code-python[Method calls](~/samples-anomaly-detector/quickstarts/python-detect-anomalies.py?name=methodCalls)]

### <a name="example-response"></a>Örnek yanıt

Başarılı bir yanıt JSON biçiminde döndürülür. GitHub'da JSON yanıtını görüntülemek için aşağıdaki bağlantıları tıklayın:
* [Örnek toplu iş algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Örnek son nokta algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
