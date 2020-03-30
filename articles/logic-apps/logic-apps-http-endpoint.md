---
title: Çağrı, tetikleyici veya iç içe mantıksal uygulamalar
description: Azure Mantık Uygulamaları'nda arama, tetikleme veya yuvalama mantığı uygulaması iş akışları için HTTP uç noktalarını ayarlama
services: logic-apps
ms.workload: integration
ms.reviewer: klam, jehollan, logicappspm
ms.topic: article
ms.date: 11/04/2019
ms.openlocfilehash: d5b5a69c7927d07c0ae6b3b56ec97b6551e5d46b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77191344"
---
# <a name="call-trigger-or-nest-logic-apps-by-using-http-endpoints-in-azure-logic-apps"></a>Azure Mantık Uygulamaları'nda HTTP uç noktalarını kullanarak mantık uygulamalarını arama, tetikleme veya yerleştirme

Mantık uygulamanızın diğer hizmetlerden gelen istekleri alabilmeleri için mantık uygulamanızı bir URL üzerinden çağrılabilir hale getirmek için, bu mantık uygulamasında tetikleyici olarak eşzamanlı bir HTTP bitiş noktasını yerel olarak ortaya çıkarabilirsiniz. Bu özelliği ayarladığınızda, mantık uygulamanızı diğer mantık uygulamalarının içine de ekleyebilir ve bu da çağrılabilir uç noktalar deseni oluşturmanıza olanak tanır.

Bir HTTP bitiş noktası ayarlamak için, mantık uygulamalarının gelen istekleri almasını sağlayan bu tetikleyici türlerinden herhangi birini kullanabilirsiniz:

* [İstek](../connectors/connectors-native-reqres.md)
* [HTTP Web kancası](../connectors/connectors-native-webhook.md)
* [ApiConnectionWebhook türüne](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-trigger) sahip ve gelen HTTP isteklerini alabilen yönetilen bağlayıcı tetikleyiciler

> [!NOTE]
> Bu örnekler İstek tetikleyicisini kullanır, ancak önceki listede bulunan herhangi bir HTTP istek tabanlı tetikleyiciyi kullanabilirsiniz. Tüm ilkeler aynı şekilde bu diğer tetikleyici türleri için geçerlidir.

Mantık uygulamalarında yeniyseniz, Azure Mantık Uygulamaları ve Quickstart [nedir:](../logic-apps/logic-apps-overview.md) [İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Tetikleyici olarak HTTP bitiş noktasını ayarlamak istediğiniz mantık uygulaması. Boş bir mantık uygulaması yla veya geçerli tetikleyiciyi değiştirmek istediğiniz varolan bir mantık uygulamasıyla başlayabilirsiniz. Bu örnek boş bir mantık uygulamasıyla başlar.

## <a name="create-a-callable-endpoint"></a>Ayarlanabilir bir bitiş noktası oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın. Logic App Designer'da boş bir mantık uygulaması oluşturun ve açın.

   Bu örnek, İstek tetikleyicisini kullanır, ancak gelen HTTP isteklerini alabilecek herhangi bir tetikleyiciyi kullanabilirsiniz. Tüm ilkeler aynı şekilde bu tetikleyiciler için geçerlidir. İstek tetikleyicisi hakkında daha fazla bilgi için Azure [Mantık Uygulamaları'nı kullanarak gelen HTTPS aramalarını al ve yanıtla.](../connectors/connectors-native-reqres.md)

1. Arama kutusunun altında **Yerleşik'i**seçin. Arama kutusuna filtreniz olarak girin. `request` Tetikleyiciler listesinden, **bir HTTP isteği ne zaman alındığını**seçin.

   ![İstek tetikleyicisini bulma ve seçme](./media/logic-apps-http-endpoint/find-and-select-request-trigger.png)

1. İsteğe bağlı olarak, **İstek Gövdesi JSON Schema** kutusuna, tetikleyicinin almasını beklediğiniz yükü veya verileri açıklayan bir JSON şeması girebilirsiniz.

   Tasarımcı, tetikleyici çıktıları temsil eden belirteçler oluşturmak için bu şemayı kullanır. Daha sonra, mantık uygulamanızın iş akışı boyunca bu çıktılara kolayca başvuruyapabilirsiniz. [JSON şemalarından oluşturulan jetonlar](#generated-tokens)hakkında daha fazla bilgi edinin.

   Bu örnek için, bu şea girin:

   ```json
      {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "streetNumber": {
                  "type": "string"
               },
               "streetName": {
                  "type": "string"
               },
               "town": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "string"
               }
            }
         }
      }
   }
    ```

   ![İstek eylemi için JSON şeması sağlayın](./media/logic-apps-http-endpoint/manual-request-trigger-schema.png)

   Veya, örnek bir yük sağlayarak bir JSON şeması oluşturabilirsiniz:

   1. **İstek** tetikleyicisinde **şema oluşturmak için örnek yükü kullan'ı**seçin.

   1. Örnek **bir JSON yük kutusunu girin veya yapıştırın,** örneğin örnek yükünüzü girin:

      ```json
      {
         "address": {
            "streetNumber": "00000",
            "streetName": "AnyStreet",
            "town": "AnyTown",
            "postalCode": "11111-1111"
        }
      }
      ```

   1. Hazır **olduğunuzda, Bitti'yi**seçin.

      **İstek Vücut JSON Schema** kutusu şimdi oluşturulan şema gösterir.

1. Mantıksal uygulamanızı kaydedin.

   **Bu URL kutusuna** http POST artık diğer hizmetlerin mantık uygulamanızı çağırmak ve tetiklemek için kullanabileceği oluşturulan geri arama URL'sini gösterir. Bu URL, örneğin sorgu parametrelerinde kimlik doğrulama için kullanılan paylaşılan erişim imzası (SAS) anahtarını içerir:

   ![Uç nokta için oluşturulan geri arama URL'si](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

   Ayrıca, mantık uygulamanızın **Genel Bakış** bölmesinden HTTP uç nokta URL'sini de alabilirsiniz.

   1. Mantık uygulamanızın menüsünde **Genel Bakış'ı**seçin.

   1. **Özet** bölümünde, **tetikleyici geçmişi gör'üni**seçin.

      ![Azure portalından HTTP uç noktası URL'si alın](./media/logic-apps-http-endpoint/find-manual-trigger-url.png)

   1. **Callback url [POST]** altında, URL'yi kopyalayın:

      ![Azure portalından HTTP uç noktası URL'si kopyalama](./media/logic-apps-http-endpoint/copy-manual-trigger-callback-url.png)

      Veya bu aramayı yaparak URL'yi alabilirsiniz:

      ```http
      POST https://management.azure.com/{logic-app-resource-ID}/triggers/{endpoint-trigger-name}/listCallbackURL?api-version=2016-06-01
      ```

<a name="set-method"></a>

## <a name="set-expected-http-method"></a>Beklenen HTTP yöntemini ayarlama

Varsayılan olarak, İstek tetikleyicisi bir HTTP POST isteği bekler. Ancak, beklemek için farklı bir yöntem belirtebilirsiniz, ancak yalnızca bir yöntem.

1. İstek tetikleyicisinde, **yeni parametre** ekle listesini açın ve bu özelliği tetikleyiciye ekleyen **Yöntem'i**seçin.

   ![Tetiklemek için "Yöntem" özelliği ekleme](./media/logic-apps-http-endpoint/select-add-new-parameter-for-method.png)

1. **Yöntem** listesinden, tetikleyicinin beklediği başka bir yöntem seçin. Veya özel bir yöntem belirtebilirsiniz.

   Örneğin, HTTP uç noktanızın URL'sini daha sonra test etmek için **GET** yöntemini seçin.

   ![Tetikleyici için kullanmak üzere HTTP yöntemini seçin](./media/logic-apps-http-endpoint/select-method-request-trigger.png)

## <a name="accept-parameters-in-endpoint-url"></a>Parametreleri uç nokta URL'sinde kabul et

Uç nokta URL'nizin parametreleri kabul etmesini istediğinizde, tetikleyicinizdeki göreli yolu belirtin. Ayrıca, HTTP [isteğinizin](#set-method) beklediği yöntemi açıkça ayarlamanız gerekir.

1. İstek tetikleyicisinde, **yeni parametre** ekle listesini açın ve bu özelliği tetikleyiciye ekleyen **Göreli yolu**seçin.

   ![Tetiklemek için "Göreli yol" özelliği ekleme](./media/logic-apps-http-endpoint/select-add-new-parameter-for-relative-path.png)

1. **Göreli yol** özelliğinde, JSON şemanızda URL'nizin kabul etmesini istediğiniz parametrenin göreli yolunu belirtin, örneğin. `address/{postalCode}`

   ![Parametrenin göreli yolunu belirtin](./media/logic-apps-http-endpoint/relative-path-url-value.png)

1. Parametreyi kullanmak için mantık uygulamanıza bir **Yanıt** eylemi bulun ve ekleyin.

   1. İstek tetikleyicisinin altında **Yeni adım'ı** > seçin**eylem ekle'** yi seçin.

   1. Bir **eylem seçin**altında, arama `response` kutusuna filtreniz olarak girin.

   1. Eylemler listesinden **Yanıt** eylemini seçin.

1. Yanıt eyleminin **Gövdesi** özelliğinde, tetikleyicinizin göreli yolunda belirttiğiniz parametreyi temsil eden belirteci ekleyin.

   Örneğin, Yanıt eyleminin dönmesini `Postal Code: {postalCode}`istediğinizi varsayalım.

   **Vücut** özelliği, bir `Postal Code: ` iz alanı ile girin. Görünen dinamik içerik listesinden Posta **Kodu** belirteci'ni seçin.

   ![Yanıt gövdesine belirtilen parametreyi ekleme](./media/logic-apps-http-endpoint/relative-url-with-parameter-token.png)

   **Gövde** özelliği artık seçili parametreyi içerir:

   ![Parametreli örnek yanıt gövdesi](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

1. Mantıksal uygulamanızı kaydedin.

    HTTP uç nokta URL'niz artık göreceli yolu içerir, örneğin:

    ```http
    https://prod-25.westus.logic.azure.com/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke/address/{postalCode}?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}
    ```

1. HTTP bitiş noktanızı test etmek için güncellenen URL'yi başka `{postalCode}` `123456`bir tarayıcı penceresine kopyalayıp yapıştırın, ancak değiştirin ve Enter tuşuna basın.

   Tarayıcınız bu metni gösterir:`Postal Code: 123456`

## <a name="call-logic-app-through-http-endpoint"></a>HTTP bitiş noktası üzerinden arama mantığı uygulaması

HTTP bitiş noktasını oluşturduktan sonra, bitiş noktasının tam `POST` URL'sine bir HTTP isteği göndererek mantık uygulamasını tetikleyebilirsiniz. Mantık uygulamaları, doğrudan erişim uç noktaları için yerleşik destek içerir.

<a name="generated-tokens"></a>

## <a name="tokens-generated-from-schema"></a>Şema'dan oluşturulan belirteçler

İstek tetikleyicisinde bir JSON şeması sağladığınızda, Logic App Designer bu şemadaki özellikler için belirteçler oluşturur. Daha sonra bu belirteçleri mantık uygulaması iş akışı nızdan veri aktarmak için kullanabilirsiniz.

Örneğin, JSON şemanıza , `"suite"`örneğin daha fazla özellik eklerseniz, bu özellikler için belirteçler, mantık uygulamanızın sonraki adımlarında kullanmanız için kullanılabilir. Burada tam JSON şema:

```json
   {
   "type": "object",
   "properties": {
      "address": {
         "type": "object",
         "properties": {
            "streetNumber": {
               "type": "string"
            },
            "streetName": {
               "type": "string"
            },
            "suite": {
               "type": "string"
            },
            "town": {
               "type": "string"
            },
            "postalCode": {
               "type": "string"
            }
         }
      }
   }
}
```

## <a name="create-nested-logic-apps"></a>İç içe mantık uygulamaları oluşturma

İstek leri alabilecek diğer mantık uygulamaları ekleyerek iş akışlarını mantık uygulamanızda yuvalayabilirsiniz. Bu mantık uygulamalarını eklemek için aşağıdaki adımları izleyin:

1. Başka bir mantık uygulamasını çağırmak istediğiniz adımın altında, **Yeni adım** > **bir eylem ekle'yi**seçin.

1. Bir **eylem seçin,** **Dahili'yi**seçin. Arama kutusuna filtreniz olarak girin. `logic apps` Eylemler listesinden, **Mantık Uygulamaları iş akışını seçin'i**seçin.

   ![Geçerli mantık uygulaması nın içindeki mantık uygulaması yuvalama](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

   Tasarımcı, seçmeniz için uygun mantık uygulamalarını gösterir.

1. Geçerli mantık uygulamanızdan aramak için mantık uygulamasını seçin.

   ![Geçerli mantık uygulamasından aramak için mantık uygulamasını seçin](./media/logic-apps-http-endpoint/select-logic-app-to-nest.png)

## <a name="reference-content-from-an-incoming-request"></a>Gelen bir istekten referans içeriği

Gelen isteğin içerik türü ise, `application/json`gelen istekteki özelliklere başvuruda bulunabilirsiniz. Aksi takdirde, bu içerik diğer API'lere geçirebileceğiniz tek bir ikili birim olarak kabul edilir. Mantık uygulamanızın iş akışı içinde bu içeriğe başvurmak için önce bu içeriği dönüştürmeniz gerekir.

Örneğin, türü olan `application/xml` içeriği geçiyorsanız, [ `@xpath()` bir](../logic-apps/workflow-definition-language-functions-reference.md#xpath) XPath çıkarma gerçekleştirmek için [ `@json()` ifadeyi](../logic-apps/workflow-definition-language-functions-reference.md#json) kullanabilir veya XML'yi JSON'a dönüştürmek için ifadeyi kullanabilirsiniz. Desteklenen [içerik türleri](../logic-apps/logic-apps-content-type.md)ile çalışma hakkında daha fazla bilgi edinin.

Çıktıyı gelen bir istekten almak için [ `@triggerOutputs` ifadeyi](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs)kullanabilirsiniz. Örneğin, bu örneğe benzeyen çıktınız olduğunu varsayalım:

```json
{
   "headers": {
      "content-type" : "application/json"
   },
   "body": {
      "myProperty" : "property value"
   }
}
```

Özellikle özel `body` tesise erişmek için [ `@triggerBody()` ifadeyi](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) kısayol olarak kullanabilirsiniz.

## <a name="respond-to-requests"></a>İstekleri yanıtlama

Bazen içeriği arayana geri vererek mantık uygulamanızı tetikleyen belirli isteklere yanıt vermek istersiniz. Yanıtınız için durum kodunu, üstbilgisini ve gövdesini oluşturmak için Yanıt eylemini kullanın. Bu eylem, sadece iş akışınızın sonunda değil, mantık uygulamanızın herhangi bir yerinde görünebilir. Mantık uygulamanız bir Yanıt eylemi içermiyorsa, HTTP bitiş noktası **202 Kabul edilen** durumla *anında* yanıt verir.

Özgün arayanın yanıtı başarıyla alabilmesi için, tetiklenen mantık uygulaması iç içe mantık uygulaması olarak adlandırılmadığı sürece yanıt için gereken tüm adımların [istek zaman dışarı sınırı](./logic-apps-limits-and-config.md) içinde tamamlanması gerekir. Bu sınır içinde yanıt döndürülmezse, gelen istek zamanlanır ve **408 İstemci zaman ödeme** yanıtını alır.

İç içe mantık uygulamaları için, ana mantık uygulaması ne kadar zaman gerektirdiğine bakılmaksızın tüm adımlar tamamlanana kadar yanıt beklemeye devam eder.

### <a name="construct-the-response"></a>Yanıtı oluşturma

Yanıt gövdesine, birden çok üstbilgi ve her tür içerik ekleyebilirsiniz. Örneğin, bu yanıtın üstbilgisinde yanıtın içerik türü ve `application/json` gövdenin İstek tetikleyicisi için bu konuda daha önce açıklanan JSON şemasına göre değerler `town` ve `postalCode` özellikler içerdiği belirtilir.

![HTTP Yanıt eylemi için yanıt içeriği sağlama](./media/logic-apps-http-endpoint/content-for-response-action.png)

Yanıtlar şu özelliklere sahiptir:

| Özellik (Ekran) | Emlak (JSON) | Açıklama |
|--------------------|-----------------|-------------|
| **Durum Kodu** | `statusCode` | Gelen istek yanıtında kullanılacak HTTP durum kodu. Bu kod 2xx, 4xx veya 5xx ile başlayan geçerli bir durum kodu olabilir. Ancak, 3xx durum kodlarına izin verilmez. |
| **Üst Bilgiler** | `headers` | Yanıta dahil edilen bir veya daha fazla üstbilgi |
| **Gövde** | `body` | Bir dize, JSON nesnesi veya hatta önceki adımdan başvurulan ikili içerik olabilecek bir gövde nesnesi |
||||

Yanıt eylemi için JSON tanımını ve mantık uygulamanızın tam JSON tanımını Logic App Designer araç çubuğunda görüntülemek için **Kod görünümünü**seçin.

``` json
"Response": {
   "type": "Response",
   "kind": "http",
   "inputs": {
      "body": {
         "postalCode": "@triggerBody()?['address']?['postalCode']",
         "town": "@triggerBody()?['address']?['town']"
      },
      "headers": {
         "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {}
}
```

## <a name="q--a"></a>Soru-Cevap

#### <a name="q-what-about-url-security"></a>S: URL güvenliği ne olacak?

**C**: [Azure, Paylaşılan Erişim İmzası (SAS)](https://docs.microsoft.com/rest/api/storageservices/delegate-access-with-shared-access-signature)kullanarak mantık uygulaması geri arama URL'lerini güvenli bir şekilde oluşturur. Bu imza bir sorgu parametresi olarak geçer ve mantık uygulamanızın çalıştırılaması için önce doğrulanması gerekir. Azure, mantık uygulaması başına gizli bir anahtar, tetikleyici ad ve gerçekleştirilen işlemin benzersiz bir birleşimini kullanarak imzayı oluşturur. Bu nedenle, birisi gizli mantık uygulama anahtarına erişemiyorsa, geçerli bir imza oluşturamaz.

> [!IMPORTANT]
> Üretim ve yüksek güvenlik sistemleri için, mantık uygulamanızı aşağıdaki nedenlerden dolayı doğrudan tarayıcıdan aramanızı şiddetle tavsiye ederiz:
>
> * Paylaşılan erişim anahtarı URL'de görünür.
> * Azure Logic Apps müşterilerinin paylaştığı etki alanları nedeniyle güvenlik içerik ilkelerini yönetemezsiniz.

#### <a name="q-can-i-configure-http-endpoints-further"></a>S: HTTP uç noktalarını daha fazla yapılandırabilir miyim?

**C**: Evet, HTTP uç noktaları [Azure API Yönetimi](../api-management/api-management-key-concepts.md)aracılığıyla daha gelişmiş yapılandırmayı destekler. Bu hizmet ayrıca, mantık uygulamaları da dahil olmak üzere tüm API'lerinizi tutarlı bir şekilde yönetebilme, özel alan adları oluşturma, daha fazla kimlik doğrulama yöntemi kullanma ve örneğin daha fazlasını yönetebilme olanağı da sunar:

* [İstek yöntemini değiştirme](../api-management/api-management-advanced-policies.md#SetRequestMethod)
* [İsteğin URL bölümlerini değiştirme](../api-management/api-management-transformation-policies.md#RewriteURL)
* API Yönetimi etki alanlarınızı [Azure portalında](https://portal.azure.com/) ayarlama
* Temel kimlik doğrulamasını denetlemek için ilke ayarlama

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps'ı kullanarak gelen HTTPS aramalarını alma ve yanıtla](../connectors/connectors-native-reqres.md)