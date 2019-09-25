---
title: 'Hızlı Başlangıç: Mürekkep tanıyıcı REST API ile dijital mürekkebi tanıma veC#'
titleSuffix: Azure Cognitive Services
description: Dijital mürekkep vuruşlarını tanımayı başlatmak için mürekkep tanıyıcı API 'sini kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: quickstart
ms.date: 09/23/2019
ms.author: aahi
ms.openlocfilehash: 86e69d75c067159a4daa637984a392a393dc46fa
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211792"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-c"></a>Hızlı Başlangıç: Mürekkep tanıyıcı REST API ile dijital mürekkebi tanıma veC#

Mürekkep tanıyıcı API 'sine dijital mürekkep vuruşları göndermeye başlamak için bu hızlı başlangıcı kullanın. Bu C# uygulama JSON biçimli mürekkep konturu verilerini IÇEREN bir API isteği gönderir ve yanıtı alır.

Bu uygulama içine C#YAZıLıRKEN, API çoğu programlama dili ile uyumlu olan bir yeniden sorun Web hizmetidir.

Genellikle, API 'YI bir dijital mürekkep uygulamasının içinden çağıracağınızdan. Bu hızlı başlangıç, bir JSON dosyasından aşağıdaki el yazısı örneği için mürekkep konturu verileri gönderir.

![El yazısı metin görüntüsü](../media/handwriting-sample.jpg)

Bu hızlı başlangıç için kaynak kodu [GitHub](https://go.microsoft.com/fwlink/?linkid=2089502)' da bulabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)’nin herhangi bir sürümü.
- [Newtonsoft.Json](https://www.newtonsoft.com/json)
    - Newtonsoft. json ' i Visual Studio 'da bir NuGet paketi olarak yüklemek için:
        1. **Çözüm Yöneticisi** 'ne sağ tıklayın
        2. **NuGet Paketlerini Yönet...** öğesine tıklayın.
        3. Paketi arayın `Newtonsoft.Json` ve yükledikten sonra
- Linux/MacOS kullanıyorsanız, bu uygulama [mono](https://www.mono-project.com/)kullanılarak çalıştırılabilir.

- Bu hızlı başlangıç için örnek mürekkep konturu verileri [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/InkRecognition/quickstart/example-ink-strokes.json)'da bulunabilir.

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]


## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Visual Studio 'da yeni bir konsol çözümü oluşturun ve aşağıdaki paketleri ekleyin. 

    ```csharp
    using System;
    using System.IO;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    ```

2. Abonelik anahtarınız ve uç noktanız için değişkenler oluşturun. Aşağıdaki uç noktayı mürekkep tanıyıcı kaynağınız için oluşturulan bir ile değiştirin. API 'ye bağlanmak için mürekkebi bir tanıyıcı URI 'sine ekleyin.

    ```csharp
    // Replace the subscriptionKey string with your valid subscription key.
    const string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    // Replace the dataPath string with a path to the JSON formatted ink stroke data.
    const string dataPath = @"PATH-TO-INK-STROKE-DATA"; 

    // URI information for ink recognition:
    const string endpoint = "https://<your-custom-subdomain>.cognitiveservices.azure.com";
    const string inkRecognitionUrl = "/inkrecognizer/v1.0-preview/recognize";
    ```

## <a name="create-a-function-to-send-requests"></a>İstek göndermek için bir işlev oluşturma

1. Yukarıda oluşturulan değişkenleri alan adlı `Request` yeni bir zaman uyumsuz işlev oluşturun.

2. İstemcinin güvenlik protokolünü ve üst bilgi bilgilerini bir `HttpClient` nesne kullanarak ayarlayın. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye eklediğinizden emin olun. Ardından istek için `StringContent` bir nesne oluşturun.
 
3. İsteği ile `PutAsync()`gönderin. İstek başarılı olursa yanıtı döndürün.  
    
    ```csharp
    static async Task<string> Request(string apiAddress, string endpoint, string subscriptionKey, string requestData){
        
        using (HttpClient client = new HttpClient { BaseAddress = new Uri(apiAddress) }){
            System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls;
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

            var content = new StringContent(requestData, Encoding.UTF8, "application/json");
            var res = await client.PutAsync(endpoint, content);
            if (res.IsSuccessStatusCode){
                return await res.Content.ReadAsStringAsync();
            }
            else{
                return $"ErrorCode: {res.StatusCode}";
            }
        }
    }
    ```

## <a name="send-an-ink-recognition-request"></a>Mürekkep tanıma isteği gönder

1. Adlı `recognizeInk()`yeni bir işlev oluşturun. İsteği oluşturun ve uç nokta, abonelik anahtarınız, `Request()` API URL 'si ve dijital mürekkep konturu verileri ile işlevi çağırarak gönderin.

2. JSON nesnesinin serisini kaldırma ve konsola yazma. 
    
    ```csharp
    static void recognizeInk(string requestData){

        //construct the request
        var result = Request(
            endpoint,
            inkRecognitionUrl,
            subscriptionKey,
            requestData).Result;

        dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
        System.Console.WriteLine(jsonObj);
    }
    ```

## <a name="load-your-digital-ink-data"></a>Dijital mürekkep verilerinizi yükleme

Mürekkep verileri JSON dosyasını `LoadJson()` yüklemek için adlı bir işlev oluşturun. Oluşturma `JsonTextReader` `StreamReader` ve`JObject` döndürme için bir ve kullanın.
    
```csharp
public static JObject LoadJson(string fileLocation){

    var jsonObj = new JObject();

    using (StreamReader file = File.OpenText(fileLocation))
    using (JsonTextReader reader = new JsonTextReader(file)){
        jsonObj = (JObject)JToken.ReadFrom(reader);
    }
    return jsonObj;
}
```

## <a name="send-the-api-request"></a>API isteğini gönder

1. Uygulamanızın ana yönteminde, JSON verilerinizi yukarıda oluşturulan işlevle yükleyin. 

2. Yukarıda oluşturulan `recognizeInk()` işlevi çağırın. Uygulamayı `System.Console.ReadKey()` çalıştırdıktan sonra konsol penceresini açık tutmak için kullanın.
    
    ```csharp
    static void Main(string[] args){

        var requestData = LoadJson(dataPath);
        string requestString = requestData.ToString(Newtonsoft.Json.Formatting.None);
        recognizeInk(requestString);
        System.Console.WriteLine("\nPress any key to exit ");
        System.Console.ReadKey();
        }
    ```

## <a name="run-the-application-and-view-the-response"></a>Uygulamayı çalıştırma ve yanıtı görüntüleme

Uygulamayı çalıştırın. JSON biçiminde başarılı bir yanıt döndürülür. Ayrıca, [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/InkRecognition/quickstart/example-response.json)'da JSON yanıtını bulabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)


Mürekkep tanıma API 'sinin dijital bir mürekkep uygulamasında nasıl çalıştığını görmek için GitHub 'da aşağıdaki örnek uygulamalara göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
