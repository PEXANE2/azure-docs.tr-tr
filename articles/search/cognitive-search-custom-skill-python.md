---
title: Özel beceri örneği (Python)
titleSuffix: Azure Cognitive Search
description: Python geliştiricileri için, Azure Işlevleri ve Visual Studio kullanarak özel bir beceri oluşturmaya yönelik araçları ve teknikleri öğrenin. Özel yetenekler, Azure Bilişsel Arama ' de bir AI zenginleştirme dizin oluşturma işlem hattına ekleyebileceğiniz Kullanıcı tanımlı modelleri veya mantığı içerir.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/15/2020
ms.custom: tracking-python
ms.openlocfilehash: 7945b3b0aeba7f5135665f4413d304eb225ac749
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84604703"
---
# <a name="example-create-a-custom-skill-using-python"></a>Örnek: Python kullanarak özel bir yetenek oluşturma

Bu Azure Bilişsel Arama Beceri örneğinde, Python ve Visual Studio Code kullanarak Web API özel yeteneği oluşturmayı öğreneceksiniz. Örnek, [özel yetenek arabirimini](cognitive-search-custom-skill-interface.md)uygulayan bir [Azure işlevi](https://azure.microsoft.com/services/functions/) kullanır.

Özel beceri tasarım tarafından basittir (iki dizeyi birleştirir), böylece Python 'da özel beceri geliştirme için kullanılan araçlara ve teknolojilere odaklanırsınız. Basit bir beceriyle başarılı olduktan sonra daha karmaşık senaryolara sahip olabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

+ Özel bir yeteneğin uygulanması gereken giriş/çıkış arabirimine giriş için [özel beceri arabirimini](cognitive-search-custom-skill-interface.md) gözden geçirin.

+ Ortamınızı ayarlayın. [Bu öğreticiyi](https://docs.microsoft.com/azure/python/tutorial-vs-code-serverless-python-01) , Visual Studio Code ve Python uzantıları kullanarak sunucusuz Azure işlevini ayarlamaya yönelik uçtan uca izliyoruz. Öğretici, aşağıdaki araçların ve bileşenlerin yüklenmesine kılavuzluk eden bir yol gösterir: 

  + [Python 3,75](https://www.python.org/downloads/release/python-375/)
  + [Visual Studio Code](https://code.visualstudio.com/)
  + [Visual Studio Code için Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  + [Azure Functions Core Tools](https://docs.microsoft.com/azure/azure-functions/functions-run-local#v2)
  + [Visual Studio Code için Azure İşlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu örnek, bir Web API 'SI barındırma kavramını göstermek için bir Azure Işlevi kullanır, ancak diğer yaklaşımlar mümkündür. Bilişsel [bir beceri için arabirim gereksinimlerini](cognitive-search-custom-skill-interface.md)karşıladığınızdan, aldığınız yaklaşım imsel olur. Ancak Azure Işlevleri, özel bir beceri oluşturmayı kolaylaştırır.

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Visual Studio Code'daki Azure İşlevleri proje şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. İşlev uygulaması, kaynakların yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

1. Visual Studio Code ' de, F1 tuşuna basarak komut paletini açın. Komut paletinde, araması yapın ve seçin `Azure Functions: Create new project...` .

1. Proje çalışma alanınız için bir dizin konumu seçin ve **Seç**' i seçin.

    > [!NOTE]
    > Bu adımlar, bir çalışma alanının dışında tamamlanacak şekilde tasarlandı. Bu nedenle, bir çalışma alanının parçası olan bir proje klasörü seçmeyin.

1. İşlev uygulaması projeniz için bir dil seçin. Bu öğretici için **Python**' ı seçin.
1. Python sürümünü seçin (sürüm 3.7.5 Azure Işlevleri tarafından desteklenir)
1. Projenizin ilk işlevi için bir şablon seçin. Yeni işlev uygulamasında HTTP tetiklemeli bir işlev oluşturmak için **http tetikleyicisi** ' ni seçin.
1. Bir işlev adı girin. Bu durumda, **Concatenator** kullanalım 
1. Yetkilendirme düzeyi olarak **işlev** ' ı seçin. Bu, işlevin HTTP uç noktasını çağırmak için bir [işlev anahtarı](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) sağlayacağız anlamına gelir. 
1. Projenizi nasıl açmak istediğinizi seçin. Bu adım için, geçerli çalışma alanında işlev uygulamasını oluşturmak üzere **çalışma alanına Ekle** ' yi seçin.

Visual Studio Code işlev uygulaması projesini yeni bir çalışma alanında oluşturur. Bu proje [host.json](../azure-functions/functions-host-json.md) ve [local.settings.json](../azure-functions/functions-run-local.md#local-settings-file) yapılandırma dosyaları ve tüm dile özgü proje dosyalarını içerir. 

İşlev uygulaması projesinin **Concatenator** klasöründe yenı bir http tetiklemeli işlev de oluşturulur. Bunun içinde \_ \_ , bu içerikle birlikte "init__. Kopyala" adlı bir dosya olacaktır:

```py
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )

```

Şimdi [özel beceri arabirimini](cognitive-search-custom-skill-interface.md)izlemek için bu kodu değiştirelim. Kodu aşağıdaki içerikle değiştirin:

```py
import logging
import azure.functions as func
import json

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    try:
        body = json.dumps(req.get_json())
    except ValueError:
        return func.HttpResponse(
             "Invalid body",
             status_code=400
        )
    
    if body:
        result = compose_response(body)
        return func.HttpResponse(result, mimetype="application/json")
    else:
        return func.HttpResponse(
             "Invalid body",
             status_code=400
        )


def compose_response(json_data):
    values = json.loads(json_data)['values']
    
    # Prepare the Output before the loop
    results = {}
    results["values"] = []
    
    for value in values:
        output_record = transform_value(value)
        if output_record != None:
            results["values"].append(output_record)
    return json.dumps(results, ensure_ascii=False)

## Perform an operation on a record
def transform_value(value):
    try:
        recordId = value['recordId']
    except AssertionError  as error:
        return None

    # Validate the inputs
    try:         
        assert ('data' in value), "'data' field is required."
        data = value['data']        
        assert ('text1' in data), "'text1' field is required in 'data' object."
        assert ('text2' in data), "'text2' field is required in 'data' object."
    except AssertionError  as error:
        return (
            {
            "recordId": recordId,
            "errors": [ { "message": "Error:" + error.args[0] }   ]       
            })

    try:                
        concatenated_string = value['data']['text1'] + " " + value['data']['text2']  
        # Here you could do something more interesting with the inputs

    except:
        return (
            {
            "recordId": recordId,
            "errors": [ { "message": "Could not complete operation for record." }   ]       
            })

    return ({
            "recordId": recordId,
            "data": {
                "text": concatenated_string
                    }
            })
```

**Transform_value** yöntemi tek bir kayıt üzerinde bir işlem gerçekleştirir. Yöntemi, özel gereksinimlerinizi karşılayacak şekilde değiştirebilirsiniz. Gerekli giriş doğrulamasını belirtmeyi ve kayıt için işlem tamamlanmayamıyorsanız oluşturulan hataları ve uyarıları döndürmeyi unutmayın.

### <a name="debug-your-code-locally"></a>Kodunuzda yerel olarak hata ayıklama

Visual Studio Code kodun hatalarını ayıklamanızı kolaylaştırır. ' F5 ' tuşuna basın veya **Hata Ayıkla** menüsüne gidin ve **hata ayıklamayı Başlat**' ı seçin.

Kod üzerinde herhangi bir kesme noktasını, ilgilendiğiniz satırdaki ' F9 ' öğesine vurarak ayarlayabilirsiniz.

Hata ayıklamaya başladıktan sonra işleviniz yerel olarak çalışır. Localhost 'a istek vermek için Postman veya Fiddler gibi bir aracı kullanabilirsiniz. Terminal penceresinde yerel uç noktanızın konumunu aklınızda edin. 

## <a name="publish-your-function"></a>İşlevinizi yayımlayın

İşlev davranışından memnun kaldığınızda, bunu yayımlayabilirsiniz.

1. Visual Studio Code ' de, F1 tuşuna basarak komut paletini açın. Komut paletinde, **işlev uygulaması Için Dağıt ve seç..**. öğesini seçin. 

1. Uygulamanızı dağıtmak istediğiniz Azure aboneliğini seçin.

1. **Azure 'da + yeni işlev uygulaması oluştur** seçeneğini belirleyin

1. İşlev uygulamanız için genel olarak benzersiz bir ad girin.

1. Python sürümü seçin (Bu işlev için Python 3.7. x çalışır).

1. Yeni kaynak için bir konum seçin (örneğin, Batı ABD 2).

Bu noktada, Azure 'da yeni Azure Işlevini barındırmak için Azure aboneliğinizde gerekli kaynaklar oluşturulacaktır. Dağıtımın tamamlanmasını bekleyin. Çıkış penceresinde, dağıtım sürecinin durumu gösterilir.

1. [Azure Portal](https://portal.azure.com), **tüm kaynaklar** ' a gidin ve adıyla yayımladığınız işlevi bulun. **Concatenator**olarak adlandırdıysanız, kaynağı seçin.

1. **</> işlev URL 'Si al** düğmesine tıklayın. Bu, işlevi çağırmak için URL 'YI kopyalamanızı sağlar.

## <a name="test-the-function-in-azure"></a>İşlevi Azure 'da test etme

Artık varsayılan ana bilgisayar anahtarına sahip olduğunuza göre, işlevinizi şu şekilde test edin:

```http
POST [Function URL you copied above]
```

### <a name="request-body"></a>İstek Gövdesi
```json
{
    "values": [
        {
            "recordId": "e1",
            "data":
            {
                "text1":  "Hello",
                "text2":  "World"
            }
        },
        {
            "recordId": "e2",
            "data": "This is an invalid input"
        }
    ]
}
```

Bu örnek, işlevi yerel ortamda çalıştırırken daha önce gördüğünüz sonucu üretmelidir.

## <a name="connect-to-your-pipeline"></a>İşlem hattınızla bağlantı kurmak

Artık yeni bir özel beceriye sahip olduğunuza göre, Beceri 'e ekleyebilirsiniz. Aşağıdaki örnekte, belgenin başlığını ve yazarını merged_title_author çağırdığımız tek bir alana birleştirme beceriye nasıl çağrılacağını gösterilmektedir. `[your-function-url-here]`Yeni Azure işlevinizin URL 'siyle değiştirin.

```json
{
    "skills": [
      "[... your existing skills remain here]",  
      {
        "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
        "description": "Our new search custom skill",
        "uri": "https://[your-function-url-here]",        
          "context": "/document/merged_content/organizations/*",
          "inputs": [
            {
              "name": "text1",
              "source": "/document/metadata_title"
            },
            {
              "name": "text2",
              "source": "/document/metadata_author"
            },
          ],
          "outputs": [
            {
              "name": "text",
              "targetName": "merged_title_author"
            }
          ]
      }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
Tebrikler! İlk özel becerinizi oluşturdunuz. Artık kendi özel işlevselliklerinizi eklemek için aynı kalıbı izleyebilirsiniz. Daha fazla bilgi edinmek için aşağıdaki bağlantılara tıklayın.

+ [Güç becerileri: özel yeteneklerin bir deposu](https://github.com/Azure-Samples/azure-search-power-skills)
+ [Bir AI zenginleştirme ardışık düzenine özel yetenek ekleme](cognitive-search-custom-skill-interface.md)
+ [Beceri tanımlama](cognitive-search-defining-skillset.md)
+ [Beceri oluşturma (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanları eşleme](cognitive-search-output-field-mapping.md)
