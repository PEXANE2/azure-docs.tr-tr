---
title: İçerik türlerini işleme
description: Logic Apps tasarım zamanında ve çalışma zamanında içerik türlerini nasıl işleyeceğinizi öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 07/20/2018
ms.openlocfilehash: 75d9285c4a838c2057c0f23841c3a2f465789c7c
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791525"
---
# <a name="handle-content-types-in-azure-logic-apps"></a>Azure Logic Apps içerik türlerini işleme

Çeşitli içerik türleri bir mantıksal uygulama aracılığıyla (örneğin, JSON, XML, düz dosyalar ve ikili veriler) akabilir. Logic Apps tüm içerik türlerini desteklese de, bazıları yerel desteğe sahiptir ve mantıksal uygulamalarınızda atama veya dönüştürme gerektirmez. Diğer türler gerektiğinde atama veya dönüştürme gerektirebilir. Bu makalede, Logic Apps içerik türlerini nasıl işleyeceği ve gerektiğinde bu türleri nasıl doğru şekilde dönüştürebileceğiniz ve dönüştürebileceğiniz açıklanır.

Logic Apps içerik türlerini işlemenin uygun yolunu öğrenmek için, HTTP çağrılarında `Content-Type` üst bilgi değerine bağlıdır, örneğin:

* [uygulama/JSON](#application-json) (yerel tür)
* [metin/düz](#text-plain) (yerel tür)
* [Uygulama/XML ve uygulama/sekizli-akış](#application-xml-octet-stream)
* [Diğer içerik türleri](#other-content-types)

<a name="application-json"></a>

## <a name="applicationjson"></a>uygulama/json

Logic Apps, *uygulama/JSON* içerik türüne sahip tüm Istekleri JavaScript GÖSTERIMI (JSON) nesnesi olarak depolar ve işler. Varsayılan olarak, herhangi bir atama olmadan JSON içeriğini ayrıştırabilirsiniz. "Application/JSON" içerik türüne sahip bir üst bilgisine sahip bir isteği ayrıştırmak için bir ifade kullanabilirsiniz. Bu örnek, `animal-type` dizisinden atama olmadan `dog` değer döndürür: 
 
`@body('myAction')['animal-type'][0]` 
  
  ```json
  {
    "client": {
       "name": "Fido",
       "animal-type": [ "dog", "cat", "rabbit", "snake" ]
    }
  }
  ```

Bir üst bilgi belirtmeyen JSON verileriyle çalışıyorsanız, [JSON () işlevini](../logic-apps/workflow-definition-language-functions-reference.md#json)kullanarak bu verileri JSON 'a el ile dönüştürebilirsiniz, örneğin: 
  
`@json(triggerBody())['animal-type']`

### <a name="create-tokens-for-json-properties"></a>JSON özellikleri için belirteçler oluşturma

Logic Apps, bu özellikleri mantıksal uygulamanızın iş akışında daha kolay bir şekilde başvurabilmeniz ve kullanabilmeniz için JSON içeriğindeki özellikleri temsil eden Kullanıcı dostu belirteçler oluşturma özelliğini sağlar.

* **İstek tetikleyicisi**

  Bu tetikleyiciyi Logic App Designer 'da kullandığınızda, almayı düşündüğünüz yükü açıklayan bir JSON şeması sağlayabilirsiniz. 
  Tasarımcı, bu şemayı kullanarak JSON içeriğini ayrıştırır ve JSON içeriğinizin özelliklerini temsil eden Kullanıcı dostu belirteçler oluşturur. 
  Daha sonra mantıksal uygulamanızın iş akışının tamamında bu özellikleri kolayca başvurabilir ve kullanabilirsiniz. 
  
  Bir şemanız yoksa şemayı oluşturabilirsiniz. 
  
  1. Istek tetikleyicisinde, **şema oluşturmak için örnek yük kullan**' ı seçin.  
  
  2. **Örnek BIR JSON yükü girin veya yapıştırın**, örnek bir yük sağlayın ve **bitti**' yi seçin. Örnek: 

     ![Örnek JSON yükü sağlama](./media/logic-apps-content-type/request-trigger.png)

     Oluşturulan şema şimdi Tetikleyiciniz içinde görüntülenir.

     ![Örnek JSON yükü sağlama](./media/logic-apps-content-type/generated-schema.png)

     Kod görünümü düzenleyicisinde Istek Tetikleyiciniz için temel alınan tanım aşağıdadır:

     ```json
     "triggers": { 
        "manual": {
           "type": "Request",
           "kind": "Http",
           "inputs": { 
              "schema": {
                 "type": "object",
                 "properties": {
                    "client": {
                       "type": "object",
                       "properties": {
                          "animal-type": {
                             "type": "array",
                             "items": {
                                "type": "string"
                             },
                          },
                          "name": {
                             "type": "string"
                          }
                       }
                    }
                 }
              }
           }
        }
     }
     ```

  3. İsteğiniz içinde bir `Content-Type` üst bilgisi eklediğinizden ve üstbilginin değerini `application/json`olarak ayarladığınızdan emin olun.

* **JSON eylemini Ayrıştır**

  Bu eylemi Logic App Designer 'da kullandığınızda JSON çıkışını ayrıştırarak JSON içeriğinizdeki özellikleri temsil eden Kullanıcı dostu belirteçler oluşturabilirsiniz. 
  Daha sonra mantıksal uygulamanızın iş akışının tamamında bu özellikleri kolayca başvurabilir ve kullanabilirsiniz. Istek tetikleyicisine benzer şekilde, ayrıştırmak istediğiniz JSON içeriğini açıklayan bir JSON şeması sağlayabilir veya oluşturabilirsiniz. 
  Böylece, Azure Service Bus, Azure Cosmos DB vb. verileri daha kolay bir şekilde kullanabilirsiniz.

  ![JSON Ayrıştır](./media/logic-apps-content-type/parse-json.png)

<a name="text-plain"></a>

## <a name="textplain"></a>metin/düz

Mantıksal uygulamanız `Content-Type` üst bilgisi `text/plain`olarak ayarlanmış HTTP iletileri aldığında, mantıksal uygulamanız bu iletileri ham biçimde depolar. Bu iletileri, atama olmadan sonraki eylemlere dahil ederseniz, istekler `Content-Type` üst bilgisi `text/plain`olarak ayarlanmış şekilde geçer. 

Örneğin, düz bir dosyayla çalışırken `Content-Type` üst bilgisi `text/plain` içerik türü olarak ayarlanmış bir HTTP isteği alabilirsiniz:

`Date,Name,Address`</br>
`Oct-1,Frank,123 Ave`

Daha sonra bu isteği başka bir isteğin gövdesi olarak sonraki bir eylemde gönderirseniz, örneğin `@body('flatfile')`, bu ikinci isteğin de `text/plain`ayarlanmış bir `Content-Type` üstbilgisi vardır. Düz metin olan ancak bir başlık belirtmeyen verilerle çalışıyorsanız, bu ifade gibi [String () işlevini](../logic-apps/workflow-definition-language-functions-reference.md#string) kullanarak bu verileri el ile metne çevirebilirsiniz: 

`@string(triggerBody())`

<a name="application-xml-octet-stream"></a>

## <a name="applicationxml-and-applicationoctet-stream"></a>Uygulama/XML ve uygulama/sekizli-akış

Logic Apps, alınan bir HTTP isteğindeki veya yanıttaki `Content-Type` her zaman korur. Mantıksal uygulamanız `application/octet-stream`olarak ayarlanan `Content-Type` içerik alırsa ve bu içeriği, atama olmadan sonraki bir eyleme eklerseniz, giden istek `Content-Type` `application/octet-stream`olarak ayarlanır. Bu şekilde Logic Apps, iş akışı boyunca taşırken verilerin kaybolmamasını garanti edebilir. Ancak, eylem durumu veya girişler ve çıktılar, durum iş akışı üzerinden hareket ederken bir JSON nesnesinde depolanır. 

## <a name="converter-functions"></a>Dönüştürücü işlevleri

Logic Apps, bazı veri türlerini korumak için, `$content` yükünü ve `$content-type`otomatik olarak dönüştürülen koruyan uygun meta verileri içeren ikili base64 kodlamalı bir dizeye içerik dönüştürür. 

Bu liste, bu [işlevleri](../logic-apps/workflow-definition-language-functions-reference.md)kullandığınızda Logic Apps içeriği nasıl dönüştüreceğini açıklar:

* `json()`: verileri `application/json` 'e yayınlar
* `xml()`: verileri `application/xml` 'e yayınlar
* `binary()`: verileri `application/octet-stream` 'e yayınlar
* `string()`: verileri `text/plain` 'e yayınlar
* `base64()`: içeriği Base64 kodlamalı bir dizeye dönüştürür
* `base64toString()`: Base64 kodlamalı dizeyi `text/plain` dönüştürür
* `base64toBinary()`: Base64 kodlamalı dizeyi `application/octet-stream` dönüştürür
* `dataUri()`: bir dizeyi veri URI 'sine dönüştürür
* `dataUriToBinary()`: bir veri URI 'sini ikili dizeye dönüştürür
* `dataUriToString()`: bir veri URI 'sini dizeye dönüştürür

Örneğin, `Content-Type` `application/xml`olarak ayarlandığı, bu içerik gibi bir HTTP isteği alırsanız:

```html
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

`xml()` ve `triggerBody()` işlevleriyle `@xml(triggerBody())` ifadesini kullanarak bu içeriği çevirebilirsiniz ve sonra bu içeriği daha sonra kullanabilirsiniz. Ya da `xpath()` ve `xml()` işlevleriyle `@xpath(xml(triggerBody()), '/CustomerName')` ifadesini kullanabilirsiniz. 

## <a name="other-content-types"></a>Diğer içerik türleri

Logic Apps ile birlikte çalışarak diğer içerik türlerini destekler, ancak `$content` değişkeninin kodunu çözerek ileti gövdesini el ile almanızı gerektirebilir.

Örneğin, mantıksal uygulamanızın `application/x-www-url-formencoded` içerik türüne sahip bir istek tarafından tetiklendiğini varsayalım. Tüm verileri korumak için, istek gövdesinde `$content` değişken Base64 dizesi olarak kodlanmış bir yüke sahiptir:

`CustomerName=Frank&Address=123+Avenue`

İstek düz metin veya JSON olmadığından, istek eylemde şu şekilde depolanır:

```json
"body": {
   "$content-type": "application/x-www-url-formencoded",
   "$content": "AAB1241BACDFA=="
}
```

Logic Apps, form verilerini işlemeye yönelik yerel işlevler sağlar, örneğin: 

* [triggerFormDataValue ()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue)
* [triggerFormDataMultiValues ()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues)
* [formDataValue ()](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) 
* [formDataMultiValues ()](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues)

Ya da bu örnek gibi bir ifade kullanarak verilere el ile erişebilirsiniz:

`@string(body('formdataAction'))` 

Giden isteğin aynı `application/x-www-url-formencoded` içerik türü başlığına sahip olmasını istiyorsanız, `@body('formdataAction')`gibi bir ifade kullanarak, isteği herhangi bir atama yapmadan eylemin gövdesine ekleyebilirsiniz. Ancak, bu yöntem yalnızca gövde `body` girişte tek parametre olduğunda işe yarar. Bir `application/json` isteğinde `@body('formdataAction')` ifadesini kullanmayı denerseniz, gövde kodlanmış olarak gönderildiği için bir çalışma zamanı hatası alırsınız.
