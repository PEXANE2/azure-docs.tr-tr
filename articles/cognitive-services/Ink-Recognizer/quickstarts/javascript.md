---
title: 'Quickstart: Mürekkep Recognizer REST API ve Node.js ile dijital mürekbeyi tanıyın'
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
ms.openlocfilehash: a37f2b7044fcba04ca18093aa73563961e9e35de
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448138"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-javascript"></a>Quickstart: Mürekkep Recognizer REST API ve JavaScript ile dijital mürekbeyi tanıyın

Dijital mürekkep darbelerinde Mürekkep Tanıyıcı API'sini kullanmaya başlamak için bu hızlı başlangıcı kullanın. Bu JavaScript uygulaması JSON biçimli mürekkep kontur verilerini içeren bir API isteği gönderir ve yanıtı görüntüler.

Bu uygulama Javascript'te yazılıp web tarayıcınızda çalışırken, API çoğu programlama diliyle uyumlu bir RESTful web hizmetidir.

Genellikle bir dijital mürekkep uygulamasından API'yi çağırırsınız. Bu hızlı başlatma, json dosyasından aşağıdaki el yazısı örneği için mürekkep kontur verileri gönderir.

![el yazısı metin bir görüntü](../media/handwriting-sample.jpg)

Bu hızlı başlatmanın kaynak kodu [GitHub'da](https://go.microsoft.com/fwlink/?linkid=2089905)bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

- Bir web tarayıcısı
- Bu hızlı başlatma için örnek mürekkep darbesi verileri [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/javascript/InkRecognition/quickstart/example-ink-strokes.json)bulunabilir.

### <a name="create-an-ink-recognizer-resource"></a>Mürekkep Tanıyıcı kaynağı oluşturma

[!INCLUDE [creating an ink recognizer resource](../includes/setup-instructions.md)]

## <a name="create-a-new-application"></a>Yeni uygulama oluşturma

1. Sık kullanılan IDE veya düzenleyicinizde `.html` yeni bir dosya oluşturun. Daha sonra ekleyeceğimiz kod için temel HTML ekleyin.
    
    ```html
    <!DOCTYPE html>
    <html>
    
        <head>
            <script type="text/javascript">
            </script>
        </head>
        
        <body>
        </body>
    
    </html>
    ```

2. Etiketin `<body>` içine aşağıdaki html'i ekleyin:
    1. JSON isteğini ve yanıtını görüntülemek için iki metin alanı.
    2. Daha sonra oluşturulacak `recognizeInk()` işlevi aramak için bir düğme.
    
    ```HTML
    <!-- <body>-->
        <h2>Send a request to the Ink Recognition API</h2>
        <p>Request:</p>
        <textarea id="request" style="width:800px;height:300px"></textarea>
        <p>Response:</p>
        <textarea id="response" style="width:800px;height:300px"></textarea>
        <br>
        <button type="button" onclick="recognizeInk()">Recognize Ink</button>
    <!--</body>-->
    ```

## <a name="load-the-example-json-data"></a>Örnek JSON verilerini yükleyin

1. Etiket `<script>` içinde, örnekJson için bir değişken oluşturun. Ardından, JSON dosyanızı seçebilmeniz için dosya gezginini açan bir JavaScript işlevi `openFile()` oluşturun. `Recognize ink` Düğme tıklatıldığında, bu işlevi arar ve dosyayı okumaya başlar.
2. Dosyayı `FileReader` eşit `onload()` olarak işlemek için nesnenin işlevini kullanın. 
    1. Dosyadaki `\n` `\r` herhangi bir karakteri veya karakteri boş bir dizeyle değiştirin. 
    2. Metni `JSON.parse()` geçerli JSON'a dönüştürmek için kullanın
    3. Uygulamadaki `request` metin kutusunu güncelleştirin. JSON dizesini biçimlendirmek için kullanın. `JSON.stringify()` 
    
    ```javascript
    var sampleJson = "";
    function openFile(event) {
        var input = event.target;
    
        var reader = new FileReader();
        reader.onload = function(){
            sampleJson = reader.result.replace(/(\\r\\n|\\n|\\r)/gm, "");
            sampleJson = JSON.parse(sampleJson);
            document.getElementById('request').innerHTML = JSON.stringify(sampleJson, null, 2);
        };
        reader.readAsText(input.files[0]);
    };
    ```

## <a name="send-a-request-to-the-ink-recognizer-api"></a>Mürekkep Tanıyıcısı API'sine istek gönderme

1. Etiketin `<script>` içinde, adı `recognizeInk()`verilen bir işlev oluşturun. Bu işlev daha sonra API'yi çağırır ve sayfayı yanıtla günceller. Bu işlev içinde aşağıdaki adımlardan kodu ekleyin. 
        
    ```javascript
    function recognizeInk() {
    // add the code from the below steps here 
    }
    ```

    1. Uç nokta URL'niz, abonelik anahtarınız ve json örneği için değişkenler oluşturun. Ardından API isteğini göndermek için bir `XMLHttpRequest` nesne oluşturun. 
        
        ```javascript
        // Replace the below URL with the correct one for your subscription. 
        // Your endpoint can be found in the Azure portal. For example: "https://<your-custom-subdomain>.cognitiveservices.azure.com";
        var SERVER_ADDRESS = process.env["INK_RECOGNITION_ENDPOINT"];
        var ENDPOINT_URL = SERVER_ADDRESS + "/inkrecognizer/v1.0-preview/recognize";
        var SUBSCRIPTION_KEY = process.env["INK_RECOGNITION_SUBSCRIPTION_KEY"];
        var xhttp = new XMLHttpRequest();
        ```
    2. `XMLHttpRequest` Nesne için geri dönüş işlevini oluşturun. Bu işlev, başarılı bir istekten API yanıtını ayrıştıracak ve uygulamada görüntüler. 
            
        ```javascript
        function returnFunction(xhttp) {
            var response = JSON.parse(xhttp.responseText);
            console.log("Response: %s ", response);
            document.getElementById('response').innerHTML = JSON.stringify(response, null, 2);
        }
        ```
    3. İstek nesnesi için hata işlevini oluşturun. Bu işlev, hatayı konsola kaydeder. 
            
        ```javascript
        function errorFunction() {
            console.log("Error: %s, Detail: %s", xhttp.status, xhttp.responseText);
        }
        ```

    4. İstek nesnesinin `onreadystatechange` özelliği için bir işlev oluşturun. İstek nesnesinin hazır durum değişiklikleri, yukarıdaki return ve hata işlevleri uygulanır.
            
        ```javascript
        xhttp.onreadystatechange = function () {
            if (this.readyState === 4) {
                if (this.status === 200) {
                    returnFunction(xhttp);
                } else {
                    errorFunction(xhttp);
                }
            }
        };
        ```
    
    5. API isteğini gönderin. Abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üstbilgiye ekleyin ve `content-type``application/json`
    
        ```javascript
        xhttp.open("PUT", ENDPOINT_URL, true);
        xhttp.setRequestHeader("Ocp-Apim-Subscription-Key", SUBSCRIPTION_KEY);
        xhttp.setRequestHeader("content-type", "application/json");
        xhttp.send(JSON.stringify(sampleJson));
        };
        ```

## <a name="run-the-application-and-view-the-response"></a>Uygulamayı çalıştırın ve yanıtı görüntüleyin

Bu uygulama web tarayıcınızda çalıştırılabilir. Başarılı bir yanıt JSON biçiminde döndürülür. Ayrıca [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/javascript/InkRecognition/quickstart/example-response.json)json yanıtı bulabilirsiniz:

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)

Mürekkep Tanıma API'sinin dijital mürekkep uygulamasında nasıl çalıştığını görmek için GitHub'daki aşağıdaki örnek uygulamalara göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
