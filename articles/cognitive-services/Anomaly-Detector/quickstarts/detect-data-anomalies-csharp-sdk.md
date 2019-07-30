---
title: 'Hızlı Başlangıç: .NET için anomali algılayıcı istemci kitaplığını kullanarak zaman serisi verilerinde bozukluklar algılama'
titleSuffix: Azure Cognitive Services
description: Veri serinizdeki tüm verileri toplu olarak veya akış verilerinde saptamak için anomali algılayıcı API 'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 07/26/2019
ms.author: aahi
ms.openlocfilehash: c65b64608ade76a65dca42b72844d42ddc1b14fd
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639366"
---
# <a name="quickstart-anomaly-detector-client-library-for-net"></a>Hızlı Başlangıç: .NET için anomali algılayıcı istemci kitaplığı

.NET için anomali algılayıcı istemci kitaplığını kullanmaya başlayın. Paketi yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Anomali algılayıcı hizmeti, sektör, senaryo veya veri hacminin ne olursa olsun, üzerinde en iyi şekilde sığdırma modellerini kullanarak zaman serisi verilerinizde yer alan anormallikleri bulmanıza olanak sağlar.

.NET için anomali algılayıcı istemci kitaplığını kullanarak şunları yapın:

* Toplu istek olarak zaman serisi veri kümeniz genelinde anomali algılama
* Zaman serinizdeki en son veri noktasının anomali durumunu Algıla

[Kitaplık başvurusu belge](https://docs.microsoft.com/dotnet/api/Microsoft.Azure.CognitiveServices.AnomalyDetector?view=azure-dotnet-preview) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/AnomalyDetector) | [paketi (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.AnomalyDetector/) | [örnekleri](https://github.com/Azure-Samples/anomalydetector)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
* Geçerli [.NET Core](https://dotnet.microsoft.com/download/dotnet-core) sürümü

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-an-anomaly-detector-resource"></a>Anomali algılayıcısı kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

Deneme aboneliğinizden veya kaynağından bir anahtar aldıktan sonra adlı `ANOMALY_DETECTOR_KEY`anahtar için [bir ortam değişkeni oluşturun](../../cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) .

### <a name="create-a-new-c-application"></a>Yeni C# bir uygulama oluşturun

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir .NET Core uygulaması oluşturun. 

Konsol penceresinde (cmd, PowerShell veya Bash gibi), ad `new` `anomaly-detector-quickstart`ile yeni bir konsol uygulaması oluşturmak için DotNet komutunu kullanın. Bu komut, tek bir kaynak dosyası olan C# basit bir "Merhaba Dünya" projesi oluşturur: *Program.cs*. 

```console
dotnet new console -n anomaly-detector-quickstart
```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

```console
dotnet build
```

Derleme çıktısı hiçbir uyarı veya hata içermemelidir. 

```console
...
Build succeeded.
 0 Warning(s)
 0 Error(s)
...
```

Proje dizininden, *program.cs* dosyasını tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Aşağıdakileri kullanarak `directives`aşağıdakileri ekleyin:

[!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=usingStatements)]

Uygulamanın `main()` yönteminde, kaynağınızın Azure konumu ve anahtarınızın bir ortam değişkeni olarak değişkenlerini oluşturun. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, bu dosyayı çalıştıran düzenleyicinin, IDE 'nin veya kabuğun kapatılıp yeniden yüklenmesi gerekir.

[!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=mainMethod)]

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Uygulama dizini içinde, aşağıdaki komutla .NET için anomali algılayıcı istemci Kitaplığı ' nı bir daha yükleyeceksiniz:

```console
dotnet add package Microsoft.Azure.CognitiveServices.AnomalyDetector --version 0.8.0-preview
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı bir NuGet paketi olarak kullanılabilir. 

## <a name="object-model"></a>Nesne modeli

Anomali algılayıcı istemcisi, anahtarınızı içeren [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials)kullanarak Azure 'da kimlik doğrulaması yapan bir [anorivtorclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient) nesnesidir. İstemci iki anomali algılama yöntemi sağlar: [Entiredetectasync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync)kullanan tüm veri kümelerinde ve [lastdetectasync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync)kullanan en son veri noktasında. 

Zaman serisi verileri, [istek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request) nesnesinde bir dizi [işaret](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.series?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_Series) olarak gönderilir. Nesnesi, verileri (örneğin,[ayrıntı düzeyi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request.granularity) ) ve anomali algılama parametrelerini tanımlayacak özellikler içerir. `Request` 

Anomali algılayıcısı yanıtı, kullanılan yönteme bağlı olarak bir [Entiredetectresponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse) veya [lastdetectresponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse) nesnesidir. 

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, .NET için anomali algılayıcı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [İstemcinin kimliğini doğrulama](#authenticate-the-client)
* [Bir dosyadan zaman serisi veri kümesi yükleme](#load-time-series-data-from-a-file)
* [Tüm veri kümesindeki anormallikleri Algıla](#detect-anomalies-in-the-entire-data-set) 
* [En son veri noktasının anomali durumunu Algıla](#detect-the-anomaly-status-of-the-latest-data-point)

### <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Yeni bir yöntemde, uç nokta ve anahtarınızla bir istemci örneği oluşturun. Anahtarınızla bir [ApiKeyServiceClientCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.apikeyserviceclientcredentials?view=azure-dotnet-preview) nesnesi oluşturun ve bir [Anorivtorclient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclient?view=azure-dotnet-preview) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın. 

[!code-csharp[Client authentication function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=createClient)]
    
### <a name="load-time-series-data-from-a-file"></a>Bir dosyadan zaman serisi verilerini yükle

Bu hızlı başlangıçta [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/example-data/request-data.csv)'dan örnek verileri indirin:
1. Tarayıcınızda, **RAW**' a sağ tıklayın.
2. **Bağlantıyı farklı kaydet**' e tıklayın.
3. Dosyayı uygulama dizininize bir. csv dosyası olarak kaydedin.

Bu zaman serisi verileri. csv dosyası olarak biçimlendirilir ve anomali algılayıcısı API 'sine gönderilir.

Zaman serisi verilerinde okumak için yeni bir yöntem oluşturun ve bunu bir [istek](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.request?view=azure-dotnet-preview) nesnesine ekleyin. Dosya `File.ReadAllLines()` yolu ile çağrı yapın ve [nokta](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.point?view=azure-dotnet-preview) nesnelerinin bir listesini oluşturun ve yeni satır karakterlerini kaldırın. Değerleri ayıklayın ve dateStamp değerini sayısal değerinden ayırın ve bunları yeni `Point` bir nesneye ekleyin. 

Bir `Request` nesneyi işaret dizisine ve `Granularity.Daily` veri noktalarının [ayrıntı düzeyi](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.granularity?view=azure-dotnet-preview) (veya dönemliği) için yapın.

[!code-csharp[load the time series data file](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=GetSeriesFromFile)]

### <a name="detect-anomalies-in-the-entire-data-set"></a>Tüm veri kümesindeki anormallikleri Algıla 

İstemcinin [entiredetectasync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.entiredetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_EntireDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) yöntemini `Request` nesnesiyle çağırmak ve yanıtı bir [entiredetectresponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse?view=azure-dotnet-preview) nesnesi olarak beklemek için bir yöntem oluşturun. Zaman serisi herhangi bir anomali içeriyorsa, yanıtın [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.entiredetectresponse.isanomaly?view=azure-dotnet-preview) değerlerini yineleyin ve bunları yazdırın `true`. Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

[!code-csharp[EntireDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=entireDatasetExample)]

### <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

İstemcinin [lastdetectasync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.anomalydetectorclientextensions.lastdetectasync?view=azure-dotnet-preview#Microsoft_Azure_CognitiveServices_AnomalyDetector_AnomalyDetectorClientExtensions_LastDetectAsync_Microsoft_Azure_CognitiveServices_AnomalyDetector_IAnomalyDetectorClient_Microsoft_Azure_CognitiveServices_AnomalyDetector_Models_Request_System_Threading_CancellationToken_) yöntemini `Request` nesnesiyle çağırmak ve yanıtı bir [lastdetectresponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse?view=azure-dotnet-preview) nesnesi olarak beklemek için bir yöntem oluşturun. Gönderilen en son veri noktasının bir anomali olup olmadığını öğrenmek için yanıtın [IsAnomaly](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.anomalydetector.models.lastdetectresponse.isanomaly?view=azure-dotnet-preview) özniteliğini denetleyin. 

[!code-csharp[LastDetectSampleAsync() function](~/samples-anomaly-detector/quickstarts/sdk/csharp-sdk-sample.cs?name=latestPointExample)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizde DotNet `run` komutuyla uygulamayı çalıştırın.

```dotnet
dotnet run
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

* [Portal](../../cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure CLI](../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

Kaynak grubunu ve ilişkili kaynaklarını kaldırmak için aşağıdaki Cloud Shell komutunu da çalıştırabilirsiniz. Bu işlem birkaç dakika sürebilir. 

```azurecli-interactive
az group delete --name example-anomaly-detector-resource-group
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Azure Databricks ile akış anomali algılama](../tutorials/anomaly-detection-streaming-databricks.md)

* [Anomali ALGıLAYıCı API nedir?](../overview.md)
* Anomali algılayıcı API 'sini kullanırken [en iyi uygulamalar](../concepts/anomaly-detection-best-practices.md) .
* Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/sdk/csharp-sdk-sample.cs)' da bulunabilir.
