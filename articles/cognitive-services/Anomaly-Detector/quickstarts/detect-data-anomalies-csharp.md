---
title: 'Hızlı başlangıç: anomali algılayıcısının REST API ve C kullanarak zaman serisi verilerinizde bozukluklar algılama #'
titleSuffix: Azure Cognitive Services
description: Bir toplu iş ya da akış verileri olarak veri serinizdeki anormal verileri algılamak için anomali algılayıcı API 'sini nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 06/30/2020
ms.author: aahi
ms.openlocfilehash: 86742568d8f0c7c951d872e7df23b8ce1cb0920f
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2020
ms.locfileid: "88244236"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Hızlı başlangıç: anomali algılayıcısının REST API ve C kullanarak zaman serisi verilerinizde bozukluklar algılama #

Bu hızlı başlangıcı kullanarak, zaman serisi verilerinizde bozukluklar olup olmadığı konusunda anomali algılayıcı API 'sinin iki algılama modunu kullanmaya başlayın. Bu C# uygulaması JSON biçimli zaman serisi verilerini içeren iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıkışı                                                                                                                                         |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bir toplu iş olarak anomali algılama                        | Zaman serisi verilerinde her bir veri noktasının anomali durumunu (ve diğer verileri) ve algılanan tüm anormalilerin konumlarını içeren JSON yanıtı. |
| En son veri noktasının anomali durumunu Algıla | Zaman serisi verilerinde en son veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı.                                        |

 Bu uygulama C# dilinde yazıldığı sırada API, çoğu programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir. Bu hızlı başlangıç için kaynak kodunu [GitHub](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/csharp-detect-anomalies.cs)'da bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
- Azure aboneliğiniz olduktan sonra <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> anahtar ve uç noktanıza ulaşmak için Azure Portal bir anomali algılayıcı kaynağı oluşturun. Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    - Uygulamanızı anomali algılayıcı API 'sine bağlamak için oluşturduğunuz kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.
- Herhangi bir [Visual Studio 2017 veya üzeri](https://visualstudio.microsoft.com/downloads/) sürümü
- NuGet paketi olarak kullanılabilen [Json.NET](https://www.newtonsoft.com/json) çerçevesi. Visual Studio 'da bir NuGet paketi olarak Newtonsoft.Jsyüklemek için:

    1. **Çözüm Gezgini**' de projenize sağ tıklayın.
    2. **NuGet Paketlerini Yönet**' i seçin.
    3. *ÜzerindeNewtonsoft.Js* arayın ve paketi yükler.

- Linux/MacOS kullanıyorsanız, bu uygulama [mono](https://www.mono-project.com/)kullanılarak çalıştırılabilir.

- Zaman serisi veri noktalarını içeren bir JSON dosyası. Bu hızlı başlangıçta örnek veriler [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json)'da bulunabilir.

[!INCLUDE [anomaly-detector-environment-variables](../includes/environment-variables.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Visual Studio 'da yeni bir konsol çözümü oluşturun ve aşağıdaki paketleri ekleyin.

    [!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=usingStatements)]


2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Anomali algılama için kullanabileceğiniz URI 'Ler aşağıda verilmiştir. Bunlar, daha sonra API isteği URL 'Leri oluşturmak için hizmet uç noktanıza eklenecektir.

    | Algılama yöntemi                   | URI                                              |
    |------------------------------------|--------------------------------------------------|
    | Toplu iş algılama                    | `/anomalydetector/v1.0/timeseries/entire/detect` |
    | En son veri noktasında algılama | `/anomalydetector/v1.0/timeseries/last/detect`   |

    [!code-csharp[initial variables for endpoint, key and data file](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=vars)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan adlı yeni bir zaman uyumsuz işlev oluşturun `Request` .

2. İstemcinin güvenlik protokolünü ve üst bilgi bilgilerini bir nesne kullanarak ayarlayın `HttpClient` . Abonelik anahtarınızı üstbilgiye eklediğinizden emin olun `Ocp-Apim-Subscription-Key` . Ardından `StringContent` istek için bir nesne oluşturun.

3. İsteği ile gönderin `PostAsync()` ve ardından yanıtı döndürün.

    [!code-csharp[Request method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=requestMethod)]

## <a name="detect-anomalies-as-a-batch"></a>Bir toplu iş olarak anomali algılama

1. Adlı yeni bir işlev oluşturun `detectAnomaliesBatch()` . `Request()`Uç nokta, abonelik anahtarınız, Batch anomali algılama URL 'si ve zaman serisi verileri ile işlevi çağırarak isteği oluşturun ve gönderin.

2. JSON nesnesinin serisini kaldırma ve konsola yazma.

3. Yanıt `code` alanı içeriyorsa, hata kodunu ve hata iletisini yazdırın.

4. Aksi takdirde, veri kümesindeki anormalilerin konumlarını bulabilirsiniz. Yanıtın `isAnomaly` alanı, her biri bir veri noktasının bir anomali olup olmadığını gösteren bir Boole değerleri dizisi içerir. Bunu, yanıt nesnesinin işleviyle bir dize dizisine dönüştürün `ToObject<bool[]>()` . Dizi boyunca yineleyin ve herhangi bir değerin dizinini yazdırın `true` . Bu değerler, varsa anormal veri noktalarının dizinine karşılık gelir.

    [!code-csharp[Detect anomalies batch](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesBatch)]


## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu Algıla

1. Adlı yeni bir işlev oluşturun `detectAnomaliesLatest()` . `Request()`Uç nokta, abonelik anahtarınız, en son nokta anomali algılama URL 'si ve zaman serisi verileri ile işlevi çağırarak isteği oluşturun ve gönderin.

2. JSON nesnesinin serisini kaldırma ve konsola yazma.

    [!code-csharp[Detect anomalies latest](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesLatest)]

## <a name="load-your-time-series-data-and-send-the-request"></a>Zaman serisi verilerinizi yükleyin ve isteği gönderin

1. Uygulamanızın ana yönteminde, JSON zaman serisi verilerinizi ile yükleyin `File.ReadAllText()` .

2. Yukarıda oluşturulan anomali algılama işlevlerini çağırın. `System.Console.ReadKey()`Uygulamayı çalıştırdıktan sonra konsol penceresini açık tutmak için kullanın.

    [!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=main)]

### <a name="example-response"></a>Örnek yanıt

JSON biçiminde başarılı bir yanıt döndürülür. GitHub 'da JSON yanıtını görüntülemek için aşağıdaki bağlantılara tıklayın:
* [Örnek toplu iş algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Örnek en son nokta algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
