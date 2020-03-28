---
title: "Hızlı başlangıç: Anomali Dedektörü REST API ve C'yi kullanarak zaman serisi verilerinizdeki anormallikleri tespit edin #"
titleSuffix: Azure Cognitive Services
description: Veri serinizdeki anormallikleri toplu olarak veya veri akışı nda algılamak için Anomali Dedektörü API'sını nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: b53fecad3655048a7b9d799134926b2730b16dae
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80239102"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Hızlı başlangıç: Anomali Dedektörü REST API ve C'yi kullanarak zaman serisi verilerinizdeki anormallikleri tespit edin # 

Zaman serisi verilerinizdeki anormallikleri tespit etmek için Anomali Dedektörü API'sının iki algılama modunu kullanmaya başlamak için bu hızlı başlangıcı kullanın. Bu C# uygulaması JSON biçimlendirilmiş zaman serisi verilerini içeren iki API isteği gönderir ve yanıtları alır.

| API isteği                                        | Uygulama çıktısı                                                                                                                                         |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Anomalileri toplu olarak algılama                        | Zaman serisi verilerindeki her veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı ve algılanan anomalilerin konumları. |
| En son veri noktasının anomali durumunu algılama | Zaman serisi verilerindeki en son veri noktası için anomali durumunu (ve diğer verileri) içeren JSON yanıtı.                                        |

 Bu uygulama C# olarak yazılmış olsa da, API çoğu programlama dili yle uyumlu bir RESTful web hizmetidir. Bu hızlı başlatmanın kaynak kodunu [GitHub'da](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/csharp-detect-anomalies.cs)bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2017 veya sonrası](https://visualstudio.microsoft.com/downloads/)herhangi bir baskı ,
- Anomali dedektörü anahtarı ve bitiş noktası
- NuGet paketi olarak kullanılabilen [Json.NET](https://www.newtonsoft.com/json) çerçevesi. Visual Studio'da Newtonsoft.Json'ı NuGet paketi olarak yüklemek için:
    
    1. **Çözüm Gezgini'nde**projenizi sağ tıklatın.
    2. **NuGet Paketlerini Yönet'i**seçin.
    3. *Newtonsoft.Json'ı* arayın ve paketi yükleyin.

- Linux/MacOS kullanıyorsanız, bu uygulama [Mono](https://www.mono-project.com/)kullanılarak çalıştırılabilir.

- Zaman serisi veri noktaları içeren bir JSON dosyası. Bu hızlı başlatma için örnek veriler [GitHub'da](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json)bulunabilir.

### <a name="create-an-anomaly-detector-resource"></a>Anomali Dedektörü kaynağı oluşturma

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Visual Studio'da yeni bir konsol çözümü oluşturun ve aşağıdaki paketleri ekleyin. 

    [!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=usingStatements)]


2. Abonelik anahtarınız ve bitiş noktanız için değişkenler oluşturun. Aşağıda anomali tespiti için kullanabileceğiniz ÜRB'ler yer almaktadır. Bunlar, API istek URL'lerini oluşturmak için daha sonra hizmet bitiş noktanıza eklenir.

    | Algılama yöntemi                   | URI                                              |
    |------------------------------------|--------------------------------------------------|
    | Toplu işlem algılama                    | `/anomalydetector/v1.0/timeseries/entire/detect` |
    | En son veri noktasında algılama | `/anomalydetector/v1.0/timeseries/last/detect`   |
        
    [!code-csharp[initial variables for endpoint, key and data file](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=vars)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri `Request` alan yeni bir async işlevi oluşturun.

2. Bir `HttpClient` nesneyi kullanarak istemcinin güvenlik protokolü ve üstbilgi bilgilerini ayarlayın. Abonelik anahtarınızı üstbilgiye `Ocp-Apim-Subscription-Key` eklediğinizden emin olun. Sonra istek `StringContent` için bir nesne oluşturun.

3. İstek ile `PostAsync()`gönderin ve ardından yanıtı döndürün.

    [!code-csharp[Request method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=requestMethod)]

## <a name="detect-anomalies-as-a-batch"></a>Anomalileri toplu olarak algılama

1. 'li yeni `detectAnomaliesBatch()`bir işlev oluşturun. İsteği oluşturve `Request()` bitiş noktanız, abonelik anahtarınız, toplu düzeltme eki algılama URL'niz ve zaman serisi verilerinizle işlevi arayarak gönderin.

2. JSON nesnesini deserialize edin ve konsola yazın.

3. Yanıt alanı `code` içeriyorsa, hata kodunu ve hata iletisini yazdırın. 

4. Aksi takdirde, veri kümesindeki anomalilerin konumlarını bulun. Yanıtın `isAnomaly` alanı, her biri bir veri noktasının bir anormallik olup olmadığını gösteren bir dizi boolean değeri içerir. Yanıt nesnesinin `ToObject<bool[]>()` işlevi ile bu bir dize dizi dönüştürün. Dizi boyunca yineleyin ve tüm `true` değerlerin dizinini yazdırın. Varsa, bu değerler anormal veri noktalarıdizine karşılık gelir.

    [!code-csharp[Detect anomalies batch](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesBatch)]


## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>En son veri noktasının anomali durumunu algılama

1. 'li yeni `detectAnomaliesLatest()`bir işlev oluşturun. İsteği oluşturve `Request()` bitiş noktanız, abonelik anahtarınız, en son nokta anomali algılamaurliniz ve zaman serisi verilerinizle işlevi arayarak gönderin.

2. JSON nesnesini deserialize edin ve konsola yazın.

    [!code-csharp[Detect anomalies latest](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesLatest)]
 
## <a name="load-your-time-series-data-and-send-the-request"></a>Zaman serisi verilerinizi yükleyin ve isteği gönderin

1. Uygulamanızın ana yönteminde, JSON zaman serisi `File.ReadAllText()`verilerinizi . 

2. Yukarıda oluşturulan anomali algılama fonksiyonlarını arayın. Uygulamayı `System.Console.ReadKey()` çalıştırdıktan sonra konsol penceresini açık tutmak için kullanın.

    [!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=main)]

### <a name="example-response"></a>Örnek yanıt

Başarılı bir yanıt JSON biçiminde döndürülür. GitHub'da JSON yanıtını görüntülemek için aşağıdaki bağlantıları tıklayın:
* [Örnek toplu iş algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Örnek son nokta algılama yanıtı](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]
