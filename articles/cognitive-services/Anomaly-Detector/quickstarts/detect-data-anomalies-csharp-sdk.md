---
title: 'Hızlı başlangıç: .NET için anomali algılayıcı istemci kitaplığını kullanarak zaman serisi verilerinde bozukluklar saptama'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, veri serinizdeki her zaman bir toplu iş veya akış verileri olarak, anormallikleri algılamak için anomali algılayıcı API 'sinin nasıl kullanılacağı gösterilmektedir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 11/19/2019
ms.author: aahi
ms.openlocfilehash: fdb35edc35e07ed4ee718281942565a8f1d061d4
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78402698"
---
# <a name="quickstart-anomaly-detector-client-library-for-net"></a>Hızlı başlangıç: .NET için anomali algılayıcı istemci kitaplığı

.NET için anomali algılayıcı istemci kitaplığını kullanmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Anomali algılayıcı hizmeti, sektör, senaryo veya veri hacminin ne olursa olsun, üzerinde en iyi şekilde sığdırma modellerini kullanarak zaman serisi verilerinizde yer alan anormallikleri bulmanıza olanak sağlar.

.NET için anomali algılayıcı istemci kitaplığını kullanarak şunları yapın:

* Toplu istek olarak zaman serisi veri kümeniz genelinde anomali algılama
* Zaman serinizdeki en son veri noktasının anomali durumunu Algıla

Kitaplık [başvuru belgeleri](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.AnomalyDetector?view=azure-dotnet-preview) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector) | [paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.AnomalyDetector/) | [GitHub 'daki kodu bulun](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/csharp-sdk-sample.cs)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* Geçerli [.NET Core](https://dotnet.microsoft.com/download/dotnet-core) sürümü
* Anomali algılayıcı anahtarı ve uç noktası

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-an-anomaly-detector-resource"></a>Anomali algılayıcısı kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="create-a-new-net-core-application"></a>Yeni bir .NET Core uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `anomaly-detector-quickstart`adlı yeni bir konsol uygulaması oluşturmak için `dotnet new` komutunu kullanın. Bu komut, tek C# bir kaynak dosyası olan basit bir "Merhaba Dünya" projesi oluşturur: *program.cs*. 

```dotnetcli
dotnet new console -n anomaly-detector-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```dotnetcli
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir. 

```output
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Uygulama dizini içinde, aşağıdaki komutla .NET için anomali algılayıcı istemci Kitaplığı ' nı bir daha yükleyeceksiniz:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.AnomalyDetector --version 0.8.0-preview
```

Proje dizininden *program.cs* dosyasını açın ve `directives`kullanarak aşağıdakini ekleyin:

[!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=usingStatements)]

Uygulamanın `main()` yönteminde, kaynağınızın Azure konumu ve anahtarınızın bir ortam değişkeni olarak değişkenlerini oluşturun. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, bu dosyayı çalıştıran düzenleyicinin, IDE 'nin veya kabuğun kapatılıp yeniden yüklenmesi gerekir.

[!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=mainMethod)]

## <a name="object-model"></a>Nesne modeli

Anomali algılayıcı istemcisi, anahtarınızı içeren [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials)kullanarak Azure 'da kimlik doğrulaması yapan bir [anorivtorclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient) nesnesidir. İstemci, iki anomali algılama yöntemi sağlar: [Entiredetectasync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync)kullanan tüm veri kümelerinde ve [lastdetectasync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync)kullanan en son veri noktasında. 

Zaman serisi verileri, [istek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request) nesnesinde bir dizi [işaret](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.series?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_Series) olarak gönderilir. `Request` nesnesi, verileri (örneğin,[ayrıntı düzeyi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.granularity) ) ve anomali algılama parametrelerini tanımlamaya yönelik özellikler içerir. 

Anomali algılayıcısı yanıtı, kullanılan yönteme bağlı olarak bir [Entiredetectresponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse) veya [lastdetectresponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse) nesnesidir. 

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için anomali algılayıcı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesi yükleme](#load-time-series-data-from-a-file)
* [Tüm veri kümesindeki anormallikleri Algıla](#detect-anomalies-in-the-entire-data-set) 
* [En son veri noktasının anomali durumunu Algıla](#detect-the-anomaly-status-of-the-latest-data-point)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Yeni bir yöntemde, uç nokta ve anahtarınızla bir istemci örneği oluşturun. Anahtarınızla bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials?view=azure-dotnet-preview) nesnesi oluşturun ve bir [Anorivtorclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-dotnet-preview) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın. 

[!code-csharp[Client authentication function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=createClient)]
    
## <a name="load-time-series-data-from-a-file"></a>Bir dosyadan zaman serisi verilerini yükle

Bu hızlı başlangıçta [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv)'dan örnek verileri indirin:
1. Tarayıcınızda, **RAW**' a sağ tıklayın.
2. **Bağlantıyı farklı kaydet**' e tıklayın.
3. Dosyayı uygulama dizininize bir. csv dosyası olarak kaydedin.

Bu zaman serisi verileri. csv dosyası olarak biçimlendirilir ve anomali algılayıcısı API 'sine gönderilir.

Zaman serisi verilerinde okumak için yeni bir yöntem oluşturun ve bunu bir [istek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request?view=azure-dotnet-preview) nesnesine ekleyin. Dosya yoluyla `File.ReadAllLines()` çağırın ve [Point](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.point?view=azure-dotnet-preview) nesnelerinin bir listesini oluşturun ve yeni satır karakterlerini kaldırın. Değerleri ayıklayın ve dateStamp değerini sayısal değerinden ayırın ve bunları yeni bir `Point` nesnesine ekleyin. 

`Request` bir nesneyi işaret dizisine ve veri noktalarının [ayrıntı düzeyi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.granularity?view=azure-dotnet-preview) (veya dönemsellik) için `Granularity.Daily` yapın.

[!code-csharp[load the time series data file](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=GetSeriesFromFile)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesindeki anormallikleri Algıla 

İstemcinin [Entiredetectasync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_EntireDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) yöntemini `Request` nesnesiyle çağırmak ve yanıtı bir [entiredetectresponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-dotnet-preview) nesnesi olarak beklemek için bir yöntem oluşturun. Zaman serisi herhangi bir anomali içeriyorsa, yanıtın [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse.isanomaly?view=azure-dotnet-preview) değerlerini yineleyin ve `true`olan her birini yazdırın. Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

[!code-csharp[EntireDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=entireDatasetExample)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

İstemcinin [Lastdetectasync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_LastDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) metodunu `Request` nesnesiyle çağırmak ve yanıtı bir [lastdetectresponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-dotnet-preview) nesnesi olarak beklemek için bir yöntem oluşturun. Gönderilen en son veri noktasının bir anomali olup olmadığını öğrenmek için yanıtın [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse.isanomaly?view=azure-dotnet-preview) özniteliğini denetleyin. 

[!code-csharp[LastDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=latestPointExample)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı uygulama dizininizdeki `dotnet run` komutuyla çalıştırın.

```dotnetcli
dotnet run
```

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
