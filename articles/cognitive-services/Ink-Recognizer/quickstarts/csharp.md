---
title: 'Hızlı başlangıç: mürekkep tanıyıcı REST API ve C ile dijital mürekkep tanıma #'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta dijital mürekkep vuruşlarını tanımayı başlatmak için mürekkep tanıyıcı API 'sinin nasıl kullanılacağı gösterilmektedir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 06/30/2020
ms.author: aahi
ms.openlocfilehash: f61566c3d5efa7f973141253353fde7e4893bc6e
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85986083"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-c"></a>Hızlı başlangıç: mürekkep tanıyıcı REST API ve C ile dijital mürekkep tanıma #

Mürekkep tanıyıcı API 'sine dijital mürekkep vuruşları göndermeye başlamak için bu hızlı başlangıcı kullanın. Bu C# uygulaması JSON biçimli mürekkep konturu verilerini içeren bir API isteği gönderir ve yanıtı alır.

Bu uygulama C# dilinde yazıldığı sırada API, çoğu programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

Genellikle, API 'YI bir dijital mürekkep uygulamasının içinden çağıracağınızdan. Bu hızlı başlangıç, bir JSON dosyasından aşağıdaki el yazısı örneği için mürekkep konturu verileri gönderir.

![El yazısı metin görüntüsü](../media/handwriting-sample.jpg)

Bu hızlı başlangıç için kaynak kodu [GitHub](https://go.microsoft.com/fwlink/?linkid=2089502)' da bulabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Herhangi bir [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)sürümü.
- [Newtonsoft.Json](https://www.newtonsoft.com/json)
    - Visual Studio 'da bir NuGet paketi olarak Newtonsoft.Jsyüklemek için:
        1. **Çözüm Yöneticisi** 'ne sağ tıklayın
        2. **NuGet Paketlerini Yönet...** öğesine tıklayın.
        3. Paketi arayın `Newtonsoft.Json` ve yükledikten sonra
- Linux/MacOS kullanıyorsanız, bu uygulama [mono](https://www.mono-project.com/)kullanılarak çalıştırılabilir.

- Bu hızlı başlangıç için örnek mürekkep konturu verileri [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Vision/InkRecognition/quickstart/example-ink-strokes.json)'da bulunabilir.

### <a name="create-an-ink-recognizer-resource"></a>Mürekkep tanıyıcı kaynağı oluşturma

[!INCLUDE [creating-an-ink-recognizer-resource](../includes/setup-instructions.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Visual Studio 'da yeni bir konsol çözümü oluşturun ve aşağıdaki paketleri ekleyin. 
    
    [!code-csharp[import statements](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=imports)]

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun ve örnek JSON dosyası. Uç nokta daha sonra `inkRecognitionUrl` , API 'ye erişmek için ile birleştirilir. 

    [!code-csharp[endpoint file and key variables](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=vars)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan adlı yeni bir zaman uyumsuz işlev oluşturun `Request` .

2. İstemcinin güvenlik protokolünü ve üst bilgi bilgilerini bir nesne kullanarak ayarlayın `HttpClient` . Abonelik anahtarınızı üstbilgiye eklediğinizden emin olun `Ocp-Apim-Subscription-Key` . Ardından `StringContent` istek için bir nesne oluşturun.
 
3. İsteği ile gönderin `PutAsync()` . İstek başarılı olursa yanıtı döndürün.  
    
    [!code-csharp[request example method](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=request)]

## <a name="send-an-ink-recognition-request"></a>Mürekkep tanıma isteği gönder

1. Adlı yeni bir işlev oluşturun `recognizeInk()` . İsteği oluşturun ve `Request()` uç nokta, abonelik anahtarınız, API URL 'si ve dijital mürekkep konturu verileri ile işlevi çağırarak gönderin.

2. JSON nesnesinin serisini kaldırma ve konsola yazma. 
    
    [!code-csharp[request to recognize ink data](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=recognize)]

## <a name="load-your-digital-ink-data"></a>Dijital mürekkep verilerinizi yükleme

`LoadJson()`Mürekkep VERILERI json dosyasını yüklemek için adlı bir işlev oluşturun. Oluşturma ve `StreamReader` döndürme için bir ve kullanın `JsonTextReader` `JObject` .

[!code-csharp[load the JSON file](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=loadJson)]

## <a name="send-the-api-request"></a>API isteğini gönder

1. Uygulamanızın ana yönteminde, JSON verilerinizi yukarıda oluşturulan işlevle yükleyin. 

2. `recognizeInk()`Yukarıda oluşturulan işlevi çağırın. `System.Console.ReadKey()`Uygulamayı çalıştırdıktan sonra konsol penceresini açık tutmak için kullanın.
    
    [!code-csharp[file main method](~/cognitive-services-rest-samples/dotnet/Vision/InkRecognition/quickstart/recognizeInk.cs?name=main)]


## <a name="run-the-application-and-view-the-response"></a>Uygulamayı çalıştırma ve yanıtı görüntüleme

Uygulamayı çalıştırın. JSON biçiminde başarılı bir yanıt döndürülür. Ayrıca, [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Vision/InkRecognition/quickstart/example-response.json)'da JSON yanıtını bulabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)


Mürekkep tanıma API 'sinin dijital bir mürekkep uygulamasında nasıl çalıştığını görmek için GitHub 'da aşağıdaki örnek uygulamalara göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
