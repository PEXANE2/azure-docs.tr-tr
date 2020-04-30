---
title: Anomali algılayıcısı JavaScript istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2020
ms.author: aahi
ms.openlocfilehash: dca07f37377880008160fa3521d66ed4f6afc084
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81759989"
---
JavaScript için anomali algılayıcı istemci kitaplığını kullanmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Anomali algılayıcı hizmeti, sektör, senaryo veya veri hacminin ne olursa olsun, üzerinde en iyi şekilde sığdırma modellerini kullanarak zaman serisi verilerinizde yer alan anormallikleri bulmanıza olanak sağlar.

JavaScript için anomali algılayıcı istemci kitaplığını kullanarak şunları yapın:

* Toplu istek olarak zaman serisi veri kümeniz genelinde anomali algılama
* Zaman serinizdeki en son veri noktasının anomali durumunu Algıla

[Başvuru belge](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/?view=azure-node-latest) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector) | [paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-anomalydetector) | [GitHub 'da kodu bulma](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/javascript/AnomalyDetector)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* [Node. js](https://nodejs.org/) ' nin geçerli sürümü
* Anomali algılayıcı anahtarı ve uç noktası

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-an-anomaly-detector-azure-resource"></a>Anomali algılayıcı Azure kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

Bir `package.json` dosya `npm init` ile bir düğüm uygulaması oluşturmak için komutunu çalıştırın. 

```console
npm init
```

Adlı `index.js` bir dosya oluşturun ve aşağıdaki kitaplıkları içeri aktarın:

[!code-javascript[Import statements](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=imports)]

Kaynağınızın Azure uç noktası ve anahtarı için değişken oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir. Daha sonraki bir adımda indirileceği örnek veri dosyası için başka bir değişken ve veri noktaları için boş bir liste oluşturun. Sonra anahtarı içeren `ApiKeyCredentials` bir nesne oluşturun.

[!code-javascript[Initial endpoint and key variables](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=vars)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

`ms-rest-azure` Ve `azure-cognitiveservices-anomalydetector` NPM paketlerini yükler. Bu hızlı başlangıçta CSV ayrıştırma kitaplığı da kullanılır:

```console
npm install  @azure/cognitiveservices-anomalydetector @azure/ms-rest-js csv-parse
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.

## <a name="object-model"></a>Nesne modeli

Anomali algılayıcı istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulaması yapan bir [Anokidetectorclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest) nesnesidir. İstemci,, [Entiredetect ()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest#entiredetect-request--servicecallback-entiredetectresponse--)kullanan bir veri kümesinin tamamında ve [lastdetect ()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest#lastdetect-request--msrest-requestoptionsbase-)kullanan en son veri noktasında iki anomali algılama yöntemi sağlar. 

Zaman serisi verileri bir [istek](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/request?view=azure-node-latest) nesnesinde bir dizi [işaret](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/point?view=azure-node-latest) olarak gönderilir. `Request` Nesnesi, verileri (örneğin,[ayrıntı düzeyi](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/request?view=azure-node-latest#granularity) ) ve anomali algılama parametrelerini tanımlayacak özellikler içerir. 

Anomali algılayıcısı yanıtı, kullanılan yönteme bağlı olarak bir [Lastdetectresponse](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/lastdetectresponse?view=azure-node-latest) veya [entiredetectresponse](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/entiredetectresponse?view=azure-node-latest) nesnesidir. 

## <a name="code-examples"></a>Kod örnekleri 

Bu kod parçacıkları, Node. js için anomali algılayıcı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesi yükleme](#load-time-series-data-from-a-file)
* [Tüm veri kümesindeki anormallikleri Algıla](#detect-anomalies-in-the-entire-data-set) 
* [En son veri noktasının anomali durumunu Algıla](#detect-the-anomaly-status-of-the-latest-data-point)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Uç noktanız ve kimlik bilgilerinizle bir [Anorivtorclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest) nesnesi örneği oluşturun.

[!code-javascript[Authentication](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=authentication)]

## <a name="load-time-series-data-from-a-file"></a>Bir dosyadan zaman serisi verilerini yükle

Bu hızlı başlangıçta [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/AnomalyDetector/request-data.csv)'dan örnek verileri indirin:
1. Tarayıcınızda, **RAW**' a sağ tıklayın.
2. **Bağlantıyı farklı kaydet**' e tıklayın.
3. Dosyayı uygulama dizininize bir. csv dosyası olarak kaydedin.

Bu zaman serisi verileri. csv dosyası olarak biçimlendirilir ve anomali algılayıcısı API 'sine gönderilir.

CSV ayrıştırma kitaplığının `readFileSync()` yöntemiyle veri dosyanızı okuyun ve dosyasını ile `parse()`ayrıştırın. Her satır için zaman damgasını içeren bir [nokta](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/point?view=azure-node-latest) nesnesi ve sayısal değeri gönderin.

[!code-javascript[Read the data file](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=readFile)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesindeki anormallikleri Algıla 

İstemcinin [Entiredetect ()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest#entiredetect-request--msrest-requestoptionsbase-) yöntemiyle bir toplu iş olarak tüm zaman serilerinin bozuklukilerini algılamak için API 'yi çağırın. Döndürülen [Entiredetectresponse](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/entiredetectresponse?view=azure-node-latest) nesnesini depolayın. Yanıtın `isAnomaly` listesini yineleyin ve herhangi bir `true` değerin dizinini yazdırın. Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

[!code-javascript[Batch detection function](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=batchCall)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

En son veri noktanağınızın, istemcinin [Lastdetect ()](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient?view=azure-node-latest#lastdetect-request--msrest-requestoptionsbase-) yöntemini kullanarak bir anomali olup olmadığını belirlemek ve döndürülen [lastdetectresponse](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-anomalydetector/lastdetectresponse?view=azure-node-latest) nesnesini depolamak IÇIN anomali algılayıcısı API 'sini çağırın. Yanıtın `isAnomaly` değeri, bu noktanın anomali durumunu belirten bir Boole değeridir.  

[!code-javascript[Last point detection function](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=lastDetection)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı hızlı başlangıç dosyanızdaki `node` komutla çalıştırın.

```console
node index.js
```

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
