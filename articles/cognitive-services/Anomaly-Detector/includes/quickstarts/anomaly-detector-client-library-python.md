---
title: Anomali Dedektörü Python istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2020
ms.author: aahi
ms.openlocfilehash: a51025245e5b02b89126afd886bc89fb1cefdef4
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759996"
---
Python için Anomaly Detector istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. Anomaly Detector hizmeti, endüstri, senaryo veya veri hacmine bakılmaksızın, üzerinde otomatik olarak en uygun modelleri kullanarak zaman serisi verilerinizdeki anormallikleri bulmanızı sağlar.

Python için Anomaly Detector istemci kitaplığını kullanın:

* Toplu iş isteği olarak, zaman serisi veri setiniz boyunca anormallikleri algılama
* Zaman serinizdeki en son veri noktasının anomali durumunu algılama

[Kütüphane referans belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-anomalydetector) | [Paketi (PyPi)](https://pypi.org/project/azure-cognitiveservices-anomalydetector/) | [GitHub'daki örnek kodu bulun](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/python-sdk-sample.py)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* Anomali dedektörü anahtarı ve bitiş noktası
* [Python 3.x](https://www.python.org/)
* [Pandalar veri analizi kütüphanesi](https://pandas.pydata.org/)
 
## <a name="setting-up"></a>Ayarlama

### <a name="create-an-anomaly-detector-resource"></a>Anomali Dedektörü kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="create-a-new-python-application"></a>Yeni bir python uygulaması oluşturma

 Yeni bir Python dosyası oluşturun ve aşağıdaki kitaplıkları aktarın.

[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

Bir ortam değişkeni olarak anahtarınız için değişkenler, bir zaman serisi veri dosyasına giden yol ve aboneliğinizin azure konumu oluşturun. Örneğin, `westus2`. 

[!code-python[Vars for the key, path location and data path](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=initVars)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Python'u yükledikten sonra istemci kitaplığını aşağıdakilerle yükleyebilirsiniz:

```console
pip install --upgrade azure-cognitiveservices-anomalydetector
```

## <a name="object-model"></a>Nesne modeli

Anomaly Detector istemcisi, anahtarınızı kullanarak Azure'a doğrulayan bir [AnomalyDetectorClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python) nesnesidir. İstemci iki anomali algılama yöntemi sağlar: [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-)kullanarak tüm veri setinde ve [Last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-)kullanarak en son veri noktasında. 

Zaman serisi verileri, [İstek](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request(class)?view=azure-python) nesnesinde [bir puan](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point(class)?view=azure-python) dizisi olarak gönderilir. Nesne, `Request` verileri (örneğin[tanecikli)](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) açıklayan özellikler ve anomali algılama parametreleri içerir. 

Anomali Dedektörü yanıtı, kullanılan yönteme bağlı olarak [lastdetectresponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) veya [EntireDetectResponse nesnesidir.](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) 

## <a name="code-examples"></a>Kod örnekleri 

Bu kod parçacıkları Python için Anomaly Detector istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesini yükleme](#load-time-series-data-from-a-file)
* [Tüm veri kümesindeki anormallikleri algılama](#detect-anomalies-in-the-entire-data-set) 
* [En son veri noktasının anomali durumunu algılama](#detect-the-anomaly-status-of-the-latest-data-point)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Azure konum değişkeninizi bitiş noktasına ekleyin ve anahtarınızla istemcinin kimliğini doğrulayın.

[!code-python[Client authentication](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=client)]

## <a name="load-time-series-data-from-a-file"></a>Dosyadan zaman serisi verileri yükleme

[GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv)bu hızlı başlangıç için örnek verileri indirin:
1. Tarayıcınızda **Raw'a**sağ tıklayın.
2. **Olarak Kaydet bağlantısını**tıklatın.
3. .csv dosyası olarak dosyayı uygulama dizininize kaydedin.

Bu zaman serisi verileri .csv dosyası olarak biçimlendirilir ve Anomaly Detector API'sine gönderilir.

Veri dosyanızı Pandas kitaplığı `read_csv()` yöntemiyle yükleyin ve veri serilerinizi depolamak için boş bir liste değişkeni yapın. Dosya yı titretin ve verileri [Nokta](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point%28class%29?view=azure-python) nesnesi olarak ekleyin. Bu nesne ,csv veri dosyanızın satırlarındaki zaman damgası ve sayısal değeri içerir. 

[!code-python[Load the data file](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=loadDataFile)]

Zaman seriniz ve veri noktalarının [parçalılığı](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) (veya periyodikliği) ile bir [İstek](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request%28class%29?view=azure-python) nesnesi oluşturun. Örneğin, `Granularity.daily`.

[!code-python[Create the request object](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=request)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesindeki anormallikleri algılama 

İstemcinin [entire_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-) yöntemini kullanarak tüm zaman serisi verileri boyunca anormallikleri algılamak için API'yi arayın. Döndürülen [EntireDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) nesnesini saklayın. Yanıt `is_anomaly` ın listesini yineleyin ve tüm `true` değerlerin dizinini yazdırın. Varsa, bu değerler anormal veri noktalarıdizine karşılık gelir.

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=detectAnomaliesBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu algılama

En son veri noktanızın istemcinin [last_detect()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-) yöntemini kullanarak bir anormallik olup olmadığını belirlemek için Anomaly Detector API'yi arayın ve döndürülen [LastDetectResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) nesnesini saklayın. Yanıtın `is_anomaly` değeri, o noktanın anomali durumunu belirten bir boolean'dır.  

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=latestPointDetection)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı komut ve `python` dosya adınız ile çalıştırın.
 
[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
