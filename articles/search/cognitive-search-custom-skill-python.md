---
title: Özel beceri örneği (Python)
titleSuffix: Azure Cognitive Search
description: Python geliştiricileri için Azure İşlevleri ve Visual Studio'yu kullanarak özel bir beceri oluşturma araçlarını ve tekniklerini öğrenin. Özel beceriler, Azure Bilişsel Arama'da AI ile zenginleştirilmiş dizin oluşturma ardışık bir ardışık boru hattına ekleyebileceğiniz kullanıcı tanımlı modeller veya mantık içerir.
manager: nitinme
author: luiscabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: fc69761a05ea381d39d58d5ebf0046e0d9874961
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77210474"
---
# <a name="example-create-a-custom-skill-using-python"></a>Örnek: Python kullanarak özel bir beceri oluşturun

Bu Azure Bilişsel Arama skillset örneğinde, Python ve Visual Studio Code'u kullanarak web API özel becerisi oluşturmayı öğreneceksiniz. Örnek, [özel beceri arabirimini](cognitive-search-custom-skill-interface.md)uygulayan bir [Azure İşlevi](https://azure.microsoft.com/services/functions/) kullanır.

Python'da özel beceri geliştirme için kullanılan araçlara ve teknolojilere odaklanabilmeniz için özel beceri tasarımı basittir (iki dizeleri birleştirir). Basit bir beceri ile başarılı olduktan sonra, daha karmaşık senaryolar ile dallanma olabilir.

## <a name="prerequisites"></a>Ön koşullar

+ Özel bir becerinin uygulaması gereken giriş/çıkış arabirimine giriş için [özel beceri arabirimini](cognitive-search-custom-skill-interface.md) gözden geçirin.

+ Ortamınızı ayarlayın. Visual Studio Code ve Python uzantılarını kullanarak sunucusuz Azure İşi'ni kurmak için [bu öğreticiyi uçlardan uca](https://docs.microsoft.com/azure/python/tutorial-vs-code-serverless-python-01) izledik. Öğretici, aşağıdaki araçların ve bileşenlerin yüklenmesi nde size yol açar: 

  + [Piton 3.75](https://www.python.org/downloads/release/python-375/)
  + [Visual Studio Code](https://code.visualstudio.com/)
  + [Visual Studio Code için Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  + [Azure İşlevler Temel Araçları](https://docs.microsoft.com/azure/azure-functions/functions-run-local#v2)
  + [Visual Studio Code için Azure İşlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)

## <a name="create-an-azure-function"></a>Azure İşlevi oluşturma

Bu örnek, bir web API barındırma kavramını göstermek için bir Azure İşlevi kullanır, ancak diğer yaklaşımlar mümkündür. Bilişsel bir beceri [için arayüz gereksinimlerini](cognitive-search-custom-skill-interface.md)karşıladığınız sürece, aldığınız yaklaşım önemsizdir. Ancak Azure İşlevler, özel bir beceri oluşturmayı kolaylaştırır.

### <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Visual Studio Code'daki Azure İşlevleri proje şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. İşlev uygulaması, kaynakların yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.

1. Visual Studio Code'da komut paletini açmak için F1 tuşuna basın. Komut paletinde, 'yi `Azure Functions: Create new project...`arayın ve seçin.

1. Proje çalışma alanınız için bir dizin konumu seçin ve **Seç'i**seçin.

    > [!NOTE]
    > Bu adımlar, bir çalışma alanı dışında tamamlanacak şekilde tasarlanmıştır. Bu nedenle, çalışma alanının parçası olan bir proje klasörü seçmeyin.

1. İşlev uygulaması projeniz için bir dil seçin. Bu öğretici için **Python'u**seçin.
1. Python sürümünü seçin (sürüm 3.7.5 Azure Fonksiyonları tarafından desteklenir)
1. Projenizin ilk işlevi için bir şablon seçin. Yeni işlev uygulamasında HTTP tetiklenmiş bir işlev oluşturmak için **HTTP tetikleyicisini** seçin.
1. Bir işlev adı sağlayın. Bu durumda, **Concatenator** kullanalım 
1. Yetkilendirme düzeyi olarak **İşlev'i** seçin. Bu, işlevin HTTP bitiş noktasını aramak için bir [işlev anahtarı](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) sağlayacağımız anlamına gelir. 
1. Projenizi nasıl açmak istediğinizi seçin. Bu adım için, geçerli çalışma alanında işlev uygulamasını oluşturmak için **çalışma alanına Ekle'yi** seçin.

Visual Studio Code işlev uygulaması projesini yeni bir çalışma alanında oluşturur. Bu proje [host.json](../azure-functions/functions-host-json.md) ve [local.settings.json](../azure-functions/functions-run-local.md#local-settings-file) yapılandırma dosyaları ve tüm dile özgü proje dosyalarını içerir. 

İşlev uygulaması projesinin **Concatenator** klasöründe yeni bir HTTP tetikleme işlevi de oluşturulur. İçinde "init__.py"\_\_adında bir dosya olacak ve bu içerik le birlikte:

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

Şimdi bu kodu özel beceri [arabirimini](cognitive-search-custom-skill-interface.md)izleyecek şekilde değiştirelim). Kodu aşağıdaki içerikle değiştirin:

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

**transform_value** yöntemi tek bir kayıt üzerinde bir işlem gerçekleştirir. Yöntemi, özel gereksinimlerinizi karşılamak için değiştirebilirsiniz. Gerekli giriş doğrulaması yapmayı ve işlem kayıt için tamamlanamadıysa üretilen hataları ve uyarıları döndürmeyi unutmayın.

### <a name="debug-your-code-locally"></a>Kodunuzu yerel olarak hata ayıklama

Visual Studio Code, kodu niçin hata ayıklamayı kolaylaştırır. 'F5' tuşuna basın veya **Hata Ayıklama** menüsüne gidin ve **Hata Ayıklama başlat'ı**seçin.

İlgi satırında 'F9' tuşuna basarak koddaki herhangi bir kesme noktasını ayarlayabilirsiniz.

Hata ayıklama başladıktan sonra, işleviniz yerel olarak çalışacaktır. İsteği localhost'a vermek için Postacı veya Fiddler gibi bir aracı kullanabilirsiniz. Terminal penceresinde yerel bitiş noktanızın konumuna dikkat edin. 

## <a name="publish-your-function"></a>İşlevinizi yayımlayın

İşlev davranışından memnun olduğunuzda, yayımlayabilirsiniz.

1. Visual Studio Code'da komut paletini açmak için F1 tuşuna basın. Komut paletinde, **İşlev Uygulamasına Dağıt'ı**arayın ve seçin... . 

1. Uygulamanızı dağıtmak istediğiniz Azure Aboneliği'ni seçin.

1. **Azure'da Yeni İşlev Uygulaması** Seçin + Oluştur

1. İşlev uygulamanız için genel olarak benzersiz bir ad girin.

1. Python sürümünü seçin (Python 3.7.x bu işlev için çalışır).

1. Yeni kaynak için bir konum seçin (örneğin, Batı ABD 2).

Bu noktada, yeni Azure İşlevi'ni Azure'da barındırmak için Azure aboneliğinizde gerekli kaynaklar oluşturulur. Dağıtımın tamamlanmasını bekleyin. Çıkış penceresi, dağıtım işleminin durumunu gösterir.

1. Azure [portalında](https://portal.azure.com)Tüm **Kaynaklar'a** gidin ve adıyla yayınladığınız işlevi arayın. **Concatenator**adını verdiyseniz, kaynağı seçin.

1. **İşlev URL'sini</> Alın** düğmesini tıklatın. Bu, işlevi aramak için URL'yi kopyalamanızı sağlar.

## <a name="test-the-function-in-azure"></a>Azure'da işlevi test edin

Varsayılan ana bilgisayar anahtarına sahip olduğunuza göre, işlevinizi aşağıdaki gibi sınayın:

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

Bu örnek, yerel ortamda işlevi çalıştırırken daha önce gördüğünüz aynı sonucu üretmelidir.

## <a name="connect-to-your-pipeline"></a>Ardınıza bağlanın

Şimdi yeni bir özel beceri var, sizin skillset ekleyebilirsiniz. Aşağıdaki örnekte, başlığı ve belgenin Yazarını merged_title_author dediğimiz tek bir alana dönüştürme becerisini nasıl çağırabileceğinizi gösterir. Yeni `[your-function-url-here]` Azure İşlevinizin URL'siyle değiştirin.

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
Tebrikler! İlk özel yeteneğini yarattın. Artık kendi özel işlevselliğinizi eklemek için aynı deseni takip edebilirsiniz. Daha fazla bilgi için aşağıdaki bağlantıları tıklatın.

+ [Güç Becerileri: özel becerilerin deposu](https://github.com/Azure-Samples/azure-search-power-skills)
+ [AI zenginleştirme hattına özel bir beceri ekleme](cognitive-search-custom-skill-interface.md)
+ [Bir skillset nasıl tanımlanır?](cognitive-search-defining-skillset.md)
+ [Skillset (REST) oluşturun](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Zenginleştirilmiş alanların haritası nasıl eşlenir?](cognitive-search-output-field-mapping.md)
