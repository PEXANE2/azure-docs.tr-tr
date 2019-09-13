---
title: HTTP isteklerine yanıt verme-Azure Logic Apps
description: Azure Logic Apps kullanarak HTTP üzerinden olaylara gerçek zamanlı olarak yanıt verin
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewers: klam, LADocs
manager: carmonm
ms.assetid: 566924a4-0988-4d86-9ecd-ad22507858c0
ms.topic: article
ms.date: 09/06/2019
tags: connectors
ms.openlocfilehash: 07f143b261d0cff9eba0d4b1803753446c311818
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70914368"
---
# <a name="respond-to-http-requests-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak HTTP isteklerine yanıt verme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik istek tetikleyicisi veya yanıtı EYLEMIYLE, http isteklerine gerçek zamanlı olarak alan ve yanıt veren otomatikleştirilmiş görevler ve iş akışları oluşturabilirsiniz. Örneğin, mantıksal uygulamanızı kullanabilirsiniz:

* Şirket içi bir veritabanındaki veriler için HTTP isteğine yanıt verme.
* Dış Web kancası olayı gerçekleştiğinde iş akışı tetikleyin.
* Başka bir mantıksal uygulamanın içinden bir mantıksal uygulama çağırın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolabilirsiniz](https://azure.microsoft.com/free/).

* [Logic Apps](../logic-apps/logic-apps-overview.md)hakkında temel bilgi. Logic Apps 'e yeni başladıysanız, [ilk mantıksal uygulamanızı oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

<a name="add-request"></a>

## <a name="add-a-request-trigger"></a>Istek tetikleyicisi ekleme

Bu yerleşik tetikleyici, gelen HTTP isteği alabilen el ile çağrılabilir bir uç nokta oluşturur. Bu olay gerçekleştiğinde tetikleyici ateşlenir ve mantıksal uygulamayı çalıştırır. Tetikleyicinin temel alınan JSON tanımı ve bu tetikleyiciyi çağırma hakkında daha fazla bilgi için, bkz. [istek tetikleme türü](../logic-apps/logic-apps-workflow-actions-triggers.md#request-trigger) ve [Azure Logic Apps içindeki HTTP uç noktalarına çağrı, tetikleyici veya iç içe geçme iş akışları](../logic-apps/logic-apps-http-endpoint.md)

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş bir mantıksal uygulama oluşturma.

1. Mantıksal uygulama Tasarımcısı açıldıktan sonra, arama kutusuna filtreniz olarak "http isteği" yazın. Tetikleyiciler listesinden, mantıksal uygulama iş akışınızın ilk adımı olan **BIR http isteği alındığında** tetiklenir ' ı seçin.

   ![HTTP istek tetikleyicisini seçme](./media/connectors-native-reqres/select-request-trigger.png)

   Istek tetikleyicisi şu özellikleri gösterir:

   ![İstek tetikleyicisi](./media/connectors-native-reqres/request-trigger.png)

   | Özellik adı | JSON Özellik adı | Gerekli | Açıklama |
   |---------------|--------------------|----------|-------------|
   | **HTTP POST URL 'SI** | seçim | Evet | Mantıksal uygulamayı kaydettikten sonra oluşturulan ve mantıksal uygulamanızı çağırmak için kullanılan uç nokta URL 'SI |
   | **İstek gövdesi JSON şeması** | `schema` | Hayır | Gelen HTTP istek gövdesindeki özellikleri ve değerleri açıklayan JSON şeması |
   |||||

1. **Istek GÖVDESI JSON şeması** kutusunda, isteğe bağlı olarak gelen istekteki http istek gövdesini açıklayan bir JSON şeması girin, örneğin:

   ![Örnek JSON şeması](./media/connectors-native-reqres/provide-json-schema.png)

   Tasarımcı, istekteki özelliklerin belirteçlerini oluşturmak için bu şemayı kullanır. Bu şekilde, mantıksal uygulamanız, iş akışınıza tetikleyici aracılığıyla istekten verileri ayrıştırtırabilir, tüketebilir ve geçirebilir.

   Örnek şema aşağıda verilmiştir:

   ```json
   {
      "type": "object",
      "properties": {
         "account": {
            "type": "object",
            "properties": {
               "name": {
                  "type": "string"
               },
               "ID": {
                  "type": "string"
               },
               "address": {
                  "type": "object",
                  "properties": {
                     "number": {
                        "type": "string"
                     },
                     "street": {
                        "type": "string"
                     },
                     "city": {
                        "type": "string"
                     },
                     "state": {
                        "type": "string"
                     },
                     "country": {
                        "type": "string"
                     },
                     "postalCode": {
                        "type": "string"
                     }
                  }
               }
            }
         }
      }
   }
   ```

   Bir JSON şeması girdiğinizde, tasarımcı, bu üstbilgiyi isteğinize eklemek `Content-Type` için bir anımsatıcı gösterir ve bu üst bilgi değerini olarak `application/json`ayarlar. Daha fazla bilgi için bkz. [içerik türlerini işleme](../logic-apps/logic-apps-content-type.md).

   !["Content-Type" üst bilgisini dahil etmek için anımsatıcı](./media/connectors-native-reqres/include-content-type.png)

   Bu üst bilgi, JSON biçiminde şöyle görünür:

   ```json
   {
      "Content-Type": "application/json"
   }
   ```

   Beklenen yük (veri) temelinde bir JSON şeması oluşturmak için, [JSONSchema.net](https://jsonschema.net)gibi bir araç kullanabilir veya aşağıdaki adımları izleyebilirsiniz:

   1. Istek tetikleyicisinde, **şema oluşturmak için örnek yük kullan**' ı seçin.

      ![Yük 'ten şema oluştur](./media/connectors-native-reqres/generate-from-sample-payload.png)

   1. Örnek yükü girin ve **bitti**' yi seçin.

      ![Yük 'ten şema oluştur](./media/connectors-native-reqres/enter-payload.png)

      Örnek yük aşağıda verilmiştir:

      ```json
      {
         "account": {
            "name": "Contoso",
            "ID": "12345",
            "address": { 
               "number": "1234",
               "street": "Anywhere Street",
               "city": "AnyTown",
               "state": "AnyState",
               "country": "USA",
               "postalCode": "11111"
            }
         }
      }
      ```

1. Ek özellikler belirtmek için **yeni parametre Ekle** listesini açın ve eklemek istediğiniz parametreleri seçin.

   | Özellik adı | JSON Özellik adı | Gerekli | Açıklama |
   |---------------|--------------------|----------|-------------|
   | **Yöntemi** | `method` | Hayır | Gelen isteğin mantıksal uygulamayı çağırmak için kullanması gereken Yöntem |
   | **Göreli yol** | `relativePath` | Hayır | Mantıksal uygulamanın uç nokta URL 'sinin kabul edebileceği parametrenin göreli yolu |
   |||||

   Bu örnek, **Yöntem** özelliğini ekler:

   ![Yöntem parametresi Ekle](./media/connectors-native-reqres/add-parameters.png)

   **Yöntem** özelliği tetikleyicide görünür, böylece listeden bir yöntem seçebilirsiniz.

   ![Yöntem seçin](./media/connectors-native-reqres/select-method.png)

1. Şimdi, iş akışınızda sonraki adım olarak başka bir eylem ekleyin. Tetikleyici altında, eklemek istediğiniz eylemi bulabilmeniz için bir **sonraki adım** ' ı seçin.

   Örneğin, özel bir yanıt döndürmek için kullanabileceğiniz ve bu konunun ilerleyen kısımlarında açıklanan [bir yanıt eylemi ekleyerek](#add-response)isteğe yanıt verebilirsiniz.

   Mantıksal uygulamanız gelen isteği yalnızca bir dakika boyunca açık tutar. Mantıksal uygulama iş akışınızın bir yanıt eylemi içerdiğini varsayarsak, mantıksal uygulama bu süre geçtikten sonra bir yanıt döndürmezse, mantıksal uygulamanız bir `504 GATEWAY TIMEOUT` arayana döndürür. Aksi takdirde, mantıksal uygulamanız bir yanıt eylemi içermiyorsa, mantıksal uygulamanız hemen çağırana bir `202 ACCEPTED` yanıt döndürür.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin. 

   Bu adım, mantıksal uygulamayı tetikleyen isteği göndermek için kullanılacak URL 'YI oluşturur. Bu URL 'yi kopyalamak için URL 'nin yanındaki Kopyala simgesini seçin.

   ![Mantıksal uygulamanızı tetiklemenin kullanılacağı URL](./media/connectors-native-reqres/generated-url.png)

1. Mantıksal uygulamanızı tetiklemek için, oluşturulan URL 'ye bir HTTP GÖNDERISI gönderin. Örneğin [Postman](https://www.getpostman.com/)gibi bir araç kullanabilirsiniz.

### <a name="trigger-outputs"></a>Tetikleme çıkışları

Istek tetikleyicisinden alınan çıktılar hakkında daha fazla bilgi aşağıda verilmiştir:

| JSON Özellik adı | Veri türü | Açıklama |
|--------------------|-----------|-------------|
| `headers` | Object | İstekten üstbilgileri açıklayan bir JSON nesnesi |
| `body` | Object | İstekten gelen gövde içeriğini açıklayan bir JSON nesnesi |
||||

<a name="add-response"></a>

## <a name="add-a-response-action"></a>Yanıt eylemi ekleme

Gelen HTTP isteğine bir yük (veri) ile yanıt vermek için yanıt eylemini kullanabilirsiniz, ancak yalnızca bir HTTP isteği tarafından tetiklenen bir mantıksal uygulamada. Yanıt eylemini iş akışınızın herhangi bir noktasına ekleyebilirsiniz. Bu tetikleyicinin temel alınan JSON tanımı hakkında daha fazla bilgi için bkz. [yanıt eylem türü](../logic-apps/logic-apps-workflow-actions-triggers.md#response-action).

Mantıksal uygulamanız gelen isteği yalnızca bir dakika boyunca açık tutar. Mantıksal uygulama iş akışınızın bir yanıt eylemi içerdiğini varsayarsak, mantıksal uygulama bu süre geçtikten sonra bir yanıt döndürmezse, mantıksal uygulamanız bir `504 GATEWAY TIMEOUT` arayana döndürür. Aksi takdirde, mantıksal uygulamanız bir yanıt eylemi içermiyorsa, mantıksal uygulamanız hemen çağırana bir `202 ACCEPTED` yanıt döndürür.

1. Mantıksal uygulama Tasarımcısı ' nda, yanıt eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Örneğin, daha önce Istek tetikleyicisi kullanımı:

   ![Yeni adım Ekle](./media/connectors-native-reqres/add-response.png)

   Adımlar arasında bir eylem eklemek için, işaretçinizi Bu adımlar arasındaki oka taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. **Eylem seçin**altında, arama kutusuna filtreniz olarak "yanıt" yazın ve **Yanıt** eylemini seçin.

   ![Yanıt eylemini seçin](./media/connectors-native-reqres/select-response-action.png)

   Istek tetikleyicisi kolaylık sağlaması için bu örnekte daraltılır.

1. Yanıt iletisi için gereken tüm değerleri ekleyin. 

   Bazı alanlarda, kutularının içine tıklanması dinamik içerik listesini açar. Daha sonra iş akışındaki önceki adımlardan bulunan mevcut çıkışları temsil eden belirteçleri seçebilirsiniz. Önceki örnekte belirtilen şemadan özellikler artık dinamik içerik listesinde görünür.

   Örneğin, **üst bilgiler** kutusu için, anahtar adı `Content-Type` olarak ekleyin ve anahtar değerini `application/json` bu konunun önceki kısımlarında belirtildiği gibi olarak ayarlayın. **Gövde** kutusu için dinamik içerik listesinden tetikleyiciyi ilet çıktısını seçebilirsiniz.

   ![Yanıt eylemi ayrıntıları](./media/connectors-native-reqres/response-details.png)

   Üst bilgileri JSON biçiminde görüntülemek için **metin görünümüne geç**' i seçin.

   ![Üstbilgiler-metin görünümüne geç](./media/connectors-native-reqres/switch-to-text-view.png)

   Yanıt eyleminde ayarlayabileceğiniz özellikler hakkında daha fazla bilgi bulabilirsiniz. 

   | Özellik adı | JSON Özellik adı | Gerekli | Açıklama |
   |---------------|--------------------|----------|-------------|
   | **Durum Kodu** | `statusCode` | Evet | Yanıtta döndürülecek HTTP durum kodu |
   | **Üst bilgileri** | `headers` | Hayır | Yanıta eklenecek bir veya daha fazla üstbilgiyi açıklayan bir JSON nesnesi |
   | **Gövde** | `body` | Hayır | Yanıt gövdesi |
   |||||

1. Yanıt gövdesi için JSON şeması gibi ek özellikler belirtmek için **yeni parametre Ekle** listesini açın ve eklemek istediğiniz parametreleri seçin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)