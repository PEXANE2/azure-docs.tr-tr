---
title: Anomali Dedektörü .NET istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/16/2020
ms.author: aahi
ms.openlocfilehash: f5f135dd44ad9e5ebd1df2144295f9a3cf3cb743
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81760003"
---
.NET için Anomaly Detector istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin. Anomaly Detector hizmeti, endüstri, senaryo veya veri hacmine bakılmaksızın, üzerinde otomatik olarak en uygun modelleri kullanarak zaman serisi verilerinizdeki anormallikleri bulmanızı sağlar.

.NET için Anomaly Detector istemci kitaplığını kullanın:

* Toplu iş isteği olarak, zaman serisi veri setiniz boyunca anormallikleri algılama
* Zaman serinizdeki en son veri noktasının anomali durumunu algılama

[Kütüphane başvuru belgeleri](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.AnomalyDetector?view=azure-dotnet-preview) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector) | [Paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.AnomalyDetector/) | [GitHub üzerinde kodu bulun](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/csharp-sdk-sample.cs)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [Ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* [.NET Core'un](https://dotnet.microsoft.com/download/dotnet-core) geçerli sürümü
* Anomali dedektörü anahtarı ve bitiş noktası

## <a name="setting-up"></a>Ayarlama

### <a name="create-an-anomaly-detector-resource"></a>Anomali Dedektörü kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

### <a name="create-a-new-net-core-application"></a>Yeni bir .NET Core uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), `dotnet new` adı `anomaly-detector-quickstart`olan yeni bir konsol uygulaması oluşturmak için komutu kullanın. Bu komut, tek bir C# kaynak dosyası ile basit bir "Hello World" projesi oluşturur: *Program.cs*. 

```dotnetcli
dotnet new console -n anomaly-detector-quickstart
```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

```dotnetcli
dotnet build
```

Yapı çıktısı hiçbir uyarı veya hata içermemelidir. 

```output
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

### <a name="install-the-client-library"></a>İstemci kitaplığını yükleme

Uygulama dizininde ,.NET için Anomaly Detector istemci kitaplığını aşağıdaki komutla yükleyin:

```dotnetcli
dotnet add package Microsoft.Azure.CognitiveServices.AnomalyDetector --version 0.8.0-preview
```

Proje *dizininden, program.cs* dosyasını açın ve `directives`aşağıdakileri kullanarak ekleyin:

[!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=usingStatements)]

Uygulama `main()` yönteminde, kaynağınızın Azure konumu için değişkenler ve ortam değişkeni olarak anahtarınız için değişkenler oluşturun. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, düzenleyici, IDE veya kabuk çalıştıran değişkene erişmek için kapatılması ve yeniden yüklenmesi gerekir.

[!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=mainMethod)]

## <a name="object-model"></a>Nesne modeli

Anomaly Detector istemcisi, anahtarınızı içeren [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials)kullanarak Azure'a doğrulayan bir [AnomalyDetectorClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient) nesnesidir. İstemci iki anomali algılama yöntemi sağlar: [EntireDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync)kullanarak tüm veri kümesinde ve [LastDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync)kullanarak en son veri noktasında. 

Zaman serisi verileri, [İstek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request) nesnesinde [bir puan](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.series?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_Series) dizisi olarak gönderilir. Nesne, `Request` verileri (örneğin[tanecikli)](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.granularity) açıklayan özellikler ve anomali algılama parametreleri içerir. 

Anomali Dedektörü yanıtı, kullanılan yönteme bağlı olarak bir [EntireDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse) veya [LastDetectResponse nesnesidir.](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse) 

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları ,NET için Anomaly Detector istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [İstemcinin kimliğini doğrula](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesini yükleme](#load-time-series-data-from-a-file)
* [Tüm veri kümesindeki anormallikleri algılama](#detect-anomalies-in-the-entire-data-set) 
* [En son veri noktasının anomali durumunu algılama](#detect-the-anomaly-status-of-the-latest-data-point)

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Yeni bir yöntemde, son noktanız ve anahtarınızla istemciyi anında anons edin. Anahtarınızla bir [ApiKeyServiceClientCredentials nesnesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials?view=azure-dotnet-preview) oluşturun ve bir [AnomalyDetectorClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-dotnet-preview) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın. 

[!code-csharp[Client authentication function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=createClient)]
    
## <a name="load-time-series-data-from-a-file"></a>Dosyadan zaman serisi verileri yükleme

[GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv)bu hızlı başlangıç için örnek verileri indirin:
1. Tarayıcınızda **Raw'a**sağ tıklayın.
2. **Olarak Kaydet bağlantısını**tıklatın.
3. .csv dosyası olarak dosyayı uygulama dizininize kaydedin.

Bu zaman serisi verileri .csv dosyası olarak biçimlendirilir ve Anomaly Detector API'sine gönderilir.

Zaman serisi verilerinde okumak ve [istek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request?view=azure-dotnet-preview) nesnesine eklemek için yeni bir yöntem oluşturun. Dosya `File.ReadAllLines()` yolunu arayın ve [Nokta](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.point?view=azure-dotnet-preview) nesnelerinin bir listesini oluşturun ve yeni satır karakterlerini sök. Değerleri ayıklayın ve datestamp'ı sayısal değerinden ayırın `Point` ve bunları yeni bir nesneye ekleyin. 

Nokta `Request` serileri ve `Granularity.Daily` veri noktalarının [parçalılığı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.granularity?view=azure-dotnet-preview) (veya periyodikliği) için bir nesne yapın.

[!code-csharp[load the time series data file](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=GetSeriesFromFile)]

## <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesindeki anormallikleri algılama 

İstemcinin [TümDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_EntireDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) yöntemini nesneyle aramak için bir yöntem oluşturun ve [yanıtı EntireDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-dotnet-preview) nesnesi olarak bekleyin. `Request` Zaman serisi herhangi bir anormallik içeriyorsa, yanıtın [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse.isanomaly?view=azure-dotnet-preview) değerlerini yineleyin `true`ve . Varsa, bu değerler anormal veri noktalarıdizine karşılık gelir.

[!code-csharp[EntireDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=entireDatasetExample)]

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu algılama

İstemcinin [LastDetectAsync()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_LastDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) yöntemini `Request` nesneyle birlikte çağırmak için bir yöntem oluşturun ve [yanıtı LastDetectResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-dotnet-preview) nesnesi olarak bekleyin. Gönderilen en son veri noktasının bir anormallik olup olmadığını belirlemek için yanıtın [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse.isanomaly?view=azure-dotnet-preview) özniteliğini kontrol edin. 

[!code-csharp[LastDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=latestPointExample)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizin `dotnet run` komutu yla uygulamayı çalıştırın.

```dotnetcli
dotnet run
```

[!INCLUDE [anomaly-detector-next-steps](../quickstart-cleanup-next-steps.md)]
