---
title: Dağıtılan Web hizmetini tüketmek için istemci oluşturma
titleSuffix: Azure Machine Learning service
description: Bir model Azure Machine Learning modeliyle dağıtıldığında oluşturulan bir Web hizmetini nasıl kullanacağınızı öğrenin. Web hizmeti bir REST API gösterir. Tercih ettiğiniz programlama dilini kullanarak bu API için istemcileri oluşturun.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 07/10/2019
ms.custom: seodec18
ms.openlocfilehash: 7799b62b2c330610663e361bbb3930340b1ebdaf
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726293"
---
# <a name="consume-an-azure-machine-learning-model-deployed-as-a-web-service"></a>Bir web hizmeti olarak bir Azure Machine Learning modeli kullanma

Bir Azure Machine Learning modeli bir web hizmeti olarak dağıtma, bir REST API oluşturur. Bu API için veri göndermek ve modeli tarafından döndürülen tahmin alırsınız. Bu belgede, Web hizmeti için, Go, Java ve Python kullanarak C#istemci oluşturma hakkında bilgi edinin.

Azure Container Instances, Azure Kubernetes hizmeti veya alan-programlanabilir kapı dizileri (FPGA) için bir görüntü dağıtırken bir Web hizmeti oluşturursunuz. Kayıtlı modeller ve Puanlama dosyalarından görüntüler oluşturursunuz. [Azure MACHINE LEARNING SDK](https://aka.ms/aml-sdk)kullanarak bir Web hizmetine erişmek IÇIN kullanılan URI 'yi alırsınız. Kimlik doğrulamasını etkinleştirdiyseniz, kimlik doğrulama anahtarlarını almak için SDK'yı da kullanabilirsiniz.

Machine Learning Web hizmeti kullanan bir istemci oluşturmak için genel iş akışı:

1. Bağlantı bilgilerini almak için SDK 'Yı kullanın.
1. Model tarafından kullanılan istek verilerinin türünü belirleme.
1. Web hizmetini çağıran bir uygulama oluşturun.

## <a name="connection-information"></a>Bağlantı bilgileri

> [!NOTE]
> Web hizmeti bilgilerini almak için Azure Machine Learning SDK 'sını kullanın. Bir Python SDK'sı budur. Hizmet için bir istemci oluşturmak üzere herhangi bir dili kullanabilirsiniz.

[Azureml. Core. WebService](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) sınıfı, istemci oluşturmak için gereken bilgileri sağlar. Aşağıdaki `Webservice` Özellikler bir istemci uygulaması oluşturmak için yararlıdır:

* `auth_enabled`-Anahtar kimlik doğrulaması etkinse `True`; Aksi takdirde,. `False`
* `token_auth_enabled`-Belirteç kimlik doğrulaması etkinse `True`; Aksi takdirde,. `False`
* `scoring_uri` -REST API adresi.


Dağıtılan web hizmetleri için bu bilgileri almak için üç yol vardır:

* Bir model dağıttığınızda bir `Webservice` nesne hizmetiyle ilgili bilgi döndürülür:

    ```python
    service = Webservice.deploy_from_model(name='myservice',
                                           deployment_config=myconfig,
                                           models=[model],
                                           image_config=image_config,
                                           workspace=ws)
    print(service.scoring_uri)
    ```

* Kullanabileceğiniz `Webservice.list` bir listesini almak için çalışma alanınızdaki modelleri için web hizmetleri dağıtıldı. Döndürülen bilgileri listesini daraltmak için filtre ekleyebilirsiniz. Ne filtrelenebilir hakkında daha fazla bilgi için, bkz. [WebService. List](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py) başvuru belgeleri.

    ```python
    services = Webservice.list(ws)
    print(services[0].scoring_uri)
    ```

* Dağıtılan hizmetin adını biliyorsanız, ' nin `Webservice`yeni bir örneğini oluşturabilir ve çalışma alanını ve hizmet adını parametreler olarak sağlayabilirsiniz. Yeni nesne dağıtılan hizmeti hakkında bilgi içerir.

    ```python
    service = Webservice(workspace=ws, name='myservice')
    print(service.scoring_uri)
    ```

### <a name="authentication-for-services"></a>Hizmetler için kimlik doğrulaması

Azure Machine Learning, Web hizmetlerinizi erişimi denetlemek için iki yol sağlar. 

|Kimlik Doğrulama Yöntemi|ACI|AKS|
|---|---|---|
|Anahtar|Varsayılan olarak devre dışı| Varsayılan olarak etkin|
|Belirteç| Kullanılamaz| Varsayılan olarak devre dışı |
#### <a name="authentication-with-keys"></a>Anahtarlar ile kimlik doğrulama

Bir dağıtım için kimlik doğrulamasını etkinleştirdiğinizde, otomatik olarak kimlik doğrulama anahtarları oluşturursunuz.

* Azure Kubernetes hizmetine dağıtım yaparken, varsayılan olarak kimlik doğrulaması etkinleştirilir.
* Azure Container Instances dağıtım yaparken, varsayılan olarak kimlik doğrulaması devre dışıdır.

Kimlik doğrulamasını denetlemek için, bir `auth_enabled` dağıtım oluştururken veya güncelleştirirken parametresini kullanın.

Kimlik doğrulamasını etkinleştirdiyseniz, kullanabileceğiniz `get_keys` yönteminin bir birincil ve ikincil kimlik doğrulama anahtarı almak için:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Bir anahtarı yeniden oluşturmak ihtiyacınız varsa [ `service.regen_key` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py).


#### <a name="authentication-with-tokens"></a>Belirteçlerle kimlik doğrulama

Bir Web hizmeti için belirteç kimlik doğrulamasını etkinleştirdiğinizde, bir kullanıcının, Web hizmetine erişmek için bir Azure Machine Learning JWT belirteci sağlaması gerekir. 

* Azure Kubernetes hizmetine dağıtım yaparken belirteç kimlik doğrulaması varsayılan olarak devre dışıdır.
* Azure Container Instances dağıtım yaparken belirteç kimlik doğrulaması desteklenmez.

Belirteç kimlik doğrulamasını denetlemek için, bir `token_auth_enabled` dağıtım oluştururken veya güncelleştirirken parametresini kullanın.

Belirteç kimlik doğrulaması etkinleştirilirse, bir taşıyıcı belirteci almak için `get_token` yöntemini ve bu belirteçlerin süre sonu süresini kullanabilirsiniz:

```python
token, refresh_by = service.get_tokens()
print(token)
```

> [!IMPORTANT]
> Belirtecin `refresh_by` zamanından sonra yeni bir belirteç istemeniz gerekir. 

## <a name="request-data"></a>İstek verileri

REST API isteği aşağıdaki yapıya sahip bir JSON belge gövdesinin bekliyor:

```json
{
    "data":
        [
            <model-specific-data-structure>
        ]
}
```

> [!IMPORTANT]
> Verilerin yapısı, hangi Puanlama betiği ve hizmet expect modelinde ile eşleşmesi gerekiyor. Puanlama betiği modele iletmeden önce verileri değiştirebilir.

Örneğin, modelde [Not Defteri içinde Train](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) örnek 10 sayıdan oluşan bir diziyi bekliyor. Bu örneğe ilişkin Puanlama betiği, istekten bir sayısal tuş bir y dizisi oluşturur ve modele geçirir. Aşağıdaki örnek, bu hizmet bekliyor veri gösterir:

```json
{
    "data": 
        [
            [
                0.0199132141783263, 
                0.0506801187398187, 
                0.104808689473925, 
                0.0700725447072635, 
                -0.0359677812752396, 
                -0.0266789028311707, 
                -0.0249926566315915, 
                -0.00259226199818282, 
                0.00371173823343597, 
                0.0403433716478807
            ]
        ]
}
``` 

Web hizmeti, birden çok bir istekteki veri kümelerini kabul edebilir. Yanıt bir dizi içeren bir JSON belgesini döndürür.

### <a name="binary-data"></a>İkili veriler

Bir görüntü gibi ikili veri modelinizi kabul ediyorsa değiştirmelisiniz `score.py` dağıtımınız için ham HTTP isteklerini kabul etmek için kullanılan dosya. İkili verileri kabul eden bir `score.py` örneği aşağıda verilmiştir:

```python
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse


def init():
    print("This is init()")


@rawhttp
def run(request):
    print("This is run()")
    print("Request: [{0}]".format(request))
    if request.method == 'GET':
        # For this example, just return the URL for GETs
        respBody = str.encode(request.full_path)
        return AMLResponse(respBody, 200)
    elif request.method == 'POST':
        reqBody = request.get_data(False)
        # For a real world solution, you would load the data from reqBody
        # and send to the model. Then return the response.

        # For demonstration purposes, this example just returns the posted data as the response.
        return AMLResponse(reqBody, 200)
    else:
        return AMLResponse("bad request", 500)
```

> [!IMPORTANT]
> Hizmeti geliştirmekle çalışırken ad alanı sıklıkla değişir. `azureml.contrib` Bu nedenle, bu ad alanındaki her şey Microsoft tarafından tam olarak desteklenmez ve önizleme olarak değerlendirilmelidir.
>
> Bunu yerel geliştirme ortamınızda test etmeniz gerekirse, aşağıdaki komutu kullanarak bu bileşenleri `contrib` ad alanına yükleyebilirsiniz:
> 
> ```shell
> pip install azureml-contrib-services
> ```

## <a name="call-the-service-c"></a>Hizmet çağrısı (C#)

Bu örnek nasıl kullanılacağını gösterir C# oluşturulan web hizmeti çağırmak amacıyla [eğitme Not Defteri içinde](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) örneği:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MLWebServiceClient
{
    // The data structure expected by the service
    internal class InputData
    {
        [JsonProperty("data")]
        // The service used by this example expects an array containing
        //   one or more arrays of doubles
        internal double[,] data;
    }
    class Program
    {
        static void Main(string[] args)
        {
            // Set the scoring URI and authentication key
            string scoringUri = "<your web service URI>";
            string authKey = "<your key>";

            // Set the data to be sent to the service.
            // In this case, we are sending two sets of data to be scored.
            InputData payload = new InputData();
            payload.data = new double[,] {
                {
                    0.0199132141783263,
                    0.0506801187398187,
                    0.104808689473925,
                    0.0700725447072635,
                    -0.0359677812752396,
                    -0.0266789028311707,
                    -0.0249926566315915,
                    -0.00259226199818282,
                    0.00371173823343597,
                    0.0403433716478807
                },
                {
                    -0.0127796318808497, 
                    -0.044641636506989, 
                    0.0606183944448076, 
                    0.0528581912385822, 
                    0.0479653430750293, 
                    0.0293746718291555, 
                    -0.0176293810234174, 
                    0.0343088588777263, 
                    0.0702112981933102, 
                    0.00720651632920303
                }
            };

            // Create the HTTP client
            HttpClient client = new HttpClient();
            // Set the auth header. Only needed if the web service requires authentication.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", authKey);

            // Make the request
            try {
                var request = new HttpRequestMessage(HttpMethod.Post, new Uri(scoringUri));
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;
                // Display the response from the web service
                Console.WriteLine(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
    }
}
```

Döndürülen sonuçlar için aşağıdaki JSON belgesini benzerdir:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-go"></a>Arama Hizmeti (Git)

Bu örnekte oluşturulan web hizmeti çağırmak için Git kullanmayı gösteren [Not Defteri içinde Train](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) örnek:

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
)

// Features for this model are an array of decimal values
type Features []float64

// The web service input can accept multiple sets of values for scoring
type InputData struct {
    Data []Features `json:"data",omitempty`
}

// Define some example data
var exampleData = []Features{
    []float64{
        0.0199132141783263, 
        0.0506801187398187, 
        0.104808689473925, 
        0.0700725447072635, 
        -0.0359677812752396, 
        -0.0266789028311707, 
        -0.0249926566315915, 
        -0.00259226199818282, 
        0.00371173823343597, 
        0.0403433716478807,
    },
    []float64{
        -0.0127796318808497, 
        -0.044641636506989, 
        0.0606183944448076, 
        0.0528581912385822, 
        0.0479653430750293, 
        0.0293746718291555, 
        -0.0176293810234174, 
        0.0343088588777263, 
        0.0702112981933102, 
        0.00720651632920303,
    },
}

// Set to the URI for your service
var serviceUri string = "<your web service URI>"
// Set to the authentication key (if any) for your service
var authKey string = "<your key>"

func main() {
    // Create the input data from example data
    jsonData := InputData{
        Data: exampleData,
    }
    // Create JSON from it and create the body for the HTTP request
    jsonValue, _ := json.Marshal(jsonData)
    body := bytes.NewBuffer(jsonValue)

    // Create the HTTP request
    client := &http.Client{}
    request, err := http.NewRequest("POST", serviceUri, body)
    request.Header.Add("Content-Type", "application/json")

    // These next two are only needed if using an authentication key
    bearer := fmt.Sprintf("Bearer %v", authKey)
    request.Header.Add("Authorization", bearer)

    // Send the request to the web service
    resp, err := client.Do(request)
    if err != nil {
        fmt.Println("Failure: ", err)
    }

    // Display the response received
    respBody, _ := ioutil.ReadAll(resp.Body)
    fmt.Println(string(respBody))
}
```

Döndürülen sonuçlar için aşağıdaki JSON belgesini benzerdir:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-java"></a>Arama Hizmeti (Java)

Bu örnek, Java oluşturulan web hizmetini çağırmak için nasıl kullanılacağını gösterir. [Not Defteri içinde Train](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) örneği:

```java
import java.io.IOException;
import org.apache.http.client.fluent.*;
import org.apache.http.entity.ContentType;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;

public class App {
    // Handle making the request
    public static void sendRequest(String data) {
        // Replace with the scoring_uri of your service
        String uri = "<your web service URI>";
        // If using authentication, replace with the auth key
        String key = "<your key>";
        try {
            // Create the request
            Content content = Request.Post(uri)
            .addHeader("Content-Type", "application/json")
            // Only needed if using authentication
            .addHeader("Authorization", "Bearer " + key)
            // Set the JSON data as the body
            .bodyString(data, ContentType.APPLICATION_JSON)
            // Make the request and display the response.
            .execute().returnContent();
            System.out.println(content);
        }
        catch (IOException e) {
            System.out.println(e);
        }
    }
    public static void main(String[] args) {
        // Create the data to send to the service
        JSONObject obj = new JSONObject();
        // In this case, it's an array of arrays
        JSONArray dataItems = new JSONArray();
        // Inner array has 10 elements
        JSONArray item1 = new JSONArray();
        item1.add(0.0199132141783263);
        item1.add(0.0506801187398187);
        item1.add(0.104808689473925);
        item1.add(0.0700725447072635);
        item1.add(-0.0359677812752396);
        item1.add(-0.0266789028311707);
        item1.add(-0.0249926566315915);
        item1.add(-0.00259226199818282);
        item1.add(0.00371173823343597);
        item1.add(0.0403433716478807);
        // Add the first set of data to be scored
        dataItems.add(item1);
        // Create and add the second set
        JSONArray item2 = new JSONArray();
        item2.add(-0.0127796318808497);
        item2.add(-0.044641636506989);
        item2.add(0.0606183944448076);
        item2.add(0.0528581912385822);
        item2.add(0.0479653430750293);
        item2.add(0.0293746718291555);
        item2.add(-0.0176293810234174);
        item2.add(0.0343088588777263);
        item2.add(0.0702112981933102);
        item2.add(0.00720651632920303);
        dataItems.add(item2);
        obj.put("data", dataItems);

        // Make the request using the JSON document string
        sendRequest(obj.toJSONString());
    }
}
```

Döndürülen sonuçlar için aşağıdaki JSON belgesini benzerdir:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-python"></a>Arama Hizmeti (Python)

Bu örnek Python oluşturulan web hizmetini çağırmak için nasıl kullanılacağını gösterir [Not Defteri içinde Train](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) örneği:

```python
import requests
import json

# URL for the web service
scoring_uri = '<your web service URI>'
# If the service is authenticated, set the key
key = '<your key>'

# Two sets of data to score, so we get two results back
data = {"data":
        [
            [
                0.0199132141783263,
                0.0506801187398187,
                0.104808689473925,
                0.0700725447072635,
                -0.0359677812752396,
                -0.0266789028311707,
                -0.0249926566315915,
                -0.00259226199818282,
                0.00371173823343597,
                0.0403433716478807
            ],
            [
                -0.0127796318808497,
                -0.044641636506989,
                0.0606183944448076,
                0.0528581912385822,
                0.0479653430750293,
                0.0293746718291555,
                -0.0176293810234174,
                0.0343088588777263,
                0.0702112981933102,
                0.00720651632920303]
        ]
        }
# Convert to JSON string
input_data = json.dumps(data)

# Set the content type
headers = {'Content-Type': 'application/json'}
# If authentication is enabled, set the authorization header
headers['Authorization'] = f'Bearer {key}'

# Make the request and display the response
resp = requests.post(scoring_uri, input_data, headers=headers)
print(resp.text)
```

Döndürülen sonuçlar için aşağıdaki JSON belgesini benzerdir:

```JSON
[217.67978776218715, 224.78937091757172]
```

## <a name="consume-the-service-from-power-bi"></a>Hizmeti Power BI tüketme

Power BI, Power BI verileri tahmine dayalı olarak zenginleştirmek için Azure Machine Learning Web hizmetlerinin kullanımını destekler. 

Power BI, tüketim için desteklenen bir Web hizmeti oluşturmak için, şemanın Power BI gereken biçimi desteklemesi gerekir. [Power BI tarafından desteklenen bir şema oluşturmayı öğrenin](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#example-script-with-dictionary-input-support-consumption-from-power-bi).

Web hizmeti dağıtıldıktan sonra, Power BI veri akışlarından tüketilebilir. [Power BI bir Azure Machine Learning Web hizmeti kullanmayı öğrenin](https://docs.microsoft.com/power-bi/service-machine-learning-integration).
