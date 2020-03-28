---
title: 'Quickstart: Mürekkep Recognizer REST API ve C ile dijital mürekçeyi tanıyın #'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıç, dijital mürekkep konturlarını tanımaya başlamak için Mürekkep Tanııcı API'nin nasıl kullanılacağını gösterir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: aahi
ms.openlocfilehash: c24d055f1904453d2f512a278f00e23c6fea1d9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371372"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-c"></a>Quickstart: Mürekkep Recognizer REST API ve C ile dijital mürekçeyi tanıyın #

Mürekkep Tanıyıcısı API'sine dijital mürekkep konturları göndermeye başlamak için bu hızlı başlatmayı kullanın. Bu C# uygulaması JSON biçimli mürekkep kontur verilerini içeren bir API isteği gönderir ve yanıtı alır.

Bu uygulama C# olarak yazılmış olsa da, API çoğu programlama dili yle uyumlu bir RESTful web hizmetidir.

Genellikle bir dijital mürekkep uygulamasından API'yi çağırırsınız. Bu hızlı başlatma, json dosyasından aşağıdaki el yazısı örneği için mürekkep kontur verileri gönderir.

![el yazısı metin bir görüntü](../media/handwriting-sample.jpg)

Bu hızlı başlatmanın kaynak kodu [GitHub'da](https://go.microsoft.com/fwlink/?linkid=2089502)bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)herhangi bir baskı .
- [Newtonsoft.Json](https://www.newtonsoft.com/json)
    - Visual studio bir NuGet paketi olarak Newtonsoft.Json yüklemek için:
        1. **Çözüm Yöneticisi'ne** sağ tıklayın
        2. **NuGet Paketlerini Yönet'e tıklayın...**
        3. Paketi `Newtonsoft.Json` arayın ve kurun
- Linux/MacOS kullanıyorsanız, bu uygulama [Mono](https://www.mono-project.com/)kullanılarak çalıştırılabilir.

- Bu hızlı başlatma için örnek mürekkep darbesi verileri [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Vision/InkRecognition/quickstart/example-ink-strokes.json)bulunabilir.

### <a name="create-an-ink-recognizer-resource"></a>Mürekkep Tanıyıcı kaynağı oluşturma

[!INCLUDE [creating-an-ink-recognizer-resource](../includes/setup-instructions.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Visual Studio'da yeni bir konsol çözümü oluşturun ve aşağıdaki paketleri ekleyin. 
    
    [!code-csharp[import statements](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=imports)]

2. Abonelik anahtarınız ve bitiş noktanız ve örnek JSON dosyanız için değişkenler oluşturun. Bitiş noktası daha sonra `inkRecognitionUrl` API'ye erişmek için birleştirilir. 

    [!code-csharp[endpoint file and key variables](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=vars)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri `Request` alan yeni bir async işlevi oluşturun.

2. Bir `HttpClient` nesneyi kullanarak istemcinin güvenlik protokolü ve üstbilgi bilgilerini ayarlayın. Abonelik anahtarınızı üstbilgiye `Ocp-Apim-Subscription-Key` eklediğinizden emin olun. Sonra istek `StringContent` için bir nesne oluşturun.
 
3. İsteği `PutAsync()`. İstek başarılı olursa, yanıtı döndürün.  
    
    [!code-csharp[request example method](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=request)]

## <a name="send-an-ink-recognition-request"></a>Mürekkep tanıma isteği gönderme

1. 'li yeni `recognizeInk()`bir işlev oluşturun. İsteği oluşturve `Request()` bitiş noktanız, abonelik anahtarınız, API URL'niz ve dijital mürekkep darbesi verilerinizle işlevi arayarak gönderin.

2. JSON nesnesini deserialize edin ve konsola yazın. 
    
    [!code-csharp[request to recognize ink data](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=recognize)]

## <a name="load-your-digital-ink-data"></a>Dijital mürekkep verilerinizi yükleyin

Mürekkep verileri `LoadJson()` JSON dosyasını yüklemek için çağrılan bir işlev oluşturun. A `StreamReader` ve `JsonTextReader` a `JObject` oluşturmak ve geri vermek için kullanın.

[!code-csharp[load the JSON file](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=loadJson)]

## <a name="send-the-api-request"></a>API isteğini gönderme

1. Uygulamanızın ana yönteminde, JSON verilerinizi yukarıda oluşturulan işlek ile yükleyin. 

2. Yukarıda `recognizeInk()` oluşturulan işlevi arayın. Uygulamayı `System.Console.ReadKey()` çalıştırdıktan sonra konsol penceresini açık tutmak için kullanın.
    
    [!code-csharp[file main method](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=main)]


## <a name="run-the-application-and-view-the-response"></a>Uygulamayı çalıştırın ve yanıtı görüntüleyin

Uygulamayı çalıştırın. Başarılı bir yanıt JSON biçiminde döndürülür. Ayrıca [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Vision/InkRecognition/quickstart/example-response.json)json yanıtı bulabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)


Mürekkep Tanıma API'sinin dijital mürekkep uygulamasında nasıl çalıştığını görmek için GitHub'daki aşağıdaki örnek uygulamalara göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
