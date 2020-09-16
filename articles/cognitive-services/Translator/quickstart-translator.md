---
title: 'Hızlı başlangıç: çevirmenle çalışmaya başlama'
titleSuffix: Azure Cognitive Services
description: Çeviri hizmeti ile metni çevirmeye, metin alfabede dile ve daha fazlasını algılamaya öğrenin. Örnekler C#, Java, JavaScript ve Python içinde verilmiştir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 09/14/2020
ms.author: erhopf
ms.custom: cog-serv-seo-aug-2020
keywords: Çevirmen, çevirmen hizmeti, çevirme metni, alfabedeki metin, dil algılama
ms.openlocfilehash: 65f16c9689984a493e3fed041545557688fbc279
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90602260"
---
# <a name="quickstart-get-started-with-translator"></a>Hızlı başlangıç: çevirmenle çalışmaya başlama

Bu hızlı başlangıçta, REST aracılığıyla Translator hizmetini kullanmayı öğreneceksiniz. Temel örneklerle başlayıp geliştirme sırasında yaygın olarak kullanılan bazı temel yapılandırma seçeneklerine geçebilirsiniz; örneğin:

* [İde](#translate-text)
* [Alfabeye çevirme](#transliterate-text)
* [Dil tanımlama/algılama](#detect-language)
* [Tümce uzunluğunu hesapla](#get-sentence-length)
* Bir tümcede [alternatif çeviriler](#dictionary-lookup-alternate-translations) ve [sözcük kullanımının örneklerini](#dictionary-examples-translations-in-context) alın

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduğunda, anahtarınızı ve uç noktanızı almak için Azure portal [bir çevirmen kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation) . Dağıtıldıktan sonra **Kaynağa Git**' i seçin.
  * Uygulamanızı Translator hizmetine bağlamak için kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
  * Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını (F0) kullanabilirsiniz.

## <a name="platform-setup"></a>Platform Kurulumu

# <a name="c"></a>[C#](#tab/csharp)

* Yeni bir proje oluşturun: `dotnet new console -o your_project_name`
* Program.cs öğesini aşağıda gösterilen C# kodu ile değiştirin.
* Program.cs ' de abonelik anahtarını ve uç nokta değerlerini ayarlayın.
* [.Net CLI kullanarak Newtonsoft.Jsekleyin](https://www.nuget.org/packages/Newtonsoft.Json/).
* Programı proje dizininden çalıştırın: ``dotnet run``

# <a name="go"></a>[Git](#tab/go)

* Sık kullandığınız kod düzenleyicisinde yeni bir Go projesi oluşturun.
* Aşağıda sağlanan kodu ekleyin.
* `subscriptionKey` değerini, aboneliğiniz için geçerli olan bir erişim anahtarı ile değiştirin.
* Dosyayı '.go' uzantısıyla kaydedin.
* Go yüklü bir bilgisayarda bir komut istemi açın.
* Dosyayı oluşturun, örneğin: ' Go Build example-Code. go '.
* Dosyayı çalıştırın, örneğin: ' example-Code '.

# <a name="java"></a>[Java](#tab/java)

* Projeniz için bir çalışma dizini oluşturun. Örneğin: `mkdir sample-project`.
* Projenizi Gradle: ile başlatın `gradle init --type basic` . Bir **DSL**seçmeniz Istendiğinde, **Kotlin**' ı seçin.
* Güncelleştirin `build.gradle.kts` . Örneğe bağlı olarak güncelleştirmeniz gerektiğini aklınızda bulundurun `mainClassName` .
  ```java
  plugins {
    java
    application
  }
  application {
    mainClassName = "<NAME OF YOUR CLASS>"
  }
  repositories {
    mavenCentral()
  }
  dependencies {
    compile("com.squareup.okhttp:okhttp:2.5.0")
    compile("com.google.code.gson:gson:2.8.5")
  }
  ```
* Bir Java dosyası oluşturun ve kodu belirtilen örnekten kopyalayın. Abonelik anahtarınızı eklemeyi unutmayın.
* Örneği çalıştırın: `gradle run` .

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

* En sevdiğiniz IDE veya düzenleyicide yeni bir proje oluşturun.
* Örneklerden birindeki kodu projenize kopyalayın.
* Abonelik anahtarınızı ayarlayın.
* Programı çalıştırın. Örneğin: `node Translate.js`.

# <a name="python"></a>[Python](#tab/python)

* En sevdiğiniz IDE veya düzenleyicide yeni bir proje oluşturun.
* Örneklerden birindeki kodu projenize kopyalayın.
* Abonelik anahtarınızı ayarlayın.
* Programı çalıştırın. Örneğin: `python translate.py`.

---

## <a name="headers"></a>Üst Bilgiler 

Translator hizmetini REST aracılığıyla çağırırken, her istek için aşağıdaki üstbilgilerin eklendiğinden emin olmanız gerekir. Endişelenmeyin, aşağıdaki bölümlerde örnek koda başlıklar dahil edeceğiz. 

<table width="100%">
  <th width="20%">Üst Bilgiler</th>
  <th>Description</th>
  <tr>
    <td>Kimlik doğrulama üst bilgisi</td>
    <td><em>Gerekli istek üst bilgisi</em>.<br/><a href="https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication">Kimlik doğrulaması için kullanılabilen seçeneklere</a>bakın.</td>
  </tr>
  <tr>
    <td>İçerik Türü</td>
    <td><em>Gerekli istek üst bilgisi</em>.<br/>Yükün içerik türünü belirtir.<br/> Kabul edilen değer <code>application/json; charset=UTF-8</code> .</td>
  </tr>
  <tr>
    <td>İçerik Uzunluğu</td>
    <td><em>Gerekli istek üst bilgisi</em>.<br/>İstek gövdesinin uzunluğu.</td>
  </tr>
  <tr>
    <td>X-Clienttraceıd</td>
    <td><em>Isteğe bağlı</em>.<br/>İsteği benzersiz şekilde tanımlamak için istemci tarafından oluşturulan bir GUID. Adlı sorgu parametresini kullanarak izleme KIMLIĞINI sorgu dizesine eklerseniz, bu üstbilgiyi atlayabilirsiniz <code>ClientTraceId</code> .</td>
  </tr>
</table> 

## <a name="keys-and-endpoints"></a>Anahtarlar ve uç noktalar

Bu sayfadaki örnekler basitlik için sabit kodlanmış anahtarlar ve uç noktalar kullanır. **İşiniz bittiğinde kodu koddan kaldırmayı**unutmayın ve **hiçbir zaman herkese açık bir şekilde nakletmeyin**. Üretim için, kimlik bilgilerinizi depolamak ve bunlara erişmek için güvenli bir yol kullanmayı düşünün. Daha fazla bilgi için bilişsel Hizmetler [güvenlik](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-security) makalesine bakın.

## <a name="translate-text"></a>Metin çevirme 

Çeviri hizmetinin temel işlemi metni çevirmelidir. Bu bölümde, tek bir kaynak ( `from` ) alan ve iki çıkış () sağlayan bir istek oluşturacaksınız `to` . Bu durumda, hem isteği hem de yanıtı ayarlamak için kullanılabilecek bazı parametreleri gözden geçireceğiz. 

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json; // Install Newtonsoft.Json with NuGet

class Program
{
    private static readonly string subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com/";
    
    static async Task Main(string[] args)
    {
        // Input and output languages are defined as parameters.
        string route = "/translate?api-version=3.0&from=en&to=de&to=it";
        string textToTranslate = "Hello, world!";
        object[] body = new object[] { new { Text = textToTranslate } };
        var requestBody = JsonConvert.SerializeObject(body);
    
        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    
            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

# <a name="go"></a>[Git](#tab/go)

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)

func main() {
    subscriptionKey := "YOUR-SUBSCRIPTION-KEY"
    endpoint := "https://api.cognitive.microsofttranslator.com/"
    uri := endpoint + "/translate?api-version=3.0"

    // Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
    u, _ := url.Parse(uri)
    q := u.Query()
    q.Add("from", "en")
    q.Add("to", "de")
    q.Add("to", "it")
    u.RawQuery = q.Encode()

    // Create an anonymous struct for your request body and encode it to JSON
    body := []struct {
        Text string
    }{
        {Text: "Hello, world!"},
    }
    b, _ := json.Marshal(body)

    // Build the HTTP POST request
    req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
    if err != nil {
        log.Fatal(err)
    }
    // Add required headers to the request
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")

    // Call the Translator API
    res, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }

    // Decode the JSON response
    var result interface{}
    if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
        log.Fatal(err)
    }
    // Format and print the response to terminal
    prettyJSON, _ := json.MarshalIndent(result, "", "  ")
    fmt.Printf("%s\n", prettyJSON)
}
```


# <a name="java"></a>[Java](#tab/java)

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;

public class Translate {
    private static String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    HttpUrl url = new HttpUrl.Builder()
        .scheme("https")
        .host("api.cognitive.microsofttranslator.com")
        .addPathSegment("/translate")
        .addQueryParameter("api-version", "3.0")
        .addQueryParameter("from", "en")
        .addQueryParameter("to", "de")
        .addQueryParameter("to", "it")
        .build();

    // Instantiates the OkHttpClient.
    OkHttpClient client = new OkHttpClient();

    // This function performs a POST request.
    public String Post() throws IOException {
        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType,
                "[{\"Text\": \"Hello World!\"}]");
        Request request = new Request.Builder().url(url).post(body)
                .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey).addHeader("Content-type", "application/json")
                .build();
        Response response = client.newCall(request).execute();
        return response.body().string();
    }

    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            Translate translateRequest = new Translate();
            String response = translateRequest.Post();
            System.out.println(prettify(response));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```Javascript
const axios = require('axios').default;
const uuidv4 = require('uuid/v4');

var subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
var endpoint = "https://api.cognitive.microsofttranslator.com";

axios({
    baseURL: endpoint,
    url: '/translate',
    method: 'post',
    headers: {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': uuidv4().toString()
    },
    params: {
        'api-version': '3.0',
        'from': 'en',
        'to': ['de', 'it']
    },
    data: [{
        'text': 'Hello World!'
    }],
    responseType: 'json'
}).then(function(response){
    console.log(JSON.stringify(response.data, null, 4));
})
```

# <a name="python"></a>[Python](#tab/python)
```python
import requests, uuid, json

# Add your subscription key and endpoint
subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "https://api.cognitive.microsofttranslator.com"

path = '/translate'
constructed_url = endpoint + path

params = {
    'api-version': '3.0',
    'from': 'en',
    'to': ['de', 'it']
}
constructed_url = endpoint + path

headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}

# You can pass more than one object in body.
body = [{
    'text': 'Hello World!'
}]

request = requests.post(constructed_url, params=params, headers=headers, json=body)
response = request.json()

print(json.dumps(response, sort_keys=True, ensure_ascii=False, indent=4, separators=(',', ': ')))
```

---

Başarılı bir çağrıdan sonra aşağıdaki yanıtı görmeniz gerekir: 

```JSON
[
    {
        "translations": [
            {
                "text": "Hallo Welt!",
                "to": "de"
            },
            {
                "text": "Salve, mondo!",
                "to": "it"
            }
        ]
    }
]
```

## <a name="detect-language"></a>Dili algılama

Çeviri yapmanız gerektiğini bildiğiniz halde, çevirmen hizmetine gönderilecek metnin dilini bilmiyorsanız, dil algılama işlemini de kullanabilirsiniz. Kaynak metin dilini belirlemenin birden çok yolu vardır. Bu bölümde, `translate` uç noktayı ve uç noktayı kullanarak dil algılamayı kullanmayı öğreneceksiniz `detect` . 

### <a name="detect-source-language-during-translation"></a>Çeviri sırasında kaynak dilini Algıla

`from`Çeviri isteğinize parametresini eklemezseniz, çevirmen hizmeti kaynak metnin dilini algılamaya çalışır. Yanıtta, algılanan dili ( `language` ) ve güvenirlik Puanını ( `score` ) alacaksınız. `score`Ne kadar yakın olursa, `1.0` algılamanın doğru olduğu arttı demektir.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json; // Install Newtonsoft.Json with NuGet

class Program
{
    private static readonly string subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com/";
    
    static async Task Main(string[] args)
    {
        // Output languages are defined as parameters, input language detected.
        string route = "/translate?api-version=3.0&to=de&to=it";
        string textToTranslate = "Hello, world!";
        object[] body = new object[] { new { Text = textToTranslate } };
        var requestBody = JsonConvert.SerializeObject(body);
    
        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    
            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```


# <a name="go"></a>[Git](#tab/go)

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)

func main() {
    subscriptionKey := "YOUR-SUBSCRIPTION-KEY"
    endpoint := "https://api.cognitive.microsofttranslator.com/"
    uri := endpoint + "/translate?api-version=3.0"

    // Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
    u, _ := url.Parse(uri)
    q := u.Query()
    q.Add("to", "de")
    q.Add("to", "it")
    u.RawQuery = q.Encode()

    // Create an anonymous struct for your request body and encode it to JSON
    body := []struct {
        Text string
    }{
        {Text: "Hello, world!"},
    }
    b, _ := json.Marshal(body)

    // Build the HTTP POST request
    req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
    if err != nil {
        log.Fatal(err)
    }
    // Add required headers to the request
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")

    // Call the Translator API
    res, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }

    // Decode the JSON response
    var result interface{}
    if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
        log.Fatal(err)
    }
    // Format and print the response to terminal
    prettyJSON, _ := json.MarshalIndent(result, "", "  ")
    fmt.Printf("%s\n", prettyJSON)
}
```

# <a name="java"></a>[Java](#tab/java)

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;

public class Translate {
    private static String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";


    HttpUrl url = new HttpUrl.Builder()
        .scheme("https")
        .host("api.cognitive.microsofttranslator.com")
        .addPathSegment("/translate")
        .addQueryParameter("api-version", "3.0")
        .addQueryParameter("to", "de")
        .addQueryParameter("to", "it")
        .build();

    // Instantiates the OkHttpClient.
    OkHttpClient client = new OkHttpClient();

    // This function performs a POST request.
    public String Post() throws IOException {
        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType,
                "[{\"Text\": \"Hello World!\"}]");
        Request request = new Request.Builder().url(url).post(body)
                .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey).addHeader("Content-type", "application/json")
                .build();
        Response response = client.newCall(request).execute();
        return response.body().string();
    }

    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            Translate translateRequest = new Translate();
            String response = translateRequest.Post();
            System.out.println(prettify(response));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const axios = require('axios').default;
const uuidv4 = require('uuid/v4');

var subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
var endpoint = "https://api.cognitive.microsofttranslator.com";

axios({
    baseURL: endpoint,
    url: '/translate',
    method: 'post',
    headers: {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': uuidv4().toString()
    },
    params: {
        'api-version': '3.0',
        'from': 'en',
        'to': ['de', 'it']
    },
    data: [{
        'text': 'Hello World!'
    }],
    responseType: 'json'
}).then(function(response){
    console.log(JSON.stringify(response.data, null, 4));
})
```

# <a name="python"></a>[Python](#tab/python)
```python
import requests, uuid, json

# Add your subscription key and endpoint
subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "https://api.cognitive.microsofttranslator.com"

path = '/translate'
constructed_url = endpoint + path

params = {
    'api-version': '3.0',
    'to': ['de', 'it']
}
constructed_url = endpoint + path

headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}

# You can pass more than one object in body.
body = [{
    'text': 'Hello World!'
}]

request = requests.post(constructed_url, params=params, headers=headers, json=body)
response = request.json()

print(json.dumps(response, sort_keys=True, ensure_ascii=False, indent=4, separators=(',', ': ')))
```

---

Başarılı bir çağrıdan sonra aşağıdaki yanıtı görmeniz gerekir: 

```json
[
    {
        "detectedLanguage": {
            "language": "en",
            "score": 1.0
        },
        "translations": [
            {
                "text": "Hallo Welt!",
                "to": "de"
            },
            {
                "text": "Salve, mondo!",
                "to": "it"
            }
        ]
    }
]
```

### <a name="detect-source-language-without-translation"></a>Çeviri olmadan kaynak dilini Algıla

Çeviri yapmadan kaynak metnin dilini algılamak için Translator hizmetini kullanmak mümkündür. Bunu yapmak için [`/detect`](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-detect) uç noktasını kullanacaksınız. 

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json; // Install Newtonsoft.Json with NuGet

class Program
{
    private static readonly string subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com/";
    
    static async Task Main(string[] args)
    {
        // Just detect language
        string route = "/detect?api-version=3.0";
        string textToLangDetect = "Ich würde wirklich gern Ihr Auto um den Block fahren ein paar Mal.";
        object[] body = new object[] { new { Text = textToLangDetect } };
        var requestBody = JsonConvert.SerializeObject(body);
    
        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    
            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

# <a name="go"></a>[Git](#tab/go)

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)

func main() {
    subscriptionKey := "YOUR-SUBSCRIPTION-KEY"
    endpoint := "https://api.cognitive.microsofttranslator.com/"
    uri := endpoint + "/detect?api-version=3.0"

    // Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
    u, _ := url.Parse(uri)
    q := u.Query()
    u.RawQuery = q.Encode()

    // Create an anonymous struct for your request body and encode it to JSON
    body := []struct {
        Text string
    }{
        {Text: "Ich würde wirklich gern Ihr Auto um den Block fahren ein paar Mal."},
    }
    b, _ := json.Marshal(body)

    // Build the HTTP POST request
    req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
    if err != nil {
        log.Fatal(err)
    }
    // Add required headers to the request
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")

    // Call the Translator API
    res, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }

    // Decode the JSON response
    var result interface{}
    if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
         log.Fatal(err)
    }
    // Format and print the response to terminal
    prettyJSON, _ := json.MarshalIndent(result, "", "  ")
    fmt.Printf("%s\n", prettyJSON)
}
```

# <a name="java"></a>[Java](#tab/java)

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;

public class Detect {
    private static String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    HttpUrl url = new HttpUrl.Builder()
        .scheme("https")
        .host("api.cognitive.microsofttranslator.com")
        .addPathSegment("/detect")
        .addQueryParameter("api-version", "3.0")
        .build();

    // Instantiates the OkHttpClient.
    OkHttpClient client = new OkHttpClient();

    // This function performs a POST request.
    public String Post() throws IOException {
        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType,
                "[{\"Text\": \"Ich würde wirklich gern Ihr Auto um den Block fahren ein paar Mal.\"}]");
        Request request = new Request.Builder().url(url).post(body)
                .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey).addHeader("Content-type", "application/json")
                .build();
        Response response = client.newCall(request).execute();
        return response.body().string();
    }

    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            Detect detectRequest = new Detect();
            String response = detectRequest.Post();
            System.out.println(prettify(response));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const axios = require('axios').default;
const uuidv4 = require('uuid/v4');

var subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
var endpoint = "https://api.cognitive.microsofttranslator.com";

axios({
    baseURL: endpoint,
    url: '/detect',
    method: 'post',
    headers: {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': uuidv4().toString()
    },
    params: {
        'api-version': '3.0'
    },
    data: [{
        'text': 'Ich würde wirklich gern Ihr Auto um den Block fahren ein paar Mal.'
    }],
    responseType: 'json'
}).then(function(response){
    console.log(JSON.stringify(response.data, null, 4));
})
```

# <a name="python"></a>[Python](#tab/python)
```python
import requests, uuid, json

# Add your subscription key and endpoint
subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "https://api.cognitive.microsofttranslator.com"

path = '/detect'
constructed_url = endpoint + path

params = {
    'api-version': '3.0'
}
constructed_url = endpoint + path

headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}

# You can pass more than one object in body.
body = [{
    'text': 'Ich würde wirklich gern Ihr Auto um den Block fahren ein paar Mal.'
}]

request = requests.post(constructed_url, params=params, headers=headers, json=body)
response = request.json()

print(json.dumps(response, sort_keys=True, ensure_ascii=False, indent=4, separators=(',', ': ')))
```
---

`/detect`Uç nokta kullanılırken, yanıt alternatif algılamaları içerir ve tüm algılanan dillerin çeviri ve alfabede desteklenip desteklenmediğini öğrenmenizi sağlar. Başarılı bir çağrıdan sonra aşağıdaki yanıtı görmeniz gerekir: 

```json
[
    {
        "alternatives": [
            {
                "isTranslationSupported": true,
                "isTransliterationSupported": false,
                "language": "nl",
                "score": 0.92
            },
            {
                "isTranslationSupported": true,
                "isTransliterationSupported": false,
                "language": "sk",
                "score": 0.77
            }
        ],
        "isTranslationSupported": true,
        "isTransliterationSupported": false,
        "language": "de",
        "score": 1.0
    }
]
```

## <a name="transliterate-text"></a>Metni başka dildi yazma 

Alfabede, bir kelime veya tümceciği fonetik benzerliğine göre bir dilin betiğine (alfabe) dönüştürme işlemidir. Örneğin, "สวัสดี" ( `thai` ) öğesini "sawatdi" () dönüştürmek için alfabeyi kullanabilirsiniz `latn` . Tek bir alfabede gerçekleştirmek için birden çok yol vardır. Bu bölümde, `translate` uç noktayı ve uç noktayı kullanarak dil algılamayı kullanmayı öğreneceksiniz `transliterate` . 

### <a name="transliterate-during-translation"></a>Çeviri sırasında alfabede olacak şekilde

Kaynağınıza göre farklı bir alfabeyi (veya phonemes) kullanan bir dilde çeviri yapıyorsanız, bir alfabeye ihtiyacınız olabilir. Bu örnekte, "Merhaba" öğesini Ingilizce 'den Tayland 'a çeviriyoruz. Tay dilinde çeviriyi almanın yanı sıra Latin alfabesini kullanarak çevrilmiş tümceciğin bir alfabeyi elde edersiniz.

Uç noktadan bir alfabe alfabmına ulaşmak için `translate` `toScript` parametresini kullanın.

> ! NOTUN Kullanılabilir dillerin ve alfabedeki seçeneklerin tamamı listesi için bkz. [dil desteği](language-support.md).

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json; // Install Newtonsoft.Json with NuGet

class Program
{
    private static readonly string subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com/";
    
    static async Task Main(string[] args)
    {
        // Output language defined as parameter, with toScript set to latn
        string route = "/translate?api-version=3.0&to=th&toScript=latn";
        string textToTransliterate = "Hello";
        object[] body = new object[] { new { Text = textToTransliterate } };
        var requestBody = JsonConvert.SerializeObject(body);
    
        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    
            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

# <a name="go"></a>[Git](#tab/go)

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)

func main() {
    subscriptionKey := "YOUR-SUBSCRIPTION-KEY"
    endpoint := "https://api.cognitive.microsofttranslator.com/"
    uri := endpoint + "/translate?api-version=3.0"

    // Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
    u, _ := url.Parse(uri)
    q := u.Query()
    q.Add("to", "th")
    q.Add("toScript", "latn")
    u.RawQuery = q.Encode()

    // Create an anonymous struct for your request body and encode it to JSON
    body := []struct {
        Text string
    }{
        {Text: "Hello"},
    }
    b, _ := json.Marshal(body)

    // Build the HTTP POST request
    req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
    if err != nil {
        log.Fatal(err)
    }
    // Add required headers to the request
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")

    // Call the Translator API
    res, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }

    // Decode the JSON response
    var result interface{}
    if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
        log.Fatal(err)
    }
    // Format and print the response to terminal
    prettyJSON, _ := json.MarshalIndent(result, "", "  ")
    fmt.Printf("%s\n", prettyJSON)
}
```

# <a name="java"></a>[Java](#tab/java)

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;

public class Translate {
    private static String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    HttpUrl url = new HttpUrl.Builder()
        .scheme("https")
        .host("api.cognitive.microsofttranslator.com")
        .addPathSegment("/translate")
        .addQueryParameter("api-version", "3.0")
        .addQueryParameter("to", "th")
        .addQueryParameter("toScript", "latn")
        .build();

    // Instantiates the OkHttpClient.
    OkHttpClient client = new OkHttpClient();

    // This function performs a POST request.
    public String Post() throws IOException {
        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType,
                "[{\"Text\": \"Hello\"}]");
        Request request = new Request.Builder().url(url).post(body)
                .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey).addHeader("Content-type", "application/json")
                .build();
        Response response = client.newCall(request).execute();
        return response.body().string();
    }

    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            Translate translateRequest = new Translate();
            String response = translateRequest.Post();
            System.out.println(prettify(response));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const axios = require('axios').default;
const uuidv4 = require('uuid/v4');

var subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
var endpoint = "https://api.cognitive.microsofttranslator.com";

axios({
    baseURL: endpoint,
    url: '/translate',
    method: 'post',
    headers: {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': uuidv4().toString()
    },
    params: {
        'api-version': '3.0',
        'to': 'th',
        'toScript': 'latn'
    },
    data: [{
        'text': 'Hello'
    }],
    responseType: 'json'
}).then(function(response){
    console.log(JSON.stringify(response.data, null, 4));
})
```

# <a name="python"></a>[Python](#tab/python)
```Python
import requests, uuid, json

# Add your subscription key and endpoint
subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "https://api.cognitive.microsofttranslator.com"

path = '/translate'
constructed_url = endpoint + path

params = {
    'api-version': '3.0',
    'to': 'th',
    'toScript': 'latn'
}
constructed_url = endpoint + path

headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}

# You can pass more than one object in body.
body = [{
    'text': 'Hello'
}]
request = requests.post(constructed_url, params=params, headers=headers, json=body)
response = request.json()

print(json.dumps(response, sort_keys=True, ensure_ascii=False, indent=4, separators=(',', ': ')))
```

---

Başarılı bir çağrıdan sonra aşağıdaki yanıtı görmeniz gerekir. Bitiş noktasındaki yanıtın, bir `translate` Güvenirlik puanı ile algılanan kaynak dilini, çıkış dilinin alfabesini kullanan bir çeviriyi ve Latin alfabesini kullanarak bir alfabeyi kullandığını unutmayın.

```json
[
    {
        "detectedLanguage": {
            "language": "en",
            "score": 1.0
        },
        "translations": [
            {
                "text": "สวัสดี",
                "to": "th",
                "transliteration": {
                    "script": "Latn",
                    "text": "sawatdi"
                }
            }
        ]
    }
]
```

### <a name="transliterate-without-translation"></a>Çeviri olmadan alfabeye dönüştürme

`transliterate`Bir alfabeye ulaşmak için uç noktasını da kullanabilirsiniz. Alfabe bitiş noktasını kullanırken, kaynak dilini ( `language` ), kaynak betiği/ `fromScript` alfabeyi () ve çıkış betiğini/alfabeyi ( `toScript` ) parametre olarak sağlamanız gerekir. Bu örnekte, สวัสดี için alfabeyi almaya devam ediyoruz. 

> ! NOTUN Kullanılabilir dillerin ve alfabedeki seçeneklerin tamamı listesi için bkz. [dil desteği](language-support.md).

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json; // Install Newtonsoft.Json with NuGet

class Program
{
    private static readonly string subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com/";
    
    static async Task Main(string[] args)
    {
        // For a complete list of options, see API reference.
        // Input and output languages are defined as parameters.
        string route = "/translate?api-version=3.0&to=th&toScript=latn";
        string textToTransliterate = "Hello";
        object[] body = new object[] { new { Text = textToTransliterate } };
        var requestBody = JsonConvert.SerializeObject(body);
    
        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    
            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

# <a name="go"></a>[Git](#tab/go)

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)

func main() {
    subscriptionKey := "YOUR-SUBSCRIPTION-KEY"
    endpoint := "https://api.cognitive.microsofttranslator.com/"
    uri := endpoint + "/transliterate?api-version=3.0"

    // Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
    u, _ := url.Parse(uri)
    q := u.Query()
    q.Add("language", "th")
    q.Add("fromScript", "thai")
    q.Add("toScript", "latn")
    u.RawQuery = q.Encode()

    // Create an anonymous struct for your request body and encode it to JSON
    body := []struct {
        Text string
    }{
        {Text: "สวัสดี"},
    }
    b, _ := json.Marshal(body)

    // Build the HTTP POST request
    req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
    if err != nil {
        log.Fatal(err)
    }
    // Add required headers to the request
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")

    // Call the Translator API
    res, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }

    // Decode the JSON response
    var result interface{}
    if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
        log.Fatal(err)
    }
    // Format and print the response to terminal
    prettyJSON, _ := json.MarshalIndent(result, "", "  ")
    fmt.Printf("%s\n", prettyJSON)
}
```

# <a name="java"></a>[Java](#tab/java)

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;

public class Transliterate {
    private static String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    HttpUrl url = new HttpUrl.Builder()
        .scheme("https")
        .host("api.cognitive.microsofttranslator.com")
        .addPathSegment("/transliterate")
            .addQueryParameter("api-version", "3.0")
            .addQueryParameter("language", "th")
            .addQueryParameter("fromScript", "thai")
            .addQueryParameter("toScript", "latn")
        .build();

    // Instantiates the OkHttpClient.
    OkHttpClient client = new OkHttpClient();

    // This function performs a POST request.
    public String Post() throws IOException {
        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType,
                "[{\"Text\": \"สวัสดี\"}]");
        Request request = new Request.Builder().url(url).post(body)
                .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey).addHeader("Content-type", "application/json")
                .build();
        Response response = client.newCall(request).execute();
        return response.body().string();
    }

    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            Transliterate transliterateRequest = new Transliterate();
            String response = transliterateRequest.Post();
            System.out.println(prettify(response));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const axios = require('axios').default;
const uuidv4 = require('uuid/v4');

var subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
var endpoint = "https://api.cognitive.microsofttranslator.com";

axios({
    baseURL: endpoint,
    url: '/transliterate',
    method: 'post',
    headers: {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': uuidv4().toString()
    },
    params: {
        'api-version': '3.0',
        'language': 'th',
        'fromScript': 'thai',
        'toScript': 'latn'
    },
    data: [{
        'text': 'สวัสดี'
    }],
    responseType: 'json'
}).then(function(response){
    console.log(JSON.stringify(response.data, null, 4));
})
```

# <a name="python"></a>[Python](#tab/python)
```python
import requests, uuid, json

# Add your subscription key and endpoint
subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "https://api.cognitive.microsofttranslator.com"

path = '/transliterate'
constructed_url = endpoint + path

params = {
    'api-version': '3.0',
    'language': 'th',
    'fromScript': 'thai',
    'toScript': 'latn'
}
constructed_url = endpoint + path

headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}

# You can pass more than one object in body.
body = [{
    'text': 'สวัสดี'
}]

request = requests.post(constructed_url, params=params, headers=headers, json=body)
response = request.json()

print(json.dumps(response, sort_keys=True, indent=4, separators=(',', ': ')))
```

---

Başarılı bir çağrıdan sonra aşağıdaki yanıtı görmeniz gerekir. `translate`Uç nokta çağrısının aksine, `transliterate` yalnızca `script` ve çıktısını döndürür `text` . 

```json
[
    {
        "script": "latn",
        "text": "sawatdi"
    }
]
```

## <a name="get-sentence-length"></a>Cümle uzunluğu al

Translator hizmeti ile bir cümle veya tümce dizisinin karakter sayısını alabilirsiniz. Yanıt, algılanan her tümce için karakter sayısı ile bir dizi olarak döndürülür. `translate`Ve uç noktaları ile tümce uzunluklarına ulaşabilirsiniz `breaksentence` .

### <a name="get-sentence-length-during-translation"></a>Çeviri sırasında cümle uzunluğu al

Uç noktasını kullanarak hem kaynak metin hem de çeviri çıkışı için karakter sayıları alabilirsiniz `translate` . Tümce uzunluğunu ( `srcSenLen` ve) döndürmek için `transSenLen` parametresini olarak ayarlamanız gerekir `includeSentenceLength` `True` .

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json; // Install Newtonsoft.Json with NuGet

class Program
{
    private static readonly string subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com/";
    
    static async Task Main(string[] args)
    {
        // Include sentence length details.
        string route = "/translate?api-version=3.0&to=es&includeSentenceLength=true";
        string sentencesToCount = 
                "Can you tell me how to get to Penn Station? Oh, you aren't sure? That's fine.";
        object[] body = new object[] { new { Text = sentencesToCount } };
        var requestBody = JsonConvert.SerializeObject(body);
    
        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    
            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

# <a name="go"></a>[Git](#tab/go)

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)

func main() {
    subscriptionKey := "YOUR-SUBSCRIPTION-KEY"
    endpoint := "https://api.cognitive.microsofttranslator.com/"
    uri := endpoint + "/translate?api-version=3.0"

    // Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
    u, _ := url.Parse(uri)
    q := u.Query()
    q.Add("to", "es")
    q.Add("includeSentenceLength", "true")
    u.RawQuery = q.Encode()

    // Create an anonymous struct for your request body and encode it to JSON
    body := []struct {
        Text string
    }{
        {Text: "Can you tell me how to get to Penn Station? Oh, you aren't sure? That's fine."},
    }
    b, _ := json.Marshal(body)

    // Build the HTTP POST request
    req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
    if err != nil {
        log.Fatal(err)
    }
    // Add required headers to the request
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")

    // Call the Translator API
    res, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }

    // Decode the JSON response
    var result interface{}
    if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
        log.Fatal(err)
    }
    // Format and print the response to terminal
    prettyJSON, _ := json.MarshalIndent(result, "", "  ")
    fmt.Printf("%s\n", prettyJSON)
}
```

# <a name="java"></a>[Java](#tab/java)

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;

public class Translate {
    private static String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    HttpUrl url = new HttpUrl.Builder()
        .scheme("https")
        .host("api.cognitive.microsofttranslator.com")
        .addPathSegment("/translate")
        .addQueryParameter("api-version", "3.0")
        .addQueryParameter("to", "es")
        .addQueryParameter("includeSentenceLength", "true")
        .build();

    // Instantiates the OkHttpClient.
    OkHttpClient client = new OkHttpClient();

    // This function performs a POST request.
    public String Post() throws IOException {
        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType,
                "[{\"Text\": \"Can you tell me how to get to Penn Station? Oh, you aren\'t sure? That\'s fine.\"}]");
        Request request = new Request.Builder().url(url).post(body)
                .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey).addHeader("Content-type", "application/json")
                .build();
        Response response = client.newCall(request).execute();
        return response.body().string();
    }

    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            Translate translateRequest = new Translate();
            String response = translateRequest.Post();
            System.out.println(prettify(response));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const axios = require('axios').default;
const uuidv4 = require('uuid/v4');

var subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
var endpoint = "https://api.cognitive.microsofttranslator.com";

axios({
    baseURL: endpoint,
    url: '/translate',
    method: 'post',
    headers: {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': uuidv4().toString()
    },
    params: {
        'api-version': '3.0',
        'to': 'es',
        'includeSentenceLength': true
    },
    data: [{
        'text': 'Can you tell me how to get to Penn Station? Oh, you aren\'t sure? That\'s fine.'
    }],
    responseType: 'json'
}).then(function(response){
    console.log(JSON.stringify(response.data, null, 4));
})
```

# <a name="python"></a>[Python](#tab/python)
```python
import requests, uuid, json

# Add your subscription key and endpoint
subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "https://api.cognitive.microsofttranslator.com"

path = '/translate'
constructed_url = endpoint + path

params = {
    'api-version': '3.0',
    'to': 'es',
    'includeSentenceLength': True
}
constructed_url = endpoint + path

headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}

# You can pass more than one object in body.
body = [{
    'text': 'Can you tell me how to get to Penn Station? Oh, you aren\'t sure? That\'s fine.'
}]
request = requests.post(constructed_url, params=params, headers=headers, json=body)
response = request.json()

print(json.dumps(response, sort_keys=True, ensure_ascii=False, indent=4, separators=(',', ': ')))
```

---

Başarılı bir çağrıdan sonra aşağıdaki yanıtı görmeniz gerekir. Algılanan kaynak diline ve çevirisine ek olarak, hem kaynak ( `srcSentLen` ) hem de çeviri () için algılanan her cümle için karakter sayılarını alacaksınız `transSentLen` .

```json
[
    {
        "detectedLanguage": {
            "language": "en",
            "score": 1.0
        },
        "translations": [
            {
                "sentLen": {
                    "srcSentLen": [
                        44,
                        21,
                        12
                    ],
                    "transSentLen": [
                        48,
                        18,
                        10
                    ]
                },
                "text": "¿Puedes decirme cómo llegar a la estación Penn? ¿No estás seguro? Está bien.",
                "to": "es"
            }
        ]
    }
]
```

### <a name="get-sentence-length-without-translation"></a>Çeviri olmadan cümle uzunluğu al

Translator hizmeti, uç noktayı kullanarak hiçbir çeviri olmadan cümle uzunluğu isteyebilmenizi de sağlar `breaksentence` . 

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json; // Install Newtonsoft.Json with NuGet

class Program
{
    private static readonly string subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com/";
    
    static async Task Main(string[] args)
    {
        // Only include sentence length details.
        string route = "/breaksentence?api-version=3.0";
        string sentencesToCount = 
                "Can you tell me how to get to Penn Station? Oh, you aren't sure? That's fine.";
        object[] body = new object[] { new { Text = sentencesToCount } };
        var requestBody = JsonConvert.SerializeObject(body);
    
        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    
            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

# <a name="go"></a>[Git](#tab/go)

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)

func main() {
    subscriptionKey := "YOUR-SUBSCRIPTION-KEY"
    endpoint := "https://api.cognitive.microsofttranslator.com/"
    uri := endpoint + "/breaksentence?api-version=3.0"

    // Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
    u, _ := url.Parse(uri)
    q := u.Query()
    u.RawQuery = q.Encode()

    // Create an anonymous struct for your request body and encode it to JSON
    body := []struct {
        Text string
    }{
        {Text: "Can you tell me how to get to Penn Station? Oh, you aren't sure? That's fine."},
    }
    b, _ := json.Marshal(body)

    // Build the HTTP POST request
    req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
    if err != nil {
        log.Fatal(err)
    }
    // Add required headers to the request
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")

    // Call the Translator API
    res, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }

    // Decode the JSON response
    var result interface{}
    if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
        log.Fatal(err)
    }
    // Format and print the response to terminal
    prettyJSON, _ := json.MarshalIndent(result, "", "  ")
    fmt.Printf("%s\n", prettyJSON)
}
```

# <a name="java"></a>[Java](#tab/java)

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;

public class BreakSentence {
    private static String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    HttpUrl url = new HttpUrl.Builder()
        .scheme("https")
        .host("api.cognitive.microsofttranslator.com")
        .addPathSegment("/breaksentence")
        .addQueryParameter("api-version", "3.0")
        .build();

    // Instantiates the OkHttpClient.
    OkHttpClient client = new OkHttpClient();

    // This function performs a POST request.
    public String Post() throws IOException {
        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType,
                "[{\"Text\": \"Can you tell me how to get to Penn Station? Oh, you aren\'t sure? That\'s fine.\"}]");
        Request request = new Request.Builder().url(url).post(body)
                .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey).addHeader("Content-type", "application/json")
                .build();
        Response response = client.newCall(request).execute();
        return response.body().string();
    }

    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            BreakSentence breakSentenceRequest = new BreakSentence();
            String response = breakSentenceRequest.Post();
            System.out.println(prettify(response));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const axios = require('axios').default;
const uuidv4 = require('uuid/v4');

var subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
var endpoint = "https://api.cognitive.microsofttranslator.com";

axios({
    baseURL: endpoint,
    url: '/breaksentence',
    method: 'post',
    headers: {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': uuidv4().toString()
    },
    params: {
        'api-version': '3.0'
    },
    data: [{
        'text': 'Can you tell me how to get to Penn Station? Oh, you aren\'t sure? That\'s fine.'
    }],
    responseType: 'json'
}).then(function(response){
    console.log(JSON.stringify(response.data, null, 4));
})
```

# <a name="python"></a>[Python](#tab/python)
```python
import requests, uuid, json

# Add your subscription key and endpoint
subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "https://api.cognitive.microsofttranslator.com"

path = '/breaksentence'
constructed_url = endpoint + path

params = {
    'api-version': '3.0'
}
constructed_url = endpoint + path

headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}

# You can pass more than one object in body.
body = [{
    'text': 'Can you tell me how to get to Penn Station? Oh, you aren\'t sure? That\'s fine.'
}]

request = requests.post(constructed_url, params=params, headers=headers, json=body)
response = request.json()

print(json.dumps(response, sort_keys=True, indent=4, separators=(',', ': ')))
```

---

Başarılı bir çağrıdan sonra aşağıdaki yanıtı görmeniz gerekir. `translate`Uç nokta çağrısının aksine, `breaksentence` yalnızca adlı bir dizideki kaynak metnin karakter sayısını döndürür `sentLen` . 

```json
[
    {
        "detectedLanguage": {
            "language": "en",
            "score": 1.0
        },
        "sentLen": [
            44,
            21,
            12
        ]
    }
]
```

## <a name="dictionary-lookup-alternate-translations"></a>Sözlük arama (alternatif çeviriler)

Uç nokta ile bir sözcük veya tümcecik için alternatif çeviriler alabilirsiniz. Örneğin, "Shark" sözcüğünün ' den ' a çevrilirken `en` `es` , bu uç nokta hem "Tiburón" hem de "escualo" döndürür.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json; // Install Newtonsoft.Json with NuGet

class Program
{
    private static readonly string subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com/";
    
    static async Task Main(string[] args)
    {
        // See many translation options
        string route = "/dictionary/lookup?api-version=3.0&from=en&to=es";
        string wordToTranslate = "shark";
        object[] body = new object[] { new { Text = wordToTranslate } };
        var requestBody = JsonConvert.SerializeObject(body);
    
        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    
            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

# <a name="go"></a>[Git](#tab/go)

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)

func main() {
    subscriptionKey := "YOUR-SUBSCRIPTION-KEY"
    endpoint := "https://api.cognitive.microsofttranslator.com/"
    uri := endpoint + "/dictionary/lookup?api-version=3.0"

    // Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
    u, _ := url.Parse(uri)
    q := u.Query()
    q.Add("from", "en")
    q.Add("to", "es")
    u.RawQuery = q.Encode()

    // Create an anonymous struct for your request body and encode it to JSON
    body := []struct {
        Text string
    }{
        {Text: "shark"},
    }
    b, _ := json.Marshal(body)

    // Build the HTTP POST request
    req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
    if err != nil {
        log.Fatal(err)
    }
    // Add required headers to the request
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")

    // Call the Translator API
    res, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }

    // Decode the JSON response
    var result interface{}
    if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
        log.Fatal(err)
    }
    // Format and print the response to terminal
    prettyJSON, _ := json.MarshalIndent(result, "", "  ")
    fmt.Printf("%s\n", prettyJSON)
}
```

# <a name="java"></a>[Java](#tab/java)

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;

public class DictionaryLookup {
    private static String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    HttpUrl url = new HttpUrl.Builder()
        .scheme("https")
        .host("api.cognitive.microsofttranslator.com")
        .addPathSegment("/dictionary/lookup")
        .addQueryParameter("api-version", "3.0")
        .addQueryParameter("from", "en")
        .addQueryParameter("to", "es")
        .build();

    // Instantiates the OkHttpClient.
    OkHttpClient client = new OkHttpClient();

    // This function performs a POST request.
    public String Post() throws IOException {
        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType,
                "[{\"Text\": \"Shark\"}]");
        Request request = new Request.Builder().url(url).post(body)
                .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey).addHeader("Content-type", "application/json")
                .build();
        Response response = client.newCall(request).execute();
        return response.body().string();
    }

    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            DictionaryLookup dictionaryLookupRequest = new DictionaryLookup();
            String response = dictionaryLookupRequest.Post();
            System.out.println(prettify(response));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const axios = require('axios').default;
const uuidv4 = require('uuid/v4');

var subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
var endpoint = "https://api.cognitive.microsofttranslator.com";

axios({
    baseURL: endpoint,
    url: '/dictionary/lookup',
    method: 'post',
    headers: {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': uuidv4().toString()
    },
    params: {
        'api-version': '3.0',
        'from': 'en',
        'to': 'es'
    },
    data: [{
        'text': 'shark'
    }],
    responseType: 'json'
}).then(function(response){
    console.log(JSON.stringify(response.data, null, 4));
})
```

# <a name="python"></a>[Python](#tab/python)
```python
import requests, uuid, json

# Add your subscription key and endpoint
subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "https://api.cognitive.microsofttranslator.com"

path = '/dictionary/lookup'
constructed_url = endpoint + path

params = {
    'api-version': '3.0',
    'from': 'en',
    'to': 'es'
}
constructed_url = endpoint + path

headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}

# You can pass more than one object in body.
body = [{
    'text': 'shark'
}]
request = requests.post(constructed_url, params=params, headers=headers, json=body)
response = request.json()

print(json.dumps(response, sort_keys=True, ensure_ascii=False, indent=4, separators=(',', ': ')))
```

---

Başarılı bir çağrıdan sonra aşağıdaki yanıtı görmeniz gerekir. JSON, bu makaledeki diğer örneklerden bazılarının daha karmaşıktır. `translations`Dizi, çevirilerin bir listesini içerir. Bu dizideki her bir nesne Güvenirlik puanı ( `confidence` ), Son Kullanıcı görüntüsü () için iyileştirilmiş metin, `displayTarget` normalleştirilmiş metin (), `normalizedText` konuşma bölümü () `posTag` ve önceki çeviri () hakkındaki bilgiler içerir `backTranslations` . Yanıt hakkında daha fazla bilgi için bkz. [Sözlük Arama](reference/v3-0-dictionary-lookup.md)

```json
[
    {
        "displaySource": "shark",
        "normalizedSource": "shark",
        "translations": [
            {
                "backTranslations": [
                    {
                        "displayText": "shark",
                        "frequencyCount": 45,
                        "normalizedText": "shark",
                        "numExamples": 0
                    }
                ],
                "confidence": 0.8182,
                "displayTarget": "tiburón",
                "normalizedTarget": "tiburón",
                "posTag": "OTHER",
                "prefixWord": ""
            },
            {
                "backTranslations": [
                    {
                        "displayText": "shark",
                        "frequencyCount": 10,
                        "normalizedText": "shark",
                        "numExamples": 1
                    }
                ],
                "confidence": 0.1818,
                "displayTarget": "escualo",
                "normalizedTarget": "escualo",
                "posTag": "NOUN",
                "prefixWord": ""
            }
        ]
    }
]
```

## <a name="dictionary-examples-translations-in-context"></a>Sözlük örnekleri (bağlamdaki Çeviriler)

Sözlük aramasını gerçekleştirdikten sonra, `dictionary/examples` her iki terimi tümce veya tümcecik bağlamında gösteren örneklerin listesini almak için kaynak metni ve çeviriyi uç noktaya geçirebilirsiniz. Önceki örnekte derleme yaparken, `normalizedText` ve `normalizedTarget` Sözlük Arama yanıtından ve sırasıyla öğesini kullanacaksınız `text` `translation` . Kaynak dili ( `from` ) ve çıkış hedefi ( `to` ) parametreleri gereklidir. 

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Newtonsoft.Json; // Install Newtonsoft.Json with NuGet

class Program
{
    private static readonly string subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
    private static readonly string endpoint = "https://api.cognitive.microsofttranslator.com/";
    
    static async Task Main(string[] args)
    {
        // See examples of terms in context
        string route = "/dictionary/examples?api-version=3.0&from=en&to=es";
        object[] body = new object[] { new { Text = "Shark",  Translation = "tiburón" } } ;
        var requestBody = JsonConvert.SerializeObject(body);
    
        using (var client = new HttpClient())
        using (var request = new HttpRequestMessage())
        {
            // Build the request.
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(endpoint + route);
            request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
            request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
    
            // Send the request and get response.
            HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
            // Read response as a string.
            string result = await response.Content.ReadAsStringAsync();
            Console.WriteLine(result);
        }
    }
}
```

# <a name="go"></a>[Git](#tab/go)

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "net/url"
)

func main() {
    subscriptionKey := "151526a0d75d472fa4aef87aa4cf3bd9"
    endpoint := "https://api.cognitive.microsofttranslator.com/"
    uri := endpoint + "/dictionary/lookup?api-version=3.0"

    // Build the request URL. See: https://golang.org/pkg/net/url/#example_URL_Parse
    u, _ := url.Parse(uri)
    q := u.Query()
    q.Add("from", "en")
    q.Add("to", "es")
    u.RawQuery = q.Encode()

    // Create an anonymous struct for your request body and encode it to JSON
    body := []struct {
        Text string
        Translation string
    }{
        {
          Text: "How are you? I am fine. What did you do today?",
          Translation: "¿Cómo estás? Estoy bien. ¿Qué hiciste hoy?",
        },
    }
    b, _ := json.Marshal(body)

    // Build the HTTP POST request
    req, err := http.NewRequest("POST", u.String(), bytes.NewBuffer(b))
    if err != nil {
        log.Fatal(err)
    }
    // Add required headers to the request
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)
    req.Header.Add("Content-Type", "application/json")

    // Call the Translator Text API
    res, err := http.DefaultClient.Do(req)
    if err != nil {
        log.Fatal(err)
    }

    // Decode the JSON response
    var result interface{}
    if err := json.NewDecoder(res.Body).Decode(&result); err != nil {
        log.Fatal(err)
    }
    // Format and print the response to terminal
    prettyJSON, _ := json.MarshalIndent(result, "", "  ")
    fmt.Printf("%s\n", prettyJSON)
}
```

# <a name="java"></a>[Java](#tab/java)

```java
import java.io.*;
import java.net.*;
import java.util.*;
import com.google.gson.*;
import com.squareup.okhttp.*;

public class DictionaryExamples {
    private static String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    HttpUrl url = new HttpUrl.Builder()
        .scheme("https")
        .host("api.cognitive.microsofttranslator.com")
        .addPathSegment("/dictionary/examples")
        .addQueryParameter("api-version", "3.0")
        .addQueryParameter("from", "en")
        .addQueryParameter("to", "es")
        .build();

    // Instantiates the OkHttpClient.
    OkHttpClient client = new OkHttpClient();

    // This function performs a POST request.
    public String Post() throws IOException {
        MediaType mediaType = MediaType.parse("application/json");
        RequestBody body = RequestBody.create(mediaType,
                "[{\"Text\": \"Shark\", \"Translation\": \"tiburón\"}]");
        Request request = new Request.Builder().url(url).post(body)
                .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey).addHeader("Content-type", "application/json")
                .build();
        Response response = client.newCall(request).execute();
        return response.body().string();
    }

    // This function prettifies the json response.
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            DictionaryExamples dictionaryExamplesRequest = new DictionaryExamples();
            String response = dictionaryExamplesRequest.Post();
            System.out.println(prettify(response));
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

# <a name="nodejs"></a>[Node.js](#tab/nodejs)

```javascript
const axios = require('axios').default;
const uuidv4 = require('uuid/v4');

var subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
var endpoint = "https://api.cognitive.microsofttranslator.com";

axios({
    baseURL: endpoint,
    url: '/dictionary/examples',
    method: 'post',
    headers: {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-type': 'application/json',
        'X-ClientTraceId': uuidv4().toString()
    },
    params: {
        'api-version': '3.0',
        'from': 'en',
        'to': 'es'
    },
    data: [{
        'text': 'shark',
        'translation': 'tiburón'
    }],
    responseType: 'json'
}).then(function(response){
    console.log(JSON.stringify(response.data, null, 4));
})
```

# <a name="python"></a>[Python](#tab/python)
```python
import requests, uuid, json

# Add your subscription key and endpoint
subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "https://api.cognitive.microsofttranslator.com"

path = '/dictionary/examples'
constructed_url = endpoint + path

params = {
    'api-version': '3.0',
    'from': 'en',
    'to': 'es'
}
constructed_url = endpoint + path

headers = {
    'Ocp-Apim-Subscription-Key': subscription_key,
    'Content-type': 'application/json',
    'X-ClientTraceId': str(uuid.uuid4())
}

# You can pass more than one object in body.
body = [{
    'text': 'shark',
    'translation': 'tiburón'
}]

request = requests.post(constructed_url, params=params, headers=headers, json=body)
response = request.json()

print(json.dumps(response, sort_keys=True, ensure_ascii=False, indent=4, separators=(',', ': ')))
```

---

Başarılı bir çağrıdan sonra aşağıdaki yanıtı görmeniz gerekir. Yanıt hakkında daha fazla bilgi için bkz. [Sözlük Arama](reference/v3-0-dictionary-examples.md)

```json
[
    {
        "examples": [
            {
                "sourcePrefix": "More than a match for any ",
                "sourceSuffix": ".",
                "sourceTerm": "shark",
                "targetPrefix": "Más que un fósforo para cualquier ",
                "targetSuffix": ".",
                "targetTerm": "tiburón"
            },
            {
                "sourcePrefix": "Same with the mega ",
                "sourceSuffix": ", of course.",
                "sourceTerm": "shark",
                "targetPrefix": "Y con el mega ",
                "targetSuffix": ", por supuesto.",
                "targetTerm": "tiburón"
            },
            {
                "sourcePrefix": "A ",
                "sourceSuffix": " ate it.",
                "sourceTerm": "shark",
                "targetPrefix": "Te la ha comido un ",
                "targetSuffix": ".",
                "targetTerm": "tiburón"
            }
        ],
        "normalizedSource": "shark",
        "normalizedTarget": "tiburón"
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* [API 'nin karakterleri nasıl saydığını öğrenin](character-counts.md)
* [Çeviriyi özelleştirme ve geliştirme](customization.md)

## <a name="see-also"></a>Ayrıca bkz.

* [Translator v3 API başvurusu](reference/v3-0-reference.md)
* [Dil desteği](language-support.md)