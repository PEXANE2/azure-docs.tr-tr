---
title: Anomali algılayıcı Python istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/30/2020
ms.author: aahi
ms.openlocfilehash: 8d66b653f78de5b2dee1a42227fe64152ccc6fe9
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89464221"
---
Python için anomali algılayıcı istemci kitaplığını kullanmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Anomali algılayıcı hizmeti, sektör, senaryo veya veri hacminin ne olursa olsun, üzerinde en iyi şekilde sığdırma modellerini kullanarak zaman serisi verilerinizde yer alan anormallikleri bulmanıza olanak sağlar.

Python için anomali algılayıcı istemci kitaplığını kullanarak şunları yapın:

* Toplu istek olarak zaman serisi veri kümeniz genelinde anomali algılama
* Zaman serinizdeki en son veri noktasının anomali durumunu Algıla

[Kitaplık başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-anomalydetector)  |  [Paket (Pypı)](https://pypi.org/project/azure-cognitiveservices-anomalydetector/)  |  [GitHub 'da örnek kodu bulma](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/python-sdk-sample.py)

## <a name="prerequisites"></a>Ön koşullar

* [Python 3.x](https://www.python.org/)
* [Pandas veri analizi kitaplığı](https://pandas.pydata.org/)
* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtar ve uç noktanıza ulaşmak için Azure Portal bir anomali algılayıcı kaynağı oluşturun. Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    * Uygulamanızı anomali algılayıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.


## <a name="setting-up"></a>Ayarlanıyor

[!INCLUDE [anomaly-detector-environment-variables](../environment-variables.md)]

### <a name="create-a-new-python-application"></a>Yeni bir Python uygulaması oluşturma

 Yeni bir Python dosyası oluşturun ve aşağıdaki kitaplıkları içeri aktarın.

[!code-python[import declarations](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=imports)]

Anahtarınız için bir ortam değişkeni, zaman serisi veri dosyası yolu ve aboneliğinizin Azure konumu olarak değişkenler oluşturun. Örneğin, `westus2`.

[!code-python[Vars for the key, path location and data path](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=initVars)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Python yükledikten sonra, ile istemci kitaplığını yükleyebilirsiniz:

```console
pip install --upgrade azure-cognitiveservices-anomalydetector
```

## <a name="object-model"></a>Nesne modeli

Anomali algılayıcı istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulaması yapan bir [Anokidetectorclient](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python) nesnesidir. İstemci iki anomali algılama yöntemi sağlar: [entire_detect ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-)kullanarak bir veri kümesinin tamamında ve [Last_detect ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-)kullanarak en son veri noktasında.

Zaman serisi verileri, [istek](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request?view=azure-python) nesnesinde bir dizi [işaret](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point?view=azure-python) olarak gönderilir. `Request`Nesnesi, verileri (örneğin,[ayrıntı düzeyi](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) ) ve anomali algılama parametrelerini tanımlayacak özellikler içerir.

Anomali algılayıcısı yanıtı, kullanılan yönteme bağlı olarak bir [Lastdetectresponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) veya [entiredetectresponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) nesnesidir.

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için anomali algılayıcı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesi yükleme](#load-time-series-data-from-a-file)
* [Tüm veri kümesindeki anormallikleri Algıla](#detect-anomalies-in-the-entire-data-set)
* [En son veri noktasının anomali durumunu Algıla](#detect-the-anomaly-status-of-the-latest-data-point)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Azure location değişkeninizi uç noktaya ekleyin ve anahtarınızla istemcinin kimliğini doğrulayın.

[!code-python[Client authentication](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=client)]

## <a name="load-time-series-data-from-a-file"></a>Bir dosyadan zaman serisi verilerini yükle

Bu hızlı başlangıçta [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv)'dan örnek verileri indirin:
1. Tarayıcınızda, **RAW**' a sağ tıklayın.
2. **Bağlantıyı farklı kaydet**' e tıklayın.
3. Dosyayı uygulama dizininize bir. csv dosyası olarak kaydedin.

Bu zaman serisi verileri. csv dosyası olarak biçimlendirilir ve anomali algılayıcısı API 'sine gönderilir.

Veri dosyanızı Pandas kitaplığının `read_csv()` yöntemiyle yükleyin ve veri serinizi depolamak için boş bir liste değişkeni oluşturun. Dosya üzerinde yineleme yapın ve verileri bir [nokta](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.point?view=azure-python) nesnesi olarak ekleyin. Bu nesne,. csv veri dosyanızdaki satırlardan zaman damgası ve sayısal değer içerir.

[!code-python[Load the data file](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=loadDataFile)]

Zaman seriniz ve veri noktalarının [ayrıntı düzeyi](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.granularity?view=azure-python) (veya dönemsellik) Ile bir [istek](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.request?view=azure-python) nesnesi oluşturun. Örneğin, `Granularity.daily`.

[!code-python[Create the request object](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=request)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesindeki anormallikleri Algıla

İstemcinin [entire_detect ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#entire-detect-body--custom-headers-none--raw-false----operation-config-) yöntemini kullanarak tüm zaman serisi verilerinin bozuklukilerini algılamak için API 'yi çağırın. Döndürülen [Entiredetectresponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-python) nesnesini depolayın. Yanıtın `is_anomaly` listesini yineleyin ve herhangi bir değerin dizinini yazdırın `true` . Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=detectAnomaliesBatch)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

En son veri noktanağınızın, istemcinin [last_detect ()](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-python#last-detect-body--custom-headers-none--raw-false----operation-config-) yöntemini kullanarak bir anomali olup olmadığını ve döndürülen [lastdetectresponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-anomalydetector/azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-python) nesnesini depolayıp Depomadığını öğrenmek IÇIN anomali algılayıcısı API 'sini çağırın. Yanıtın değeri, `is_anomaly` Bu noktanın anomali durumunu belirten bir Boole değeridir.  

[!code-python[Batch anomaly detection sample](~/samples-anomaly-detector/quickstarts/sdk/python-sdk-sample.py?name=latestPointDetection)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `python` komutuyla ve dosya adınızla çalıştırın.

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
