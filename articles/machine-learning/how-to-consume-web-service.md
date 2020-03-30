---
title: Web hizmeti olarak dağıtılan model için istemci oluşturma
titleSuffix: Azure Machine Learning
description: Azure Machine Learning modeliyle bir model dağıtıldığında oluşturulan bir web hizmetini nasıl tükettiğinizi öğrenin. Web hizmeti bir REST API ortaya çıkarır. Seçtiğiniz programlama dilini kullanarak bu API için istemciler oluşturun.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 01/07/2020
ms.custom: seodec18
ms.openlocfilehash: a86b8ddb59719db9bdaffea44aecd5428ad16834
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80282673"
---
# <a name="consume-an-azure-machine-learning-model-deployed-as-a-web-service"></a>Web hizmeti olarak dağıtılan bir Azure Machine Learning modelini kullanma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning modelini web hizmeti olarak dağıtmak, REST API'si oluşturur. Bu API'ye veri gönderebilir ve model tarafından döndürülen öngörüyü alabilirsiniz. Bu belgede, C#, Go, Java ve Python kullanarak web hizmeti için istemci oluşturmayı öğrenin.

Bir görüntüyü Azure Kapsayıcı Örneklerine, Azure Kubernetes Hizmetine veya alan tarafından programlanabilir geçit dizilerine (FPGA) dağıttığınızda bir web hizmeti oluşturursunuz. Kayıtlı modellerden ve puanlama dosyalarından görüntüler oluşturursunuz. [Azure Machine Learning SDK'yı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)kullanarak bir web hizmetine erişmek için kullanılan URI'yi alırsınız. Kimlik doğrulama etkinse, kimlik doğrulama anahtarlarını veya belirteçlerini almak için SDK'yı da kullanabilirsiniz.

Makine öğrenme web hizmeti kullanan bir istemci oluşturmak için genel iş akışı:

1. Bağlantı bilgilerini almak için SDK'yı kullanın.
1. Model tarafından kullanılan istek verisinin türünü belirleyin.
1. Web hizmetini çağıran bir uygulama oluşturun.

> [!TIP]
> Bu belgedeki örnekler OpenAPI (Swagger) belirtimleri kullanılmadan el ile oluşturulur. Dağıtımınız için bir OpenAPI belirtimi etkinleştirdiyseniz, hizmetiniz için istemci kitaplıkları oluşturmak için [swagger-codegen](https://github.com/swagger-api/swagger-codegen) gibi araçları kullanabilirsiniz.

## <a name="connection-information"></a>Bağlantı bilgileri

> [!NOTE]
> Web hizmeti bilgilerini almak için Azure Machine Learning SDK'yı kullanın. Bu bir Python SDK. Hizmet için bir istemci oluşturmak için herhangi bir dili kullanabilirsiniz.

[azureml.core.Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py) sınıfı, istemci oluşturmak için ihtiyacınız olan bilgileri sağlar. Aşağıdaki `Webservice` özellikler bir istemci uygulaması oluşturmak için yararlıdır:

* `auth_enabled`- Anahtar kimlik doğrulaması `True`etkinse; aksi `False`takdirde, .
* `token_auth_enabled`- Belirteç kimlik doğrulaması etkinse; `True` aksi `False`takdirde, .
* `scoring_uri`- REST API adresi.
* `swagger_uri`- OpenAPI belirtiminin adresi. Otomatik şema üretimini etkinleştiriseniz bu URI kullanılabilir. Daha fazla bilgi için Azure [Machine Learning ile Modelleri Dağıt'a](how-to-deploy-and-where.md)bakın.

Dağıtılan web hizmetleri için bu bilgileri almanın üç yolu vardır:

* Bir model dağıttığınızda, `Webservice` bir nesne hizmet hakkında bilgi ile döndürülür:

    ```python
    service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
    service.wait_for_deployment(show_output = True)
    print(service.scoring_uri)
    print(service.swagger_uri)
    ```

* Çalışma alanınızdaki modeller için dağıtılan web hizmetlerinin listesini almak için kullanabilirsiniz. `Webservice.list` Döndürülen bilgi listesini daraltmak için filtreler ekleyebilirsiniz. Nelerhakkında daha fazla bilgi için [Webservice.list](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice.webservice?view=azure-ml-py) başvuru belgelerine bakın.

    ```python
    services = Webservice.list(ws)
    print(services[0].scoring_uri)
    print(services[0].swagger_uri)
    ```

* Dağıtılan hizmetin adını biliyorsanız, yeni bir örnek `Webservice`oluşturabilir ve çalışma alanını ve hizmet adını parametre olarak sağlayabilirsiniz. Yeni nesne, dağıtılan hizmet hakkında bilgi içerir.

    ```python
    service = Webservice(workspace=ws, name='myservice')
    print(service.scoring_uri)
    print(service.swagger_uri)
    ```

### <a name="secured-web-service"></a>Güvenli web hizmeti

Dağıtılan web hizmetini TLS/SSL sertifikası kullanarak güvence altına aldıysanız, [puantaj](https://en.wikipedia.org/wiki/HTTPS) veya swagger URI'yi kullanarak hizmete bağlanmak için HTTPS'yi kullanabilirsiniz. HTTPS, ikisi arasındaki iletişimi şifreleyerek bir istemci ve bir web hizmeti arasındaki iletişimin güvenliğini sağlamaya yardımcı olur. Şifreleme, [Aktarım Katmanı Güvenliği 'ni (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security)kullanır. TLS bazen hala TLS selefi olan *Güvenli Soketler Katmanı* (SSL) olarak adlandırılır.

> [!IMPORTANT]
> Azure Machine Learning tarafından dağıtılan Web hizmetleri yalnızca TLS sürüm 1.2'yi destekler. İstemci uygulaması oluştururken, bu sürümü desteklediğinden emin olun.

Daha fazla bilgi için Azure [Machine Learning aracılığıyla bir web hizmetini güvence altına almak için TLS'yi kullanın'a](how-to-secure-web-service.md)bakın.

### <a name="authentication-for-services"></a>Hizmetler için kimlik doğrulama

Azure Machine Learning, web hizmetlerinize erişimi denetlemenin iki yolunu sağlar.

|Kimlik Doğrulama Yöntemi|Acı|AKS|
|---|---|---|
|Anahtar|Varsayılan olarak devre dışı| Varsayılan olarak etkindir|
|Belirteç| Kullanılamaz| Varsayılan olarak devre dışı |

Bir anahtar veya belirteç ile güvenli bir hizmete istek gönderirken, anahtarı veya belirteçgeçmek için __Yetkilendirme__ üstbilgisini kullanın. Anahtar veya belirteç olarak `Bearer <key-or-token>`biçimlendirilmelidir , anahtar veya belirteç değeri nerede. `<key-or-token>`

#### <a name="authentication-with-keys"></a>Anahtarlarla kimlik doğrulama

Dağıtım için kimlik doğrulamasını etkinleştirdiğinizde, otomatik olarak kimlik doğrulama anahtarları oluşturursunuz.

* Azure Kubernetes Hizmetine dağıtırken kimlik doğrulaması varsayılan olarak etkinleştirilir.
* Azure Kapsayıcı Örnekleri'ne dağıtırken kimlik doğrulaması varsayılan olarak devre dışı bırakılır.

Kimlik doğrulamayı denetlemek `auth_enabled` için, dağıtım oluştururken veya güncellerken parametreyi kullanın.

Kimlik doğrulaması etkinleştirilmişse, `get_keys` birincil ve ikincil kimlik doğrulama anahtarını almak için yöntemi kullanabilirsiniz:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Bir anahtarı yenilemeniz gerekiyorsa, [`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)kullanın.

#### <a name="authentication-with-tokens"></a>Belirteçlerle kimlik doğrulama

Bir web hizmeti için belirteç kimlik doğrulamasını etkinleştirdiğinizde, kullanıcının web hizmetine erişmek için bir Azure Machine Learning JWT belirteci sağlaması gerekir. 

* Azure Kubernetes Hizmetine dağıtırken belirteç kimlik doğrulaması varsayılan olarak devre dışı bırakılır.
* Azure Kapsayıcı Örnekleri'ne dağıtırken belirteç kimlik doğrulaması desteklenmez.

Belirteç kimlik doğrulamasını `token_auth_enabled` denetlemek için, dağıtım oluştururken veya güncellerken parametreyi kullanın.

Belirteç kimlik doğrulaması etkinleştirilmişse, taşıyıcı belirteci almak için `get_token` yöntemi kullanabilirsiniz ve bu belirteçleri son kullanma süresi:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Belirteç `refresh_by` saatinden sonra yeni bir belirteç talep etmeniz gerekir. 

## <a name="request-data"></a>Veri isteme

REST API, isteğin gövdesinin aşağıdaki yapıya sahip bir JSON belgesi olmasını bekler:

```json
{
    "data":
        [
            <model-specific-data-structure>
        ]
}
```

> [!IMPORTANT]
> Verilerin yapısı, hizmetteki puanlama komut dosyası ve modelinin beklediğiyle eşleşmesi gerekir. Puanlama komut dosyası, verileri modele geçirmeden önce değiştirebilir.

Örneğin, not defteri örneğindeki [Tren'deki](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) model 10 numaradan oluşan bir dizi bekler. Bu örnek için puanlama komut dosyası istekten bir Numpy dizi oluşturur ve modele geçirir. Aşağıdaki örnek, bu hizmetin beklediği verileri gösterir:

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

Web hizmeti, tek bir istekte birden çok veri kümesini kabul edebilir. Bir dizi yanıt içeren bir JSON belgesini döndürür.

### <a name="binary-data"></a>İkili veriler

Hizmetinizde ikili veri desteğini etkinleştirme hakkında bilgi için [Ikili verilere](how-to-deploy-and-where.md#binary)bakın.

### <a name="cross-origin-resource-sharing-cors"></a>Orijinler arası kaynak paylaşımı (CORS)

Hizmetinizde CORS desteğini etkinleştirme hakkında bilgi için, [kökenler arası kaynak paylaşımına](how-to-deploy-and-where.md#cors)bakın.

## <a name="call-the-service-c"></a>Hizmeti arayın (C#)

Bu örnek, [Not defteri örneğinde Tren'den](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) oluşturulan web hizmetini aramak için C#'ın nasıl kullanılacağını gösterir:

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
            // Set the scoring URI and authentication key or token
            string scoringUri = "<your web service URI>";
            string authKey = "<your key or token>";

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

Döndürülen sonuçlar aşağıdaki JSON belgesine benzer:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-go"></a>Hizmeti arayın (Git)

Bu örnek, [Not defteri örneğinde Tren'den](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) oluşturulan web hizmetini aramak için Git'in nasıl kullanılacağını gösterir:

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
// Set to the authentication key or token (if any) for your service
var authKey string = "<your key or token>"

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

Döndürülen sonuçlar aşağıdaki JSON belgesine benzer:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-java"></a>Hizmeti arayın (Java)

Bu örnek, [Not defteri örneğinde Tren'den](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) oluşturulan web hizmetini aramak için Java'nın nasıl kullanılacağını gösterir:

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
        // If using authentication, replace with the auth key or token
        String key = "<your key or token>";
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

Döndürülen sonuçlar aşağıdaki JSON belgesine benzer:

```json
[217.67978776218715, 224.78937091757172]
```

## <a name="call-the-service-python"></a>Hizmeti arayın (Python)

Bu örnek, Python'un not defteri içinde [Trend'den](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-within-notebook/train-within-notebook.ipynb) oluşturulan web hizmetini aramak için nasıl kullanılacağını gösterir:

```python
import requests
import json

# URL for the web service
scoring_uri = '<your web service URI>'
# If the service is authenticated, set the key or token
key = '<your key or token>'

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

Döndürülen sonuçlar aşağıdaki JSON belgesine benzer:

```JSON
[217.67978776218715, 224.78937091757172]
```

## <a name="consume-the-service-from-power-bi"></a>Hizmeti Power BI’dan kullanma

Power BI, Power BI'deki verileri öngörülerle zenginleştirmek için Azure Machine Learning web hizmetlerinin tüketimini destekler. 

Power BI'de tüketim için desteklenen bir web hizmeti oluşturmak için şema, Power BI tarafından gerekli olan biçimi desteklemelidir. [Güç BI destekli şema nasıl oluşturulacak](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-and-where#example-entry-script)öğrenin.

Web hizmeti dağıtıldıktan sonra Power BI veri akışlarından tüketilebilir. [Power BI'den Azure Machine Learning web hizmetini nasıl tükettiğinizi öğrenin.](https://docs.microsoft.com/power-bi/service-machine-learning-integration)

## <a name="next-steps"></a>Sonraki adımlar

Python ve derin öğrenme modellerinin gerçek zamanlı puanlaması için bir başvuru mimarisini görüntülemek için [Azure mimari merkezine](/azure/architecture/reference-architectures/ai/realtime-scoring-python)gidin.
