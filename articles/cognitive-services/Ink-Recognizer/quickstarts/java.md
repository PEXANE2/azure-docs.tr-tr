---
title: 'Quickstart: Mürekkep Recognizer REST API ve Java ile dijital mürekçeyi tanıyın'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta dijital mürekkep konturlarını tanımaya başlamak için Mürekkep Tanıyıcı API'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: aahi
ms.openlocfilehash: d2cd4e56477ea39587ce318538c9ddd84c51b03b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448115"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-java"></a>Quickstart: Mürekkep Recognizer REST API ve Java ile dijital mürekçeyi tanıyın

Dijital mürekkep darbelerinde Mürekkep Tanıyıcı API'sini kullanmaya başlamak için bu hızlı başlangıcı kullanın. Bu Java uygulaması JSON biçimli mürekkep kontur verilerini içeren bir API isteği gönderir ve yanıtı alır.

Bu uygulama Java'da yazılmış olsa da, API çoğu programlama diliyle uyumlu bir RESTful web hizmetidir.

Genellikle bir dijital mürekkep uygulamasından API'yi çağırırsınız. Bu hızlı başlatma, json dosyasından aşağıdaki el yazısı örneği için mürekkep kontur verileri gönderir.

![el yazısı metin bir görüntü](../media/handwriting-sample.jpg)

Bu hızlı başlatmanın kaynak kodu [GitHub'da](https://go.microsoft.com/fwlink/?linkid=2089904)bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

- [Java&trade; Geliştirme Kiti(JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) veya daha sonra.

- Bu kütüphaneleri Maven Deposu'ndan alma
    - [Java paketinde JSON](https://mvnrepository.com/artifact/org.json/json)
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) paketi

- Bu hızlı başlatma için örnek mürekkep darbesi verileri [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-ink-strokes.json)bulunabilir.

### <a name="create-an-ink-recognizer-resource"></a>Mürekkep Tanıyıcı kaynağı oluşturma

[!INCLUDE [creating an ink recognizer resource](../includes/setup-instructions.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Sık kullandığınız IDE ortamında veya düzenleyicide yeni bir Java projesi oluşturun ve aşağıdaki kitaplıkları içeri aktarın.
    
    [!code-java[import statements](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=imports)]

2. Abonelik anahtarınız, bitiş noktanız ve JSON dosyanız için değişkenler oluşturun. Bitiş noktası daha sonra Mürekça tanıyan URI'ye eklenir.

    [!code-java[initial vars](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=vars)]

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan yeni `sendRequest()` bir işlev oluşturun. Ardından aşağıdaki adımları gerçekleştirin.

2. API'ye istek gönderebilecek bir `CloseableHttpClient` nesne oluşturun. Bitiş noktanızı `HttpPut` ve Mürekkep Tanıyıcısı URL'sini birleştirerek isteği bir istek nesnesine gönderin.

3. Üstbilgiyi `application/json`ayarlamak `setHeader()` ve abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye eklemek için isteğin işlevini kullanın. `Content-Type`

4. Gönderilecek veriler için `setEntity()` isteğin işlevini kullanın.   

5. İstem göndermek `execute()` ve bir `CloseableHttpResponse` nesneye kaydetmek için istemcinin işlevini kullanın. 

6. Yanıt `HttpEntity` içeriğini depolamak için bir nesne oluşturun. İçeriği `getEntity()`' ile alın. Yanıt boş değilse, iade edin.
    
    [!code-java[send a request](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=sendRequest)]

## <a name="send-an-ink-recognition-request"></a>Mürekkep tanıma isteği gönderme

Mürekkep darbesi `recognizeInk()` verilerinizi tanımak için çağrılan bir yöntem oluşturun. Bitiş `sendRequest()` noktanız, url'niz, abonelik anahtarınız ve json verilerinizle yukarıda oluşturulan yöntemi arayın. Sonucu alın ve konsola yazdırın.

[!code-java[recognizeInk](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=recognizeInk)]

## <a name="load-your-digital-ink-data-and-send-the-request"></a>Dijital mürekkep verilerinizi yükleyin ve isteği nizi gönderin

1. Uygulamanızın ana yönteminde, isteklere eklenecek verileri içeren JSON dosyasında okuyun.

2. Yukarıda oluşturulan mürekkep tanıma işlevini arayın.
    
    [!code-java[main method](~/cognitive-services-rest-samples/java/InkRecognition/quickstart/RecognizeInk.java?name=main)]


## <a name="run-the-application-and-view-the-response"></a>Uygulamayı çalıştırın ve yanıtı görüntüleyin

Uygulamayı çalıştırın. Başarılı bir yanıt JSON biçiminde döndürülür. Ayrıca [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/InkRecognition/quickstart/example-response.json)json yanıtı bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)


Mürekkep Tanıma API'sinin dijital mürekkep uygulamasında nasıl çalıştığını görmek için GitHub'daki aşağıdaki örnek uygulamalara göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
