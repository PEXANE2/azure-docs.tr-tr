---
title: 'Hızlı başlangıç: mürekkep tanıyıcı REST API ve Java ile dijital mürekkep tanıma'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta dijital mürekkep vuruşlarını tanımayı başlatmak için mürekkep tanıyıcı API 'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 06/30/2020
ms.author: aahi
ms.openlocfilehash: bc7dfcef45917081ec9ea1e97f4c36e4348dd9e7
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85985284"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-java"></a>Hızlı başlangıç: mürekkep tanıyıcı REST API ve Java ile dijital mürekkep tanıma

Mürekkep tanıyıcı API 'sini dijital mürekkep vuruşları üzerinde kullanmaya başlamak için bu hızlı başlangıcı kullanın. Bu Java uygulaması JSON biçimli mürekkep konturu verilerini içeren bir API isteği gönderir ve yanıtı alır.

Bu uygulama Java 'da yazıldığı sırada, API birçok programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

Genellikle, API 'YI bir dijital mürekkep uygulamasının içinden çağıracağınızdan. Bu hızlı başlangıç, bir JSON dosyasından aşağıdaki el yazısı örneği için mürekkep konturu verileri gönderir.

![El yazısı metin görüntüsü](../media/handwriting-sample.jpg)

Bu hızlı başlangıç için kaynak kodu [GitHub](https://go.microsoft.com/fwlink/?linkid=2089904)' da bulabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- [Java &trade; Development kıt (JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya üzeri.

- Bu kitaplıkları Maven deposundan içeri aktar
    - [Java paketindeki JSON](https://mvnrepository.com/artifact/org.json/json)
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) paketi

- Bu hızlı başlangıç için örnek mürekkep konturu verileri [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-ink-strokes.json)'da bulunabilir.

### <a name="create-an-ink-recognizer-resource"></a>Mürekkep tanıyıcı kaynağı oluşturma

[!INCLUDE [creating an ink recognizer resource](../includes/setup-instructions.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.
    
    [!code-java[import statements](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=imports)]

2. Abonelik anahtarınız, uç nokta ve JSON dosyanız için değişkenler oluşturun. Uç nokta daha sonra mürekkep tanıyıcı URI 'sine eklenecektir.

    [!code-java[initial vars](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=vars)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan adlı yeni bir işlev oluşturun `sendRequest()` . Ardından aşağıdaki adımları gerçekleştirin.

2. `CloseableHttpClient`API 'ye istek gönderebilen bir nesne oluşturun. `HttpPut`Uç noktanızı ve mürekkep tanıyıcı URL 'sini birleştirerek isteği bir istek nesnesine gönderin.

3. `setHeader()`Üstbilgiyi ayarlamak için isteğin işlevini kullanın `Content-Type` `application/json` ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin.

4. `setEntity()`Gönderilecek verilere isteğin işlevini kullanın.   

5. `execute()`İsteği göndermek ve bir nesnesine kaydetmek için istemcinin işlevini kullanın `CloseableHttpResponse` . 

6. `HttpEntity`Yanıt içeriğini depolamak için bir nesne oluşturun. İçeriğini ile alın `getEntity()` . Yanıt boş değilse, döndürün.
    
    [!code-java[send a request](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=sendRequest)]

## <a name="send-an-ink-recognition-request"></a>Mürekkep tanıma isteği gönder

`recognizeInk()`Mürekkep vuruşu verilerinizi tanımak için adlı bir yöntem oluşturun. `sendRequest()`Uç nokta, URL, abonelik anahtarı ve JSON verileriniz ile yukarıda oluşturulan yöntemi çağırın. Sonucu alın ve konsola yazdırın.

[!code-java[recognizeInk](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=recognizeInk)]

## <a name="load-your-digital-ink-data-and-send-the-request"></a>Dijital mürekkep verilerinizi yükleyin ve isteği gönderin

1. Uygulamanızın ana yönteminde, isteklere eklenecek verileri içeren JSON dosyasını okuyun.

2. Yukarıda oluşturulan mürekkep tanıma işlevini çağırın.
    
    [!code-java[main method](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=main)]


## <a name="run-the-application-and-view-the-response"></a>Uygulamayı çalıştırma ve yanıtı görüntüleme

Uygulamayı çalıştırın. JSON biçiminde başarılı bir yanıt döndürülür. Ayrıca, [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-response.json)'da JSON yanıtını bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)


Mürekkep tanıma API 'sinin dijital bir mürekkep uygulamasında nasıl çalıştığını görmek için GitHub 'da aşağıdaki örnek uygulamalara göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
