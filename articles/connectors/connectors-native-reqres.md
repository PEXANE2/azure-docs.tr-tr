---
title: HTTPS kullanarak çağrıları alma ve yanıtlama
description: Azure Logic Apps kullanarak dış hizmetlerden gelen HTTPS isteklerini işleyin
services: logic-apps
ms.suite: integration
ms.reviewers: jonfan, logicappspm
ms.topic: conceptual
ms.date: 05/06/2020
tags: connectors
ms.openlocfilehash: c6d8dc087e6306173fc4d55368cd3c4c624d5302
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82978578"
---
# <a name="receive-and-respond-to-inbound-https-requests-in-azure-logic-apps"></a>Azure Logic Apps gelen HTTPS isteklerini alın ve bunlara yanıt verin

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik istek tetikleyicisi ve yanıtı eylemiyle, gelen https isteklerini alıp yanıtlayan otomatik görevler ve iş akışları oluşturabilirsiniz. Örneğin, mantıksal uygulamanızı kullanabilirsiniz:

* Şirket içi bir veritabanındaki veriler için bir HTTPS isteği alın ve yanıt verin.
* Dış Web kancası olayı gerçekleştiğinde iş akışı tetikleyin.
* Başka bir mantıksal uygulamadan bir HTTPS çağrısını alın ve yanıtlayın.

Istek tetikleyicisi, mantıksal uygulamanıza gelen çağrıları yetkilendirmek için [Azure Active Directory açma kimlik doğrulamasını](../active-directory/develop/about-microsoft-identity-platform.md) (Azure AD OAuth) destekler. Bu kimlik doğrulamasını etkinleştirme hakkında daha fazla bilgi için [Azure Logic Apps Azure AD OAuth kimlik doğrulamasını etkinleştirme ' deki güvenli erişim ve verilere](../logic-apps/logic-apps-securing-a-logic-app.md#enable-oauth)bakın.

> [!NOTE]
> Istek tetikleyicisi, gelen çağrılar için *yalnızca* aktarım katmanı GÜVENLIĞI (TLS) 1,2 ' i destekler. Giden çağrılar TLS 1,0, 1,1 ve 1,2 destekler. Daha fazla bilgi için bkz. [TLS 1,0 sorununu çözme](https://docs.microsoft.com/security/solving-tls1-problem).
>
> TLS el sıkışma hataları alırsanız TLS 1,2 kullandığınızdan emin olun. 
> Gelen çağrılar için desteklenen şifre paketleri şunlardır:
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

* Azure aboneliği. Aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolabilirsiniz](https://azure.microsoft.com/free/).

* [Logic Apps](../logic-apps/logic-apps-overview.md)hakkında temel bilgi. Logic Apps 'e yeni başladıysanız, [ilk mantıksal uygulamanızı oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

<a name="add-request"></a>

## <a name="add-request-trigger"></a>Istek tetikleyicisi Ekle

Bu yerleşik tetikleyici, *yalnızca* gelen https isteklerini alabilen el ile ÇAĞRıLABILIR bir HTTPS uç noktası oluşturur. Bu olay gerçekleştiğinde tetikleyici ateşlenir ve mantıksal uygulamayı çalıştırır. Tetikleyicinin temel alınan JSON tanımı ve bu tetikleyiciyi çağırma hakkında daha fazla bilgi için, bkz. [istek tetikleme türü](../logic-apps/logic-apps-workflow-actions-triggers.md#request-trigger) ve Azure Logic Apps ' [de HTTPS uç noktaları ile çağrı, tetikleyici veya iç içe geçme iş akışları](../logic-apps/logic-apps-http-endpoint.md).

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş bir mantıksal uygulama oluşturma.

1. Mantıksal uygulama Tasarımcısı açıldıktan sonra arama kutusuna filtreniz olarak girin `http request` . Tetikleyiciler listesinden, mantıksal uygulama iş akışınızın ilk adımı olan **BIR http isteği alındığında** tetiklenir ' ı seçin.

   ![Istek tetikleyicisi seçin](./media/connectors-native-reqres/select-request-trigger.png)

   Istek tetikleyicisi şu özellikleri gösterir:

   ![İstek tetikleyicisi](./media/connectors-native-reqres/request-trigger.png)

   | Özellik adı | JSON Özellik adı | Gerekli | Açıklama |
   |---------------|--------------------|----------|-------------|
   | **HTTP POST URL 'SI** | seçim | Yes | Mantıksal uygulamayı kaydettikten sonra oluşturulan ve mantıksal uygulamanızı çağırmak için kullanılan uç nokta URL 'SI |
   | **İstek gövdesi JSON şeması** | `schema` | No | Gelen istek gövdesindeki özellikleri ve değerleri açıklayan JSON şeması |
   |||||

1. **Istek GÖVDESI JSON şeması** kutusunda, isteğe bağlı olarak, gelen istekteki gövdeyi açıklayan bir JSON şeması girin, örneğin:

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
   | **Yöntem** | `method` | No | Gelen isteğin mantıksal uygulamayı çağırmak için kullanması gereken Yöntem |
   | **Göreli yol** | `relativePath` | No | Mantıksal uygulamanın uç nokta URL 'sinin kabul edebileceği parametrenin göreli yolu |
   |||||

   Bu örnek, **Yöntem** özelliğini ekler:

   ![Yöntem parametresi Ekle](./media/connectors-native-reqres/add-parameters.png)

   **Yöntem** özelliği tetikleyicide görünür, böylece listeden bir yöntem seçebilirsiniz.

   ![Select yöntemi](./media/connectors-native-reqres/select-method.png)

1. Şimdi, iş akışınızda sonraki adım olarak başka bir eylem ekleyin. Tetikleyici altında, eklemek istediğiniz eylemi bulabilmeniz için bir **sonraki adım** ' ı seçin.

   Örneğin, özel bir yanıt döndürmek için kullanabileceğiniz ve bu konunun ilerleyen kısımlarında açıklanan [bir yanıt eylemi ekleyerek](#add-response)isteğe yanıt verebilirsiniz.

   Mantıksal uygulamanız gelen isteği yalnızca bir dakika boyunca açık tutar. Mantıksal uygulama iş akışınızın bir yanıt eylemi içerdiğini varsayarsak, mantıksal uygulama bu süre geçtikten sonra bir yanıt döndürmezse, mantıksal uygulamanız bir `504 GATEWAY TIMEOUT` arayana döndürür. Aksi takdirde, mantıksal uygulamanız bir yanıt eylemi içermiyorsa, mantıksal uygulamanız hemen çağırana bir `202 ACCEPTED` yanıt döndürür.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin.

   Bu adım, mantıksal uygulamayı tetikleyen isteği göndermek için kullanılacak URL 'YI oluşturur. Bu URL 'yi kopyalamak için URL 'nin yanındaki Kopyala simgesini seçin.

   ![Mantıksal uygulamanızı tetiklemenin kullanılacağı URL](./media/connectors-native-reqres/generated-url.png)

1. Mantıksal uygulamanızı tetiklemek için, oluşturulan URL 'ye bir HTTP GÖNDERISI gönderin.

   Örneğin, HTTP GÖNDERISINI göndermek için [Postman](https://www.getpostman.com/) gibi bir araç kullanabilirsiniz. Istek tetikleyicisine gelen çağrıları yetkilendirmek için [Azure Active Directory açma kimlik doğrulamasını](../logic-apps/logic-apps-securing-a-logic-app.md#enable-oauth) (Azure AD OAuth) etkinleştirdiyseniz, bir [paylaşılan ERIŞIM imzası (SAS) URL 'si](../logic-apps/logic-apps-securing-a-logic-app.md#sas) kullanarak veya bir kimlik doğrulama belirteci kullanarak tetikleyiciyi çağırın, ancak ikisini birden kullanamazsınız. Kimlik doğrulama belirtecinin yetkilendirme üstbilgisindeki `Bearer` türü belirtmesi gerekir. Daha fazla bilgi için bkz. [İstek tabanlı tetikleyicilere Azure Logic Apps erişimi ve verileri güvenli hale getirme](../logic-apps/logic-apps-securing-a-logic-app.md#secure-triggers).

Tetikleyicinin temel alınan JSON tanımı ve bu tetikleyiciyi çağırma hakkında daha fazla bilgi için, bu konulara bakın, [tetikleyici türü isteyin](../logic-apps/logic-apps-workflow-actions-triggers.md#request-trigger) ve [Azure Logic Apps içindeki HTTP uç noktalarına çağrı, tetikleyici veya iç Içe iş akışları çağırın](../logic-apps/logic-apps-http-endpoint.md).

### <a name="trigger-outputs"></a>Tetikleme çıkışları

Istek tetikleyicisinden alınan çıktılar hakkında daha fazla bilgi aşağıda verilmiştir:

| JSON Özellik adı | Veri türü | Açıklama |
|--------------------|-----------|-------------|
| `headers` | Nesne | İstekten üstbilgileri açıklayan bir JSON nesnesi |
| `body` | Nesne | İstekten gelen gövde içeriğini açıklayan bir JSON nesnesi |
||||

<a name="add-response"></a>

## <a name="add-a-response-action"></a>Yanıt eylemi ekleme

Yanıt eylemini, gelen HTTPS isteğine bir yük (veri) ile yanıt vermek için, ancak yalnızca HTTPS isteği tarafından tetiklenen bir mantıksal uygulamada kullanabilirsiniz. Yanıt eylemini iş akışınızın herhangi bir noktasına ekleyebilirsiniz. Bu tetikleyicinin temel alınan JSON tanımı hakkında daha fazla bilgi için bkz. [yanıt eylem türü](../logic-apps/logic-apps-workflow-actions-triggers.md#response-action).

Mantıksal uygulamanız gelen isteği yalnızca bir dakika boyunca açık tutar. Mantıksal uygulama iş akışınızın bir yanıt eylemi içerdiğini varsayarsak, mantıksal uygulama bu süre geçtikten sonra bir yanıt döndürmezse, mantıksal uygulamanız bir `504 GATEWAY TIMEOUT` arayana döndürür. Aksi takdirde, mantıksal uygulamanız bir yanıt eylemi içermiyorsa, mantıksal uygulamanız hemen çağırana bir `202 ACCEPTED` yanıt döndürür.

> [!IMPORTANT]
> Bir yanıt eylemi bu üstbilgileri içeriyorsa Logic Apps, bu üst bilgileri herhangi bir uyarı veya hata göstermeden oluşturulan yanıt iletisinden kaldırır:
>
> * `Allow`
> * `Content-*`Bu özel durumlarla birlikte `Content-Disposition`: `Content-Encoding`, ve`Content-Type`
> * `Cookie`
> * `Expires`
> * `Last-Modified`
> * `Set-Cookie`
> * `Transfer-Encoding`
>
> Logic Apps, bu üst bilgilerle bir yanıt eylemi olan mantıksal uygulamaları kaydetmenizi durdurmasa da, Logic Apps bu üst bilgileri yoksayar.

1. Mantıksal uygulama Tasarımcısı ' nda, yanıt eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Örneğin, daha önce Istek tetikleyicisi kullanımı:

   ![Yeni adım Ekle](./media/connectors-native-reqres/add-response.png)

   Adımlar arasında bir eylem eklemek için, işaretçinizi Bu adımlar arasındaki oka taşıyın. Görüntülenen artı işaretini (**+**) seçin ve ardından **Eylem Ekle**' yi seçin.

1. **Eylem seçin**altında, arama kutusuna filtreniz olarak "yanıt" yazın ve **Yanıt** eylemini seçin.

   ![Yanıt eylemini seçin](./media/connectors-native-reqres/select-response-action.png)

   Istek tetikleyicisi kolaylık sağlaması için bu örnekte daraltılır.

1. Yanıt iletisi için gereken tüm değerleri ekleyin. 

   Bazı alanlarda, kutularının içine tıklanması dinamik içerik listesini açar. Daha sonra iş akışındaki önceki adımlardan bulunan mevcut çıkışları temsil eden belirteçleri seçebilirsiniz. Önceki örnekte belirtilen şemadan özellikler artık dinamik içerik listesinde görünür.

   Örneğin, **üst bilgiler** kutusu için, anahtar adı `Content-Type` olarak ekleyin ve anahtar değerini bu konunun önceki kısımlarında belirtildiği `application/json` gibi olarak ayarlayın. **Gövde** kutusu için dinamik içerik listesinden tetikleyiciyi ilet çıktısını seçebilirsiniz.

   ![Yanıt eylemi ayrıntıları](./media/connectors-native-reqres/response-details.png)

   Üst bilgileri JSON biçiminde görüntülemek için **metin görünümüne geç**' i seçin.

   ![Üstbilgiler-metin görünümüne geç](./media/connectors-native-reqres/switch-to-text-view.png)

   Yanıt eyleminde ayarlayabileceğiniz özellikler hakkında daha fazla bilgi bulabilirsiniz. 

   | Özellik adı | JSON Özellik adı | Gerekli | Açıklama |
   |---------------|--------------------|----------|-------------|
   | **Durum kodu** | `statusCode` | Yes | Yanıtta döndürülecek durum kodu |
   | **Bilgisinde** | `headers` | No | Yanıta eklenecek bir veya daha fazla üstbilgiyi açıklayan bir JSON nesnesi |
   | **Gövde** | `body` | No | Yanıt gövdesi |
   |||||

1. Yanıt gövdesi için JSON şeması gibi ek özellikler belirtmek için **yeni parametre Ekle** listesini açın ve eklemek istediğiniz parametreleri seçin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)