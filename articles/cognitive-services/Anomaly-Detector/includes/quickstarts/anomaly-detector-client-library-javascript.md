---
title: Anomali algılayıcısı JavaScript istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/22/2020
ms.author: mbullwin
ms.custom: devx-track-js
ms.openlocfilehash: 36b8a6952a8dc0b34df7bf32a708c71547bf5b33
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98948428"
---
JavaScript için anomali algılayıcı istemci kitaplığını kullanmaya başlayın. Hizmet tarafından sunulan algoritmaları kullanarak paketi başlatmak için bu adımları izleyin. Anomali algılayıcı hizmeti, sektör, senaryo veya veri hacminin ne olursa olsun, üzerinde en iyi şekilde sığdırma modellerini kullanarak zaman serisi verilerinizde yer alan anormallikleri bulmanıza olanak sağlar.

JavaScript için anomali algılayıcı istemci kitaplığını kullanarak şunları yapın:

* Bir toplu istek olarak zaman serisi veri kümesi genelinde anomali algılama
* Zaman serinizdeki en son veri noktasının anomali durumunu Algıla
* Veri kümesindeki eğilim değişiklik noktalarını tespit edin.

[Kitaplık başvuru belgeleri](https://go.microsoft.com/fwlink/?linkid=2090788)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector)  |  [Paket (NPM)](https://www.npmjs.com/package/%40azure/ai-anomaly-detector)  |  [GitHub 'da kodu bulma](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/javascript/AnomalyDetector)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* [Node.js](https://nodejs.org/) geçerli sürümü
* Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtar ve uç noktanıza ulaşmak için Azure Portal bir anomali algılayıcı kaynağı oluşturun. Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    * Uygulamanızı anomali algılayıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

[!INCLUDE [anomaly-detector-environment-variables](../environment-variables.md)]

### <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

`npm init`Bir dosya ile bir düğüm uygulaması oluşturmak için komutunu çalıştırın `package.json` . 

```console
npm init
```

Adlı bir dosya oluşturun `index.js` ve aşağıdaki kitaplıkları içeri aktarın:

[!code-javascript[Import statements](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=imports)]

Kaynağınızın Azure uç noktası ve anahtarı için değişken oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için onu çalıştıran düzenleyiciyi, IDE 'yi veya kabuğu kapatıp yeniden açmanız gerekir. Daha sonraki bir adımda indirileceği örnek veri dosyası için başka bir değişken ve veri noktaları için boş bir liste oluşturun. Sonra `ApiKeyCredentials` anahtarı içeren bir nesne oluşturun.

[!code-javascript[Initial endpoint and key variables](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=vars)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

`ms-rest-azure`Ve `azure-cognitiveservices-anomalydetector` NPM paketlerini yükler. Bu hızlı başlangıçta CSV ayrıştırma kitaplığı da kullanılır:

```console
npm install @azure/ai-anomaly-detector @azure/ms-rest-js csv-parse
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.

## <a name="object-model"></a>Nesne modeli

Anomali algılayıcı istemcisi, anahtarınızı kullanarak Azure 'da kimlik doğrulaması yapan bir [Anokidetectorclient](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient) nesnesidir. İstemci, [Entiredetect ()](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient#entiredetect-request--servicecallback-entiredetectresponse--)kullanarak bir veri kümesinin tamamında veya [lastdetect ()](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient#lastdetect-request--msrest-requestoptionsbase-)kullanarak en son veri noktasında anomali algılama yapabilir. [Changepointdetectasync](https://go.microsoft.com/fwlink/?linkid=2090788) yöntemi, bir eğilim içindeki değişiklikleri işaretleyen noktaları algılar. 

Zaman serisi verileri bir [istek](/javascript/api/@azure/cognitiveservices-anomalydetector/request) nesnesinde bir dizi [işaret](/javascript/api/@azure/cognitiveservices-anomalydetector/point) olarak gönderilir. `Request`Nesnesi, verileri (örneğin,[ayrıntı düzeyi](/javascript/api/@azure/cognitiveservices-anomalydetector/request#granularity) ) ve anomali algılama parametrelerini tanımlayacak özellikler içerir. 

Anomali algılayıcısı yanıtı, kullanılan yönteme bağlı olarak bir [Lastdetectresponse](/javascript/api/@azure/cognitiveservices-anomalydetector/lastdetectresponse), [entiredetectresponse](/javascript/api/@azure/cognitiveservices-anomalydetector/entiredetectresponse)veya [changepointdetectresponse](https://go.microsoft.com/fwlink/?linkid=2090788) nesnesidir. 

## <a name="code-examples"></a>Kod örnekleri 

Bu kod parçacıkları, Node.js için anomali algılayıcı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesi yükleme](#load-time-series-data-from-a-file)
* [Tüm veri kümesindeki anormallikleri Algıla](#detect-anomalies-in-the-entire-data-set) 
* [En son veri noktasının anomali durumunu Algıla](#detect-the-anomaly-status-of-the-latest-data-point)
* [Veri kümesindeki değişiklik noktalarını Algıla](#detect-change-points-in-the-data-set)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Uç noktanız ve kimlik bilgilerinizle bir [Anorivtorclient](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient) nesnesi örneği oluşturun.

[!code-javascript[Authentication](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=authentication)]

## <a name="load-time-series-data-from-a-file"></a>Bir dosyadan zaman serisi verilerini yükle

Bu hızlı başlangıçta [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/AnomalyDetector/request-data.csv)'dan örnek verileri indirin:
1. Tarayıcınızda, **RAW**' a sağ tıklayın.
2. **Bağlantıyı farklı kaydet**' e tıklayın.
3. Dosyayı uygulama dizininize bir. csv dosyası olarak kaydedin.

Bu zaman serisi verileri. csv dosyası olarak biçimlendirilir ve anomali algılayıcısı API 'sine gönderilir.

CSV ayrıştırma kitaplığının yöntemiyle veri dosyanızı okuyun `readFileSync()` ve dosyasını ile ayrıştırın `parse()` . Her satır için zaman damgasını içeren bir [nokta](/javascript/api/@azure/cognitiveservices-anomalydetector/point) nesnesi ve sayısal değeri gönderin.

[!code-javascript[Read the data file](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=readFile)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesindeki anormallikleri Algıla 

İstemcinin [Entiredetect ()](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient#entiredetect-request--msrest-requestoptionsbase-) yöntemiyle bir toplu iş olarak tüm zaman serilerinin bozuklukilerini algılamak için API 'yi çağırın. Döndürülen [Entiredetectresponse](/javascript/api/@azure/cognitiveservices-anomalydetector/entiredetectresponse) nesnesini depolayın. Yanıtın `isAnomaly` listesini yineleyin ve herhangi bir değerin dizinini yazdırın `true` . Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

[!code-javascript[Batch detection function](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=batchCall)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

En son veri noktanağınızın, istemcinin [Lastdetect ()](/javascript/api/@azure/cognitiveservices-anomalydetector/anomalydetectorclient#lastdetect-request--msrest-requestoptionsbase-) yöntemini kullanarak bir anomali olup olmadığını belirlemek ve döndürülen [lastdetectresponse](/javascript/api/@azure/cognitiveservices-anomalydetector/lastdetectresponse) nesnesini depolamak IÇIN anomali algılayıcısı API 'sini çağırın. Yanıtın değeri, `isAnomaly` Bu noktanın anomali durumunu belirten bir Boole değeridir.  

[!code-javascript[Last point detection function](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=lastDetection)]

## <a name="detect-change-points-in-the-data-set"></a>Veri kümesindeki değişiklik noktalarını Algıla

İstemci [Detectchangepoint ()](https://go.microsoft.com/fwlink/?linkid=2090788) yöntemiyle zaman serisinde değişiklik noktalarını algılamak için API 'yi çağırın. Döndürülen [Changepointdetectresponse](https://go.microsoft.com/fwlink/?linkid=2090788) nesnesini depolayın. Yanıtın `isChangePoint` listesini yineleyin ve herhangi bir değerin dizinini yazdırın `true` . Bu değerler, varsa, eğilim değişiklik noktalarının dizinlerine karşılık gelir.

[!code-javascript[detect change points](~/cognitive-services-quickstart-code/javascript/AnomalyDetector/anomaly_detector_quickstart.js?name=changePointDetection)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node` hızlı başlangıç dosyanızdaki komutla çalıştırın.

```console
node index.js
```

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
