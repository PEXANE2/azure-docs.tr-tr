---
title: Web hizmetini tüketin
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'dan (klasik) bir makine öğrenme hizmeti dağıtıldıktan sonra, DINLENDIRICI Web hizmeti gerçek zamanlı istek yanıt hizmeti veya toplu yürütme hizmeti olarak tüketilebilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 06/02/2017
ms.openlocfilehash: de395f7167f0ab7d7c6429c14d5efce46831b576
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218229"
---
# <a name="how-to-consume-an-azure-machine-learning-studio-classic-web-service"></a>Azure Machine Learning Studio (klasik) web hizmeti nasıl tüketilir?

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bir Azure Machine Learning Studio (klasik) tahmine dayalı modeli Web hizmeti olarak dağıttıktan sonra, veri göndermek ve öngörüler almak için REST API'sini kullanabilirsiniz. Verileri gerçek zamanlı veya toplu iş modunda gönderebilirsiniz.

Machine Learning Studio 'yı (klasik) kullanarak Bir Machine Learning Web hizmeti oluşturma ve dağıtma hakkında daha fazla bilgiyi burada bulabilirsiniz:

* Machine Learning Studio'da (klasik) nasıl bir deneme oluşturabilirsiniz hakkında bir öğretici için [bkz.](create-experiment.md)
* Bir Web hizmetinin nasıl dağıtılanacağına ilişkin ayrıntılar [için](deploy-a-machine-learning-web-service.md)bkz.
* Genel olarak Makine Öğrenimi hakkında daha fazla bilgi için [Machine Learning Dokümantasyon Merkezi'ni](https://azure.microsoft.com/documentation/services/machine-learning/)ziyaret edin.



## <a name="overview"></a>Genel Bakış
Azure Machine Learning Web hizmeti ile harici bir uygulama, Machine Learning iş akışı puanlama modeliyle gerçek zamanlı olarak iletişim kurar. Machine Learning Web hizmeti çağrısı, tahmin sonuçlarını harici bir uygulamaya döndürür. Machine Learning Web hizmeti çağrısı yapmak için, bir tahmin dağıttığınızda oluşturulan bir API anahtarını geçersiniz. Machine Learning Web hizmeti, web programlama projeleri için popüler bir mimari seçim olan REST'e dayanmaktadır.

Azure Machine Learning Studio (klasik) iki tür hizmete sahiptir:

* İstek-Yanıt Hizmeti (RRS) – Machine Learning Studio'dan (klasik) oluşturulan ve dağıtılan stateless modellerine bir arayüz sağlayan düşük gecikmeli, yüksek ölçeklenebilir bir hizmettir.
* Toplu Yürütme Hizmeti (BES) – Veri kayıtları için toplu iş puanlayan eşzamanlı bir hizmettir.

Machine Learning Web hizmetleri hakkında daha fazla bilgi için [bkz.](deploy-a-machine-learning-web-service.md)

## <a name="get-an-authorization-key"></a>Yetkilendirme anahtarı alma
Denemenizi dağıttığınızda, Web hizmeti için API anahtarları oluşturulur. Anahtarları çeşitli yerlerden alabilirsiniz.

### <a name="from-the-microsoft-azure-machine-learning-web-services-portal"></a>Microsoft Azure Machine Learning Web Services portalından
Microsoft Azure [Machine Learning Web Services](https://services.azureml.net) portalında oturum açın.

Yeni Makine Öğrenimi Web hizmeti için API anahtarını almak için:

1. Azure Machine Learning Web Services portalında, üst menüde **Web Hizmetleri'ni** tıklatın.
2. Anahtarı almak istediğiniz Web hizmetini tıklatın.
3. Üst menüde, **Tüket'i**tıklatın.
4. **Birincil Anahtarı**kopyalayın ve kaydedin.

Klasik Machine Learning Web hizmeti için API anahtarını almak için:

1. Azure Machine Learning Web Services portalında, üst menüde **Klasik Web Hizmetleri'ni** tıklatın.
2. Birlikte çalıştığınız Web hizmetini tıklatın.
3. Anahtarı almak istediğiniz bitiş noktasını tıklatın.
4. Üst menüde, **Tüket'i**tıklatın.
5. **Birincil Anahtarı**kopyalayın ve kaydedin.

### <a name="classic-web-service"></a>Klasik Web hizmeti
 Ayrıca Machine Learning Studio 'dan (klasik) Klasik Web hizmeti için bir anahtar alabilirsiniz.

#### <a name="machine-learning-studio-classic"></a>Machine Learning Studio (klasik)
1. Machine Learning Studio'da (klasik), soldaki **WEB Services'ı** tıklatın.
2. Bir Web hizmetini tıklatın. **API anahtarı** **PANO** sekmesindedir.

## <a name="connect-to-a-machine-learning-web-service"></a><a id="connect"></a>Machine Learning Web hizmetine bağlanın
HTTP isteğini ve yanıtını destekleyen herhangi bir programlama dilini kullanarak bir Machine Learning Web hizmetine bağlanabilirsiniz. Machine Learning Web hizmeti yardım sayfasından C#, Python ve R örneklerini görüntüleyebilirsiniz.

**Makine Öğrenimi API yardımı** Bir Web hizmeti dağıttığınızda Makine Öğrenimi API yardımı oluşturulur. Bkz. [Öğretici 3: Kredi risk modelini dağıtın.](tutorial-part3-credit-risk-deploy.md)
Machine Learning API yardımı, bir tahmin Web hizmeti yle ilgili ayrıntıları içerir.

1. Birlikte çalıştığınız Web hizmetini tıklatın.
2. API Yardım Sayfasını görüntülemek istediğiniz bitiş noktasını tıklatın.
3. Üst menüde, **Tüket'i**tıklatın.
4. İstek-Yanıt veya Toplu Yürütme bitiş noktalarının altındaki **API yardım sayfasını** tıklatın.

**Yeni bir Web hizmeti için Machine Learning API yardımlarını görüntülemek için**

Azure [Machine Learning Web Hizmetleri Portalında:](https://services.azureml.net/)

1. Üst menüde **WEB HİzMETLerİ'ni** tıklatın.
2. Anahtarı almak istediğiniz Web hizmetini tıklatın.

İstek-Yanıt ve Toplu Yürütme Hizmetleri ve C#, R ve Python'daki Örnek kod için Ür'leri almak için **Web Hizmetini Kullan'ı** tıklatın.

Sağlanan ÜR'lerden çağrılan API'ler için Swagger tabanlı belgeleri almak için **Swagger API'yi** tıklatın.

### <a name="c-sample"></a>C# Örnek
Machine Learning Web hizmetine bağlanmak için **Bir HttpClient** geçen ScoreData kullanın. ScoreData, ScoreData'yı temsil eden sayısal özelliklerin n boyutlu vektörü olan FeatureVector'u içerir. Bir API anahtarıyla Machine Learning hizmetine kimlik doğrusursunuz.

Machine Learning Web hizmetine bağlanmak için **Microsoft.AspNet.WebApi.Client** NuGet paketinin yüklenmesi gerekir.

**Microsoft.AspNet.WebApi.Client NuGet in Visual Studio'ya Yükleyin**

1. UCI: Adult 2 sınıfı dataset Web Service'ten İndirme veri kümesini yayımlayın.
2. **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'nu**tıklatın.
3. **Microsoft.AspNet.WebApi.Client yükle'yi**seçin.

**Kod örneğini çalıştırmak için**

1. Machine Learning örnek koleksiyonunun bir parçası olan "Örnek 1: UCI'den veri seti indirin: Yetişkin 2 sınıfı dataset" deneyini yayımlayın.
2. ApiKey'i bir Web hizmetinin anahtarıyla atayın. Bkz. Yukarıda **bir yetkilendirme anahtarı alın.**
3. Request URI ile serviceUri atayın.

**İşte tam bir istek gibi görünecektir.**
```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Formatting;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace CallRequestResponseService
{
    class Program
    {
        static void Main(string[] args)
        {
            InvokeRequestResponseService().Wait();
        }

        static async Task InvokeRequestResponseService()
        {
            using (var client = new HttpClient())
            {
                var scoreRequest = new
                {
                    Inputs = new Dictionary<string, List<Dictionary<string, string>>> () {
                        {
                            "input1",
                            // Replace columns labels with those used in your dataset
                            new List<Dictionary<string, string>>(){new Dictionary<string, string>(){
                                    {
                                        "column1", "value1"
                                    },
                                    {
                                        "column2", "value2"
                                    },
                                    {
                                        "column3", "value3"
                                    }
                                }
                            }
                        },
                    },
                    GlobalParameters = new Dictionary<string, string>() {}
                };

                // Replace these values with your API key and URI found on https://services.azureml.net/
                const string apiKey = "<your-api-key>"; 
                const string apiUri = "<your-api-uri>";
                
                client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue( "Bearer", apiKey);
                client.BaseAddress = new Uri(apiUri);

                // WARNING: The 'await' statement below can result in a deadlock
                // if you are calling this code from the UI thread of an ASP.NET application.
                // One way to address this would be to call ConfigureAwait(false)
                // so that the execution does not attempt to resume on the original context.
                // For instance, replace code such as:
                //      result = await DoSomeTask()
                // with the following:
                //      result = await DoSomeTask().ConfigureAwait(false)

                HttpResponseMessage response = await client.PostAsJsonAsync("", scoreRequest);

                if (response.IsSuccessStatusCode)
                {
                    string result = await response.Content.ReadAsStringAsync();
                    Console.WriteLine("Result: {0}", result);
                }
                else
                {
                    Console.WriteLine(string.Format("The request failed with status code: {0}", response.StatusCode));

                    // Print the headers - they include the request ID and the timestamp,
                    // which are useful for debugging the failure
                    Console.WriteLine(response.Headers.ToString());

                    string responseContent = await response.Content.ReadAsStringAsync();
                    Console.WriteLine(responseContent);
                }
            }
        }
    }
}
```

### <a name="python-sample"></a>Python Örneği
Machine Learning Web hizmetine bağlanmak için Python 2.X için **urllib2** kitaplığını ve Python 3.X için **urllib.request** kitaplığını kullanın. ScoreData'yı temsil eden sayısal özelliklerin n boyutlu vektörü olan FeatureVector'u içeren ScoreData'yı geçeceksiniz. Bir API anahtarıyla Machine Learning hizmetine kimlik doğrusursunuz.

**Kod örneğini çalıştırmak için**

1. Machine Learning örnek koleksiyonunun bir parçası olan "Örnek 1: UCI'den veri kümesini indirin: Yetişkin 2 sınıfı dataset" deneyini dağıtın.
2. ApiKey'i bir Web hizmetinin anahtarıyla atayın. Bu makalenin başına yakın **bir yetkilendirme anahtarı bölümüne** bakın.
3. Request URI ile serviceUri atayın.

**İşte tam bir istek gibi görünecektir.**
```python
import urllib2 # urllib.request and urllib.error for Python 3.X
import json

data = {
    "Inputs": {
        "input1":
        [
            {
                'column1': "value1",   
                'column2': "value2",   
                'column3': "value3"
            }
        ],
    },
    "GlobalParameters":  {}
}

body = str.encode(json.dumps(data))

# Replace this with the URI and API Key for your web service
url = '<your-api-uri>'
api_key = '<your-api-key>'
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

# "urllib.request.Request(url, body, headers)" for Python 3.X
req = urllib2.Request(url, body, headers)

try:
    # "urllib.request.urlopen(req)" for Python 3.X
    response = urllib2.urlopen(req)

    result = response.read()
    print(result)
# "urllib.error.HTTPError as error" for Python 3.X
except urllib2.HTTPError, error: 
    print("The request failed with status code: " + str(error.code))

    # Print the headers - they include the request ID and the timestamp, which are useful for debugging the failure
    print(error.info())
    print(json.loads(error.read())) 
```

### <a name="r-sample"></a>R Örneği

Machine Learning Web Service'e bağlanmak için, isteği yapmak ve döndürülen JSON yanıtını işlemek için **RCurl** ve **rjson** kitaplıklarını kullanın. ScoreData'yı temsil eden sayısal özelliklerin n boyutlu vektörü olan FeatureVector'u içeren ScoreData'yı geçeceksiniz. Bir API anahtarıyla Machine Learning hizmetine kimlik doğrusursunuz.

**İşte tam bir istek gibi görünecektir.**
```r
library("RCurl")
library("rjson")

# Accept SSL certificates issued by public Certificate Authorities
options(RCurlOptions = list(cainfo = system.file("CurlSSL", "cacert.pem", package = "RCurl")))

h = basicTextGatherer()
hdr = basicHeaderGatherer()

req = list(
    Inputs = list(
            "input1" = list(
                list(
                        'column1' = "value1",
                        'column2' = "value2",
                        'column3' = "value3"
                    )
            )
        ),
        GlobalParameters = setNames(fromJSON('{}'), character(0))
)

body = enc2utf8(toJSON(req))
api_key = "<your-api-key>" # Replace this with the API key for the web service
authz_hdr = paste('Bearer', api_key, sep=' ')

h$reset()
curlPerform(url = "<your-api-uri>",
httpheader=c('Content-Type' = "application/json", 'Authorization' = authz_hdr),
postfields=body,
writefunction = h$update,
headerfunction = hdr$update,
verbose = TRUE
)

headers = hdr$value()
httpStatus = headers["status"]
if (httpStatus >= 400)
{
print(paste("The request failed with status code:", httpStatus, sep=" "))

# Print the headers - they include the request ID and the timestamp, which are useful for debugging the failure
print(headers)
}

print("Result:")
result = h$value()
print(fromJSON(result))
```

### <a name="javascript-sample"></a>JavaScript Örneği

Machine Learning Web Service'e bağlanmak için projenizdeki **istek** npm paketini kullanın. Ayrıca, girişinizi `JSON` biçimlendirmek ve sonucu ayrıştırmak için nesneyi de kullanırsınız. Kullanarak `npm install request --save`yükleyin, `"request": "*"` veya paketinize ekleyin.json altında `dependencies` ve çalıştırın. `npm install`

**İşte tam bir istek gibi görünecektir.**
```js
let req = require("request");

const uri = "<your-api-uri>";
const apiKey = "<your-api-key>";

let data = {
    "Inputs": {
        "input1":
        [
            {
                'column1': "value1",
                'column2': "value2",
                'column3': "value3"
            }
        ],
    },
    "GlobalParameters": {}
}

const options = {
    uri: uri,
    method: "POST",
    headers: {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + apiKey,
    },
    body: JSON.stringify(data)
}

req(options, (err, res, body) => {
    if (!err && res.statusCode == 200) {
        console.log(body);
    } else {
        console.log("The request failed with status code: " + res.statusCode);
    }
});
```
