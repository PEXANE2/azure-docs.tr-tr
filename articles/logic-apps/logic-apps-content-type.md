---
title: İçerik türlerini işleme
description: Azure Logic Apps'ta tasarım süresi ve çalışma süresi boyunca iş akışlarında çeşitli içerik türlerini nasıl işleyeceğinizi öğrenin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 07/20/2018
ms.openlocfilehash: ae0abe288edda2ce01311d8533b1f104409efce0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666882"
---
# <a name="handle-content-types-in-azure-logic-apps"></a>Azure Logic Apps'ta içerik türlerini işleme

JSON, XML, düz dosyalar ve ikili veriler gibi çeşitli içerik türleri bir mantık uygulamasından akabilir. Logic Apps tüm içerik türlerini desteklerken, bazıları yerel desteğe sahiptir ve mantık uygulamalarınızda döküm veya dönüştürme gerektirmez. Diğer türler gerektiğinde döküm veya dönüştürme gerektirebilir. Bu makalede, Logic Apps'ın içerik türlerini nasıl işlediği ve gerektiğinde bu türleri nasıl doğru şekilde atabileceğiniz veya dönüştürebileceğiniz açıklanmaktadır.

İçerik türlerini işlemenin uygun yolunu belirlemek için, `Content-Type` Logic Apps, örneğin HTTP aramalarında üstbilgi değerine dayanır:

* [uygulama/json](#application-json) (yerli tip)
* [metin/düz](#text-plain) (yerel tür)
* [uygulama/xml ve uygulama/sekizli-akarsu](#application-xml-octet-stream)
* [Diğer içerik türleri](#other-content-types)

<a name="application-json"></a>

## <a name="applicationjson"></a>uygulama/json

Logic Apps, javascript gösterimi (JSON) nesnesi olarak *uygulama/json* içerik türüyle istediğiniz isteği saklar ve işler. Varsayılan olarak, herhangi bir döküm olmadan JSON içeriği ayrıştın. "Uygulama/json" içerik türüne sahip üstbilgisi olan bir isteği ayrıştırmak için bir ifade kullanabilirsiniz. Bu örnek, `dog` döküm `animal-type` olmadan diziden değeri döndürür: 
 
`@body('myAction')['animal-type'][0]` 
  
  ```json
  {
    "client": {
       "name": "Fido",
       "animal-type": [ "dog", "cat", "rabbit", "snake" ]
    }
  }
  ```

Üstbilgi belirtmeyen JSON verileriyle çalışıyorsanız, örneğin [json() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#json)kullanarak bu verileri el ile JSON'a atabilirsiniz: 
  
`@json(triggerBody())['animal-type']`

### <a name="create-tokens-for-json-properties"></a>JSON özellikleri için belirteçler oluşturma

Logic Apps, json içeriğindeki özellikleri temsil eden kullanıcı dostu belirteçler oluşturma olanağı sağlar, böylece bu özelliklere mantık uygulamanızın iş akışında daha kolay başvurup kullanabilirsiniz.

* **İstek tetikleyicisi**

  Bu tetikleyiciyi Logic App Designer'da kullandığınızda, almayı beklediğiniz yükü açıklayan bir JSON şeması sağlayabilirsiniz. 
  Tasarımcı bu şemayı kullanarak JSON içeriğini ayrıştirır ve JSON içeriğinizdeki özellikleri temsil eden kullanıcı dostu belirteçler oluşturur. 
  Daha sonra mantık uygulamanızın iş akışı boyunca bu özelliklere kolayca başvurup kullanabilirsiniz. 
  
  Şema yoksa, şema oluşturabilirsiniz. 
  
  1. İstek tetikleyicisinde **şema oluşturmak için örnek yükü kullan'ı**seçin.  
  
  2. **Örnek bir JSON yükünü girin veya yapıştırın,** örnek bir yük sağlayın ve **ardından Bitti'yi**seçin. Örnek: 

     ![Örnek JSON yükü sağlayın](./media/logic-apps-content-type/request-trigger.png)

     Oluşturulan şema artık tetikleyicinizde görünüyor.

     ![Örnek JSON yükü sağlayın](./media/logic-apps-content-type/generated-schema.png)

     Kod görünümü düzenleyicisinde İstek tetikleyicinizin temel tanımı aşağıda verilmiştir:

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

  3. İsteğinize, bir `Content-Type` üstbilgi eklediğinizden ve üstbilginin `application/json`değerini ' e ayarladığınızdan emin olun.

* **Parse JSON eylem**

  Bu eylemi Logic App Designer'da kullandığınızda, JSON çıktısını ayrıştırabilir ve JSON içeriğinizdeki özellikleri temsil eden kullanıcı dostu belirteçler oluşturabilirsiniz. 
  Daha sonra mantık uygulamanızın iş akışı boyunca bu özelliklere kolayca başvurup kullanabilirsiniz. İstek tetikleyicisine benzer şekilde, ayrışdırmak istediğiniz JSON içeriğini açıklayan bir JSON şeması sağlayabilir veya oluşturabilirsiniz. 
  Bu şekilde, Azure Hizmet Veri Yolu, Azure Cosmos DB ve benzeri verileri daha kolay tüketebilirsiniz.

  ![JSON Ayrıştır](./media/logic-apps-content-type/parse-json.png)

<a name="text-plain"></a>

## <a name="textplain"></a>metin/düz

Mantık uygulamanız `Content-Type` üstbilgi ayarlanmış HTTP iletilerini `text/plain`aldığında, mantık uygulamanız bu iletileri ham biçimde saklar. Bu iletileri döküm olmadan sonraki eylemlere eklerseniz, `Content-Type` istekler `text/plain`üstbilgi ile dışarı gitmek . 

Örneğin, düz bir dosyayla çalışırken, `Content-Type` üstbilgi içerik türüne `text/plain` ayarlanmış bir HTTP isteği alabilirsiniz:

`Date,Name,Address`</br>
`Oct-1,Frank,123 Ave`

Bu isteği daha sonraki bir eylemde başka bir istek için `@body('flatfile')`gövde olarak gönderirseniz, `Content-Type` örneğin, bu ikinci `text/plain`isteğin de ayarlanmış bir üstbilgisi vardır. Düz metin olan ancak üstbilgi belirtmemiş verilerle çalışıyorsanız, bu ifade gibi [dize() işlevini](../logic-apps/workflow-definition-language-functions-reference.md#string) kullanarak bu verileri metne el ile atabilirsiniz: 

`@string(triggerBody())`

<a name="application-xml-octet-stream"></a>

## <a name="applicationxml-and-applicationoctet-stream"></a>uygulama/xml ve uygulama/sekizli-akarsu

Logic Apps her `Content-Type` zaman alınan bir HTTP isteği veya yanıtını korur. Bu nedenle, mantık uygulamanız `Content-Type` `application/octet-stream`ayarlanmış bir içerik alıyorsa ve bu içeriği daha sonraki `Content-Type` bir `application/octet-stream`eyleme oyuncu seçimi yapmadan dahil ederseniz, giden istek de . Bu şekilde, Logic Apps iş akışı nda hareket ederken verilerin kaybolmadığını garanti edebilir. Ancak, durum iş akışı boyunca hareket ederken eylem durumu veya girdiler igiriş ve çıktılar, bir JSON nesnesinde depolanır. 

## <a name="converter-functions"></a>Dönüştürücü işlevleri

Bazı veri türlerini korumak için, Logic Apps içeriği hem yükü `$content` `$content-type`hem de otomatik olarak dönüştürülen uygun meta verileri içeren ikili base64 kodlu bir dize dönüştürür. 

Bu liste, bu [işlevleri](../logic-apps/workflow-definition-language-functions-reference.md)kullandığınızda Logic Apps'ın içeriği nasıl dönüştürdüğünü açıklar:

* `json()`: Verileri`application/json`
* `xml()`: Verileri`application/xml`
* `binary()`: Verileri`application/octet-stream`
* `string()`: Verileri`text/plain`
* `base64()`: İçeriği base64 kodlanmış bir dize dönüştürür
* `base64toString()`: Base64 kodlanmış bir dizeyi`text/plain`
* `base64toBinary()`: Base64 kodlanmış bir dizeyi`application/octet-stream`
* `dataUri()`: Bir dizeyi veri URI'sine dönüştürür
* `dataUriToBinary()`: Bir veriyi URI'yi ikili bir dize dönüştürür
* `dataUriToString()`: Bir veri URI'yi bir dize dönüştürür

Örneğin, bu içerik gibi ayarlandığı `Content-Type` `application/xml`yerde bir HTTP isteği alırsanız:

```html
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Bu içeriği, ifadeyi `@xml(triggerBody())` `xml()` ve `triggerBody()` işlevleri kullanarak kullanabilir ve daha sonra bu içeriği kullanabilirsiniz. Veya, ifadeyi `@xpath(xml(triggerBody()), '/CustomerName')` `xpath()` ve `xml()` işlevleri ile kullanabilirsiniz. 

## <a name="other-content-types"></a>Diğer içerik türleri

Logic Apps diğer içerik türleri ile çalışır ve destekler, ancak `$content` değişkeni çözerek ileti gövdesini el ile almanızı gerektirebilir.

Örneğin, mantık uygulamanızın içerik türüne `application/x-www-url-formencoded` sahip bir istek tarafından tetiklendiğini varsayalım. Tüm verileri korumak için, istek gövdesindeki `$content` değişkenin base64 dizesi olarak kodlanmış bir yükü vardır:

`CustomerName=Frank&Address=123+Avenue`

İstek düz metin veya JSON olmadığından, istek eylemde aşağıdaki gibi depolanır:

```json
"body": {
   "$content-type": "application/x-www-url-formencoded",
   "$content": "AAB1241BACDFA=="
}
```

Logic Apps, form verilerini işlemek için yerel işlevler sağlar, örneğin: 

* [tetikFormDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataValue)
* [tetikformdataMultivalues()](../logic-apps/workflow-definition-language-functions-reference.md#triggerFormDataMultiValues)
* [formDataValue()](../logic-apps/workflow-definition-language-functions-reference.md#formDataValue) 
* [formDataMultiValues()](../logic-apps/workflow-definition-language-functions-reference.md#formDataMultiValues)

Veya, aşağıdaki örnek gibi bir ifade kullanarak verilere el ile erişebilirsiniz:

`@string(body('formdataAction'))` 

Giden isteğin aynı `application/x-www-url-formencoded` içerik türü üstbilgisine sahip olmasını istiyorsanız, '' gibi `@body('formdataAction')`bir ifade kullanarak herhangi bir döküm yapmadan isteği eylemin gövdesine ekleyebilirsiniz. Ancak, bu yöntem yalnızca girişteki `body` tek parametre gövde olduğunda çalışır. Bir `@body('formdataAction')` `application/json` istekteki ifadeyi kullanmaya çalışırsanız, gövde kodlanmış olarak gönderildiğinden çalışma zamanı hatası alırsınız.
