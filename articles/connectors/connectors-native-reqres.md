---
title: HTTPS kullanarak çağrıları alma ve yanıtla
description: Azure Logic Apps'ı kullanarak harici hizmetlerden gelen HTTPS isteklerini işleme
services: logic-apps
ms.suite: integration
ms.reviewers: klam, logicappspm
ms.topic: conceptual
ms.date: 03/12/2020
tags: connectors
ms.openlocfilehash: 1885d7f8713b3801ce0c9846b7a8509b3864032a
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656295"
---
# <a name="receive-and-respond-to-inbound-https-requests-in-azure-logic-apps"></a>Azure Logic Apps'ta gelen HTTPS isteklerini alma ve yanıtlama

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik İstek tetikleyici veya Yanıt eylemi yle, gelen HTTPS isteklerini alan ve yanıtlayan otomatik görevler ve iş akışları oluşturabilirsiniz. Örneğin, mantık uygulamanıza sahip olabilirsiniz:

* Şirket içi bir veritabanındaki veri için HTTPS isteğini alın ve yanıtlayın.
* Harici bir webhook olayı olduğunda bir iş akışını tetikle.
* Başka bir mantık uygulamasından bir HTTPS çağrısı alın ve yanıt verin.

> [!NOTE]
> İstek tetikleyicisi, gelen aramalar için *yalnızca* Aktarım Katmanı Güvenliği (TLS) 1.2'yi destekler. Giden aramalar TLS 1.0, 1.1 ve 1.2'yi desteklemeye devam eder. Daha fazla bilgi için [TLS 1.0 sorununu çözme ye](https://docs.microsoft.com/security/solving-tls1-problem)bakın.
>
> TLS el sıkışma hataları görürseniz, TLS 1.2 kullandığınızdan emin olun. Gelen aramalar için desteklenen şifreleme paketleri şunlardır:
>
> * TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
> * TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
> * TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
> * TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
> * TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
> * TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
> * TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
> * TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz [yoksa, ücretsiz bir Azure hesabına](https://azure.microsoft.com/free/)kaydolabilirsiniz.

* [Mantık uygulamaları](../logic-apps/logic-apps-overview.md)hakkında temel bilgi . Mantık uygulamalarında yeniyseniz, [ilk mantık uygulamanızı nasıl oluşturabileceğinizi](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

<a name="add-request"></a>

## <a name="add-request-trigger"></a>İstek tetikleyicisi ekle

Bu yerleşik tetikleyici, *yalnızca* gelen HTTPS isteklerini alabilen, el ile çağrılabilir bir HTTPS bitiş noktası oluşturur. Bu olay gerçekleştiğinde, tetikleyici mantık uygulamasını çalıştırZve çalıştırZ. Tetikleyicinin temel JSON tanımı ve bu tetikleyicinin nasıl çağrılması hakkında daha fazla bilgi için, Azure Mantık Apps'taki HTTP uç noktalarıyla [İstek tetikleyici türü](../logic-apps/logic-apps-workflow-actions-triggers.md#request-trigger) ve [Çağrı, tetikleme veya yuva iş akışlarına](../logic-apps/logic-apps-http-endpoint.md)bakın.

1. [Azure portalında](https://portal.azure.com)oturum açın. Boş bir mantıksal uygulama oluşturma.

1. Logic App Designer açıldıktan sonra, arama kutusuna filtreniz olarak "http request" girin. Tetikleyiciler listesinden, mantık uygulaması iş akışınızın ilk adımı olan **BIR HTTP isteği nin ne zaman alındığını** seçin.

   ![İstek tetikleyicisi seçin](./media/connectors-native-reqres/select-request-trigger.png)

   İstek tetikleyicisi şu özellikleri gösterir:

   ![İstek tetikleyicisi](./media/connectors-native-reqres/request-trigger.png)

   | Özellik adı | JSON özellik adı | Gerekli | Açıklama |
   |---------------|--------------------|----------|-------------|
   | **HTTP SONRASı URL** | {yok} | Evet | Mantık uygulamasını kurtardıktan sonra oluşturulan ve mantık uygulamanızı aramak için kullanılan uç nokta URL'si |
   | **İstek Vücut JSON Schema** | `schema` | Hayır | Gelen istek gövdesindeki özellikleri ve değerleri açıklayan JSON şeması |
   |||||

1. İstek **Gövdesi JSON Schema** kutusunda, isteğe bağlı olarak gelen istekte gövdeyi açıklayan bir JSON şeması girin, örneğin:

   ![Örnek JSON şema](./media/connectors-native-reqres/provide-json-schema.png)

   Tasarımcı, istekteki özellikler için belirteçler oluşturmak için bu şemayı kullanır. Bu şekilde, mantık uygulamanız istekteki verileri tetiğinizden iş akışınıza ayrıştabilir, tüketebilir ve aktarabilir.

   İşte örnek şema:

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

   Bir JSON şeması girdiğinizde, tasarımcı isteğinize `Content-Type` üstbilgi eklemek ve bu üstbilgi değerini `application/json`.'ye ayarlamak için bir anımsatıcı gösterir. Daha fazla bilgi için [bkz.](../logic-apps/logic-apps-content-type.md)

   !["İçerik Türü" üstbilgisini ekleme anımsatıcı](./media/connectors-native-reqres/include-content-type.png)

   Bu üstbilgi JSON formatında şu şekilde görünür:

   ```json
   {
      "Content-Type": "application/json"
   }
   ```

   Beklenen yükü (verileri) temel alan bir JSON şeması oluşturmak [için, JSONSchema.net](https://jsonschema.net)gibi bir araç kullanabilir veya aşağıdaki adımları izleyebilirsiniz:

   1. İstek tetikleyicisinde **şema oluşturmak için örnek yükü kullan'ı**seçin.

      ![Yükten şema oluşturma](./media/connectors-native-reqres/generate-from-sample-payload.png)

   1. Örnek yükü girin ve **Bitti'yi**seçin.

      ![Yükten şema oluşturma](./media/connectors-native-reqres/enter-payload.png)

      Örnek yük aşağıda veda edebilirsiniz:

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

1. Ek özellikler belirtmek için **yeni parametre** ekle listesini açın ve eklemek istediğiniz parametreleri seçin.

   | Özellik adı | JSON özellik adı | Gerekli | Açıklama |
   |---------------|--------------------|----------|-------------|
   | **Yöntem** | `method` | Hayır | Gelen isteğin mantık uygulamasını aramak için kullanması gereken yöntem |
   | **Göreli yol** | `relativePath` | Hayır | Mantık uygulamasının bitiş noktası URL'sinin kabul edebileceği parametrenin göreli yolu |
   |||||

   Bu örnek, **Yöntem** özelliğini ekler:

   ![Yöntem parametresi ekle](./media/connectors-native-reqres/add-parameters.png)

   Yöntem **Method** özelliği, listeden bir yöntem seçebilmeniz için tetikleyicide görünür.

   ![Yöntem seçin](./media/connectors-native-reqres/select-method.png)

1. Şimdi, iş akışınızdaki bir sonraki adım olarak başka bir eylem ekleyin. Tetikleyicinin altında, eklemek istediğiniz eylemi bulabilmeniz için **Sonraki adım'ı** seçin.

   Örneğin, özelleştirilmiş bir yanıtı döndürmek için kullanabileceğiniz ve daha sonra bu konuda açıklanan [bir Yanıt eylemi ekleyerek](#add-response)isteğe yanıt verebilirsiniz.

   Mantık uygulamanız gelen isteği yalnızca bir dakika boyunca açık tutar. Mantık uygulaması iş akışınızın bir Yanıt eylemi içerdiğini varsayarsak, mantık uygulaması bu süre geçtikten `504 GATEWAY TIMEOUT` sonra yanıt vermiyorsa, mantık uygulamanız arayana bir yanıt verir. Aksi takdirde, mantık uygulamanız bir Yanıt eylemi içermiyorsa, mantık uygulamanız arayana anında bir `202 ACCEPTED` yanıt verir.

1. Işiniz bittiğinde, mantık uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin. 

   Bu adım, mantık uygulamasını tetikleyen isteği göndermek için kullanılacak URL'yi oluşturur. Bu URL'yi kopyalamak için URL'nin yanındaki kopya simgesini seçin.

   ![Mantık uygulamanızı tetikleyen url](./media/connectors-native-reqres/generated-url.png)

1. Mantık uygulamanızı tetiklemek için oluşturulan URL'ye bir HTTP POST gönderin. Örneğin, [Postacı](https://www.getpostman.com/)gibi bir araç kullanabilirsiniz.

### <a name="trigger-outputs"></a>Çıkışları tetikleme

İstek tetikleyicisinden çıkan çıktılar hakkında daha fazla bilgi aşağıda vereb

| JSON özellik adı | Veri türü | Açıklama |
|--------------------|-----------|-------------|
| `headers` | Nesne | İstekteki üstbilgi açıklayan bir JSON nesnesi |
| `body` | Nesne | İstekten gövde içeriğini açıklayan bir JSON nesnesi |
||||

<a name="add-response"></a>

## <a name="add-a-response-action"></a>Yanıt eylemi ekleme

Yanıt eylemini, gelen bir HTTPS isteğine yük (veri) ile yanıt vermek için, ancak yalnızca bir HTTPS isteği tarafından tetiklenen bir mantık uygulamasında kullanabilirsiniz. İş akışınızdaki herhangi bir noktada Yanıt eylemini ekleyebilirsiniz. Bu tetikleyici için temel JSON tanımı hakkında daha fazla bilgi için [Yanıt eylem türüne](../logic-apps/logic-apps-workflow-actions-triggers.md#response-action)bakın.

Mantık uygulamanız gelen isteği yalnızca bir dakika boyunca açık tutar. Mantık uygulaması iş akışınızın bir Yanıt eylemi içerdiğini varsayarsak, mantık uygulaması bu süre geçtikten `504 GATEWAY TIMEOUT` sonra yanıt vermiyorsa, mantık uygulamanız arayana bir yanıt verir. Aksi takdirde, mantık uygulamanız bir Yanıt eylemi içermiyorsa, mantık uygulamanız arayana anında bir `202 ACCEPTED` yanıt verir.

> [!IMPORTANT]
> Yanıt eylemi bu üstbilgi içeriyorsa, Logic Apps bu üstbilgilerini oluşturulan yanıt iletisinden herhangi bir uyarı veya hata göstermeden kaldırır:
>
> * `Allow`
> * `Content-*`bu istisnalar `Content-Disposition`dışında: , `Content-Encoding`, ve`Content-Type`
> * `Cookie`
> * `Expires`
> * `Last-Modified`
> * `Set-Cookie`
> * `Transfer-Encoding`
>
> Logic Apps, bu üstbilgilerle yanıt eylemi olan mantık uygulamalarını kaydetmenizi engellemese de, Logic Apps bu üstbilgi leri yok sayar.

1. Mantık Uygulama Tasarımcısı'nda, Yanıt eylemi eklemek istediğiniz adımın altında **Yeni adımı**seçin.

   Örneğin, daha önceki İstek tetikleyicisini kullanarak:

   ![Yeni adım ekleme](./media/connectors-native-reqres/add-response.png)

   Adımlar arasında bir eylem eklemek için, işaretçinizin bu adımlar arasında okun üzerine taşıyın. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Arama kutusunda **bir eylem seçin,** filtreniz olarak "yanıt" girin ve **Yanıt** eylemini seçin.

   ![Yanıt eylemini seçin](./media/connectors-native-reqres/select-response-action.png)

   İstek tetikleyicisi basitlik için bu örnekte daraltılır.

1. Yanıt iletisi için gereken değerleri ekleyin. 

   Bazı alanlarda, kutularının içine tıkladığınızda dinamik içerik listesi açılır. Daha sonra, iş akışındaki önceki adımlardaki kullanılabilir çıktıları temsil eden belirteçleri seçebilirsiniz. Önceki örnekte belirtilen şema özellikleri artık dinamik içerik listesinde görünür.

   Örneğin, **Üstbilgi** kutusu için, `Content-Type` anahtar adı olarak ekleyin ve `application/json` bu konuda daha önce belirtildiği gibi anahtar değerini ayarlayın. **Gövde** kutusu için, dinamik içerik listesinden tetik gövde çıktısını seçebilirsiniz.

   ![Yanıt eylem ayrıntıları](./media/connectors-native-reqres/response-details.png)

   Üstbilgilerini JSON biçiminde görüntülemek **için metin görünümüne geç'i**seçin.

   ![Üstbilgi - Metin görünümüne geç](./media/connectors-native-reqres/switch-to-text-view.png)

   Yanıt eyleminde ayarlayabildiğiniz özellikler hakkında daha fazla bilgi aşağıda verilmiştir. 

   | Özellik adı | JSON özellik adı | Gerekli | Açıklama |
   |---------------|--------------------|----------|-------------|
   | **Durum Kodu** | `statusCode` | Evet | Yanıtta döndürülecek durum kodu |
   | **Üst Bilgiler** | `headers` | Hayır | Yanıta dahil etmek üzere bir veya daha fazla üstbilgi açıklayan bir JSON nesnesi |
   | **Gövde** | `body` | Hayır | Yanıt gövdesi |
   |||||

1. Yanıt gövdesi için JSON şeması gibi ek özellikler belirtmek için **yeni parametre** listesi ekleyin ve eklemek istediğiniz parametreleri seçin.

1. Işiniz bittiğinde, mantık uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)
