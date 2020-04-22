---
title: Anomali Dedektörü JavaScript istemci kitaplığı quickstart
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2020
ms.author: aahi
ms.openlocfilehash: dca07f37377880008160fa3521d66ed4f6afc084
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759989"
---
JavaScript için Anomaly Detector istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. Anomaly Detector hizmeti, endüstri, senaryo veya veri hacmine bakılmaksızın, üzerinde otomatik olarak en uygun modelleri kullanarak zaman serisi verilerinizdeki anormallikleri bulmanızı sağlar.

JavaScript için Anomaly Detector istemci kitaplığını kullanın:

* Toplu iş isteği olarak, zaman serisi veri setiniz boyunca anormallikleri algılama
* Zaman serinizdeki en son veri noktasının anomali durumunu algılama

[Referans belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/?view=azure-node-latest) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector) | [Paketi (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-anomalydetector) | [GitHub üzerinde kodu bulun](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/javascript/AnomalyDetector)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [Node.js'nin](https://nodejs.org/) geçerli sürümü
* Anomali dedektörü anahtarı ve bitiş noktası

## <a name="setting-up"></a>Ayarlama

### <a name="create-an-anomaly-detector-azure-resource"></a>Bir Anomali Dedektörü Azure kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu pencereye gidin. 

```console
mkdir myapp && cd myapp
```

Bir `npm init` dosya ile bir düğüm uygulaması oluşturmak için komutu çalıştırın. `package.json` 

```console
npm init
```

Adlı `index.js` bir dosya oluşturun ve aşağıdaki kitaplıkları aktarın:

[!code-javascript[Import statements](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=imports)]

Kaynağınızın Azure bitiş noktası ve anahtarı değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir. Daha sonraki bir adımda indireceğiniz örnek veri dosyası için başka bir değişken ve veri noktaları için boş bir liste oluşturun. Ardından anahtarı `ApiKeyCredentials` içerecek bir nesne oluşturun.

[!code-javascript[Initial endpoint and key variables](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=vars)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Ve `azure-cognitiveservices-anomalydetector` `ms-rest-azure` NPM paketlerini yükleyin. CSV-ayrıştıran kitaplık da bu quickstart kullanılır:

```console
npm install  @azure/cognitiveservices-anomalydetector @azure/ms-rest-js csv-parse
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.

## <a name="object-model"></a>Nesne modeli

Anomaly Detector istemcisi, anahtarınızı kullanarak Azure'a doğrulayan bir [AnomalyDetectorClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest) nesnesidir. İstemci iki anomali algılama yöntemi sağlar: [Tüm Detect()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest#entiredetect-request--servicecallback-entiredetectresponse--)kullanarak tüm veri kümesinde ve [LastDetect()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest#lastdetect-request--msrest-requestoptionsbase-)kullanarak en son veri noktasında. 

Zaman serisi verileri, [İstek](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/request?view=azure-node-latest) nesnesinde [Puan](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/point?view=azure-node-latest) lar dizisi olarak gönderilir. Nesne, `Request` verileri (örneğin[tanecikli)](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/request?view=azure-node-latest#granularity) açıklayan özellikler ve anomali algılama parametreleri içerir. 

Anomali Dedektörü yanıtı, kullanılan yönteme bağlı olarak [lastdetectresponse](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/lastdetectresponse?view=azure-node-latest) veya [EntireDetectResponse nesnesidir.](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/entiredetectresponse?view=azure-node-latest) 

## <a name="code-examples"></a>Kod örnekleri 

Bu kod parçacıkları, Node.js için Anomaly Detector istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesini yükleme](#load-time-series-data-from-a-file)
* [Tüm veri kümesindeki anormallikleri algılama](#detect-anomalies-in-the-entire-data-set) 
* [En son veri noktasının anomali durumunu algılama](#detect-the-anomaly-status-of-the-latest-data-point)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Bir [AnomalyDetectorClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest) nesnesi bitiş noktası ve kimlik bilgileri ile anında.

[!code-javascript[Authentication](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=authentication)]

## <a name="load-time-series-data-from-a-file"></a>Dosyadan zaman serisi verileri yükleme

[GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/AnomalyDetector/request-data.csv)bu hızlı başlangıç için örnek verileri indirin:
1. Tarayıcınızda **Raw'a**sağ tıklayın.
2. **Olarak Kaydet bağlantısını**tıklatın.
3. .csv dosyası olarak dosyayı uygulama dizininize kaydedin.

Bu zaman serisi verileri .csv dosyası olarak biçimlendirilir ve Anomaly Detector API'sine gönderilir.

CSV-ayrıştiran kitaplığın `readFileSync()` yöntemiyle veri dosyanızı okuyun ve dosyayı `parse()`ayrıştınız. Her satır için, zaman damgasını ve sayısal değeri içeren bir [Nokta](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/point?view=azure-node-latest) nesnesini itin.

[!code-javascript[Read the data file](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=readFile)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesindeki anormallikleri algılama 

İstemcinin [tüm Detect()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest#entiredetect-request--msrest-requestoptionsbase-) yöntemiyle tüm zaman serileri boyunca anormallikleri bir toplu iş olarak algılamak için API'yi arayın. Döndürülen [EntireDetectResponse](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/entiredetectresponse?view=azure-node-latest) nesnesini saklayın. Yanıt `isAnomaly` ın listesini yineleyin ve tüm `true` değerlerin dizinini yazdırın. Varsa, bu değerler anormal veri noktalarıdizine karşılık gelir.

[!code-javascript[Batch detection function](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=batchCall)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu algılama

En son veri noktanızın istemcinin [lastDetect()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest#lastdetect-request--msrest-requestoptionsbase-) yöntemini kullanarak bir anormallik olup olmadığını belirlemek için Anomaly Detector API'yi arayın ve döndürülen [LastDetectResponse](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/lastdetectresponse?view=azure-node-latest) nesnesini saklayın. Yanıtın `isAnomaly` değeri, o noktanın anomali durumunu belirten bir boolean'dır.  

[!code-javascript[Last point detection function](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=lastDetection)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Hızlı başlatma dosyanızdaki `node` komutla uygulamayı çalıştırın.

```console
node index.js
```

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
