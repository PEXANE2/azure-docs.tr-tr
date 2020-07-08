---
title: Çağrı, tetikleyici veya iç içe mantıksal uygulamalar
description: Azure Logic Apps içinde mantıksal uygulama iş akışlarını çağırmak, tetiklemek veya iç içe aktarmak için HTTPS uç noktalarını ayarlama
services: logic-apps
ms.workload: integration
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 05/28/2020
ms.openlocfilehash: b5c4005c95a88a40a836b9c0f6d1fd01e0417ed0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84170282"
---
# <a name="call-trigger-or-nest-logic-apps-by-using-https-endpoints-in-azure-logic-apps"></a>Azure Logic Apps ' de HTTPS uç noktalarını kullanarak Logic Apps 'i çağırma, tetikleme veya iç içe geçme

Mantıksal uygulamanızın diğer hizmetlerden gelen istekleri alabilmesi için mantıksal uygulamanızı bir URL aracılığıyla çağrılabilir hale getirmek için, bu mantıksal uygulamada bir tetikleyici olarak zaman uyumlu bir HTTPS uç noktası kullanıma sunabilirsiniz. Bu özelliği ayarlarken, mantıksal uygulamanızı diğer mantıksal uygulamalarda iç içe geçirebilirsiniz, bu da çağrılabilir bir uç nokta stili oluşturmanızı sağlar.

Çağrılabilir bir uç nokta ayarlamak için, mantıksal uygulamaların gelen istekleri almasını sağlayan bu tetikleyici türlerinden herhangi birini kullanabilirsiniz:

* [İstek](../connectors/connectors-native-reqres.md)
* [HTTP Web kancası](../connectors/connectors-native-webhook.md)
* [Apiconnectionweb kancası türüne](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnectionwebhook-trigger) sahip yönetilen bağlayıcı Tetikleyicileri ve gelen https isteklerini alabilir

> [!NOTE]
> Bu örnekler, Istek tetikleyicisini kullanır, ancak önceki listede bulunan HTTPS istek tabanlı bir tetikleyiciyi kullanabilirsiniz. Tüm ilkeler bu diğer tetikleyici türleri için de geçerlidir.

Logic Apps 'e yeni başladıysanız, bkz. [Azure Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: Ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Çağrılabilir uç noktayı oluşturmak için tetikleyiciyi kullanmak istediğiniz mantıksal uygulama. Geçerli tetikleyiciyi değiştirmek istediğiniz boş bir mantıksal uygulama ya da mevcut bir mantıksal uygulama ile başlayabilirsiniz. Bu örnek, boş bir mantıksal uygulamayla başlar.

## <a name="create-a-callable-endpoint"></a>Çağrılabilir bir uç nokta oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın. Mantıksal uygulama tasarımcısında boş bir mantıksal uygulama oluşturun ve açın.

   Bu örnek, Istek tetikleyicisini kullanır, ancak gelen HTTPS isteklerini alabilen herhangi bir tetikleyiciyi kullanabilirsiniz. Tüm ilkeler bu Tetikleyiciler için aynı şekilde geçerlidir. Istek tetikleyicisi hakkında daha fazla bilgi için, bkz. [Azure Logic Apps kullanarak gelen https çağrılarını alma ve yanıtlama](../connectors/connectors-native-reqres.md).

1. Arama kutusunda, **yerleşik**' i seçin. Arama kutusuna `request` filtreniz olarak yazın. Tetikleyiciler listesinden **BIR http isteği alındığında**öğesini seçin.

   ![Istek tetikleyicisini bul ve Seç](./media/logic-apps-http-endpoint/find-and-select-request-trigger.png)

1. İsteğe bağlı olarak, **Istek GÖVDESI JSON şeması** kutusunda, tetikleyicinin almasını istediğiniz yükü veya verileri açıklayan bir JSON şeması girebilirsiniz.

   Tasarımcı, tetikleyici çıkışlarını temsil eden belirteçler oluşturmak için bu şemayı kullanır. Daha sonra mantıksal uygulamanızın iş akışının tamamında bu çıkışlara kolayca başvurabilirsiniz. [JSON şemalardan oluşturulan belirteçler](#generated-tokens)hakkında daha fazla bilgi edinin.

   Bu örnek için şu şemayı girin:

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

   ![Istek eylemi için JSON şeması sağla](./media/logic-apps-http-endpoint/manual-request-trigger-schema.png)

   Veya örnek yük sağlayarak bir JSON şeması oluşturabilirsiniz:

   1. **İstek** tetikleyicisinde, **şema oluşturmak Için örnek yük kullan**' ı seçin.

   1. **Örnek BIR JSON yükü girin veya yapıştırın** kutusuna örnek yükünüzü girin, örneğin:

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

   1. Hazırsanız, **bitti**' yi seçin.

      **Istek GÖVDESI JSON şeması** kutusu artık oluşturulan şemayı gösterir.

1. Mantıksal uygulamanızı kaydedin.

   **Http post URL 'si** kutusu artık diğer hizmetlerin mantıksal uygulamanızı çağırmak ve tetiklemek için kullanabileceği, oluşturulan GERI çağırma URL 'sini gösterir. Bu URL, kimlik doğrulaması için kullanılan bir paylaşılan erişim Imzası (SAS) anahtarı belirten sorgu parametrelerini içerir.

   ![Uç nokta için geri çağırma URL 'SI oluşturuldu](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

1. Geri arama URL 'sini kopyalamak için şu seçeneklere sahipsiniz:

   * **Http post URL 'si** kutusunun sağında **URL 'yi Kopyala** (dosyaları Kopyala simgesi) öğesini seçin.

   * Bu GÖNDERI çağrısını yapın:

     `POST https://management.azure.com/{logic-app-resource-ID}/triggers/{endpoint-trigger-name}/listCallbackURL?api-version=2016-06-01`

   * Mantıksal uygulamanızın **genel bakış** bölmesinden geri çağırma URL 'sini kopyalayın.

     1. Mantıksal uygulamanızın menüsünde **genel bakış**' ı seçin.

     1. **Özet** bölümünde **tetikleyici geçmişini göster**' i seçin.

        ![Azure portal uç nokta URL 'sini al](./media/logic-apps-http-endpoint/find-manual-trigger-url.png)

     1. **Geri arama URL 'si [Post]** altında, URL 'yi kopyalayın:

        ![Uç nokta URL 'sini Azure portal Kopyala](./media/logic-apps-http-endpoint/copy-manual-trigger-callback-url-post.png)

<a name="select-method"></a>

## <a name="select-expected-request-method"></a>Beklenen istek metodunu seçin

Varsayılan olarak, Istek tetikleyicisi bir POST isteği bekler. Yalnızca tek bir yöntem beklemeniz için farklı bir yöntem belirtebilirsiniz.

1. Istek tetikleyicisinde **yeni parametre Ekle** listesini açın ve bu özelliği tetikleyicisine ekleyen **Yöntem**' i seçin.

   ![Tetikleyiciye "method" özelliği Ekle](./media/logic-apps-http-endpoint/select-add-new-parameter-for-method.png)

1. **Yöntem** listesinden tetikleyicinin beklediği yöntemi seçin. Ya da özel bir yöntem belirtebilirsiniz.

   Örneğin, bitiş noktanızın URL 'sini daha sonra sınayabilmeniz için **Get** yöntemini seçin.

   ![Tetikleyici tarafından beklenen istek metodunu seçin](./media/logic-apps-http-endpoint/select-method-request-trigger.png)

<a name="endpoint-url-parameters"></a>

## <a name="pass-parameters-through-endpoint-url"></a>Uç nokta URL 'SI aracılığıyla parametreleri geçirme

Bitiş noktasının URL 'SI aracılığıyla parametre değerlerini kabul etmek istediğinizde, şu seçeneklere sahipsiniz:

* [PARAMETRELERI Al veya URL parametreleri aracılığıyla kabul edin](#get-parameters) .

  Bu değerler, uç noktanın URL 'sinde ad-değer çiftleri olarak geçirilir. Bu seçenek için, Istek tetikleyicinizdeki GET yöntemini kullanmanız gerekir. Sonraki bir eylemde, bir ifadede işlevini kullanarak parametre değerlerini tetikleyici çıkışları olarak alabilirsiniz `triggerOutputs()` .

* Istek tetikleyicinizdeki parametreler için [göreli bir yol aracılığıyla değerleri kabul edin](#relative-path) .

  Bu değerler bitiş noktasının URL 'sindeki göreli bir yol üzerinden geçirilir. Ayrıca, tetikleyicinin beklediği [yöntemi açıkça seçmeniz](#select-method) gerekir. Sonraki bir eylemde, bu çıkışlara doğrudan başvurarak parametre değerlerini tetikleyici çıkışları olarak alabilirsiniz.

<a name="get-parameters"></a>

### <a name="accept-values-through-get-parameters"></a>Parametreleri al aracılığıyla kabul et

1. Istek tetikleyicisinde, **yeni parametre Ekle listesini**açın, tetikleyicisine **Yöntem** özelliğini ekleyin ve **Get** yöntemini seçin.

   Daha fazla bilgi için bkz. [beklenen istek metodunu seçin](#select-method).

1. Istek tetikleyicisi altında, parametre değerini kullanmak istediğiniz eylemi ekleyin. Bu örnekte, **Yanıt** eylemini ekleyin.

   1. İstek tetikleyicisi altında **yeni adım**  >  **Eylem Ekle**' yi seçin.
   
   1. **Eylem seçin**altında, arama kutusuna `response` filtreniz olarak girin. Eylemler listesinden, **Yanıt** eylemini seçin.

1. `triggerOutputs()`Parametre değerini alan ifadeyi oluşturmak için aşağıdaki adımları izleyin:

   1. Dinamik içerik listesi görünecek şekilde yanıt eyleminin **gövde** özelliğinin içine tıklayın ve **ifade**' i seçin.

   1. **İfade** kutusuna bu ifadeyi girip `parameter-name` parametre adınızla değiştirin ve **Tamam**' ı seçin.

      `triggerOutputs()['queries']['parameter-name']`

      ![Tetiklenecek "Triggerçıktılar ()" ifadesini ekleyin](./media/logic-apps-http-endpoint/trigger-outputs-expression.png)

      **Body** özelliğinde, ifade `triggerOutputs()` belirtece çözümleniyor.

      !["Triggerçıktılar ()" ifadesi çözümlendi](./media/logic-apps-http-endpoint/trigger-outputs-expression-token.png)

      Mantıksal uygulamayı kaydeder, tasarımcıdan uzaklaşın ve tasarımcıya geri döndüğünüzde, belirteç belirttiğiniz parametre adını gösterir, örneğin:

      ![Parametre adı için çözümlenen ifade](./media/logic-apps-http-endpoint/resolved-expression-parameter-token.png)

      Kod görünümünde, **gövde** özelliği yanıt eyleminin tanımında aşağıdaki şekilde görünür:

      `"body": "@{triggerOutputs()['queries']['parameter-name']}",`

      Örneğin, adlı bir parametre için bir değer geçirmek istediğinizi varsayalım `postalCode` . **Body** özelliği, `Postal Code: ` izleyen bir boşluk ile, ardından karşılık gelen ifade olan dizeyi belirtir:

      ![Tetiklenecek "Triggerçıktılar ()" ifadesini ekleyin](./media/logic-apps-http-endpoint/trigger-outputs-expression-postal-code.png)

1. Çağrılabilir uç noktanızı test etmek için, Istek tetikleyicisinden geri çağırma URL 'sini kopyalayın ve URL 'YI başka bir tarayıcı penceresine yapıştırın. URL 'de, soru işaretini () izleyen parametre adını ve değerini `?` aşağıdaki BIÇIMDE URL 'ye ekleyin ve ENTER tuşuna basın.

   `...?{parameter-name=parameter-value}&api-version=2016-10-01...`

   `https://prod-07.westus.logic.azure.com:433/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke?{parameter-name=parameter-value}&api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}`

   Tarayıcı şu metinle bir yanıt döndürür:`Postal Code: 123456`

   ![İsteği geri çağırma URL 'sine gönderme yanıtı](./media/logic-apps-http-endpoint/callback-url-returned-response.png)

1. Parametre adını ve değerini URL içindeki farklı bir konuma yerleştirmek için, ampersan ( `&` ) önekini önek olarak kullandığınızdan emin olun, örneğin:

   `...?api-version=2016-10-01&{parameter-name=parameter-value}&...`

   Bu örnekte, `postalCode=123456` URL içindeki farklı konumlarda örnek parametre adı ve değeri ile geri çağırma URL 'si gösterilmektedir:

   * ilk konum:`https://prod-07.westus.logic.azure.com:433/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke?postalCode=123456&api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}`

   * 2. konum:`https://prod-07.westus.logic.azure.com:433/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke?api-version=2016-10-01&postalCode=123456&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}`

> [!NOTE]
> URI 'ye karma veya sterlin simgesini () eklemek istiyorsanız **#** bunun yerine bu kodlanmış sürümü kullanın:`%25%23`

<a name="relative-path"></a>

### <a name="accept-values-through-a-relative-path"></a>Değerleri göreli bir yol üzerinden kabul et

1. Istek tetikleyicisinde **yeni parametre Ekle** listesini açın ve bu özelliği **tetikleyiciye ekleyen göreli yol**' ı seçin.

   ![Tetikleyiciye "göreli yol" özelliği Ekle](./media/logic-apps-http-endpoint/select-add-new-parameter-for-relative-path.png)

1. **Göreli yol** ÖZELLIĞINDE, JSON ŞEMANıZDA URL 'nizin kabul etmesini istediğiniz parametrenin göreli yolunu belirtin, örneğin, `/address/{postalCode}` .

   ![Parametre için göreli yolu belirtin](./media/logic-apps-http-endpoint/relative-path-url-value.png)

1. Istek tetikleyicisi altında, parametre değerini kullanmak istediğiniz eylemi ekleyin. Bu örnekte, **Yanıt** eylemini ekleyin.

   1. İstek tetikleyicisi altında **yeni adım**  >  **Eylem Ekle**' yi seçin.

   1. **Eylem seçin**altında, arama kutusuna `response` filtreniz olarak girin. Eylemler listesinden, **Yanıt** eylemini seçin.

1. Yanıt eyleminin **gövde** özelliğinde, tetikleyicinizin göreli yolunda belirttiğiniz parametreyi temsil eden belirteci ekleyin.

   Örneğin, yanıt eyleminin dönmesini istediğinizi varsayalım `Postal Code: {postalCode}` .

   1. **Body** özelliğine `Postal Code: ` bir sonundaki boşluk girin. Dinamik içerik listesi açık kalacak şekilde imlecinizi düzenleme kutusunun içinde tutun.

   1. Dinamik içerik listesinde, **http isteği alındığında** bölümünde, **PostaKodu** belirtecini seçin.

      ![Belirtilen parametreyi yanıt gövdesine ekleyin](./media/logic-apps-http-endpoint/relative-url-with-parameter-token.png)

      **Body** özelliği artık seçili parametreyi içeriyor:

      ![Parametreli örnek yanıt gövdesi](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

1. Mantıksal uygulamanızı kaydedin.

   Istek tetikleyicisinde, geri çağırma URL 'SI güncellenir ve şimdi göreli yolu içerir, örneğin:

   `https://prod-07.westus.logic.azure.com/workflows/{logic-app-resource-ID}/triggers/manual/paths/invoke/address/{postalCode}?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={shared-access-signature}`

1. Çağrılabilir uç noktanızı test etmek için, Istek tetikleyicisinden güncelleştirilmiş geri çağırma URL 'sini kopyalayın, URL 'yi başka bir tarayıcı penceresine yapıştırın, `{postalCode}` URL 'de ile değiştirin `123456` ve ENTER tuşuna basın.

   Tarayıcı şu metinle bir yanıt döndürür:`Postal Code: 123456`

   ![İsteği geri çağırma URL 'sine gönderme yanıtı](./media/logic-apps-http-endpoint/callback-url-returned-response.png)

> [!NOTE]
> URI 'ye karma veya sterlin simgesini () eklemek istiyorsanız **#** bunun yerine bu kodlanmış sürümü kullanın:`%25%23`

## <a name="call-logic-app-through-endpoint-url"></a>Uç nokta URL 'SI aracılığıyla mantıksal uygulama çağırma

Uç noktasını oluşturduktan sonra, `POST` uç noktanın tam URL 'sine BIR https isteği göndererek mantıksal uygulamayı tetikleyebilirsiniz. Logic Apps, doğrudan erişimli uç noktalar için yerleşik desteğe sahiptir.

<a name="generated-tokens"></a>

## <a name="tokens-generated-from-schema"></a>Şemadan oluşturulan belirteçler

Istek tetikleyicisinde bir JSON şeması sağladığınızda, mantıksal uygulama Tasarımcısı bu şemadaki özellikler için belirteçler oluşturur. Daha sonra bu belirteçleri, mantıksal uygulama iş akışınız aracılığıyla veri geçirmek için kullanabilirsiniz.

Örneğin, JSON şemanıza gibi daha fazla özellik eklerseniz, `"suite"` Bu özelliklerin belirteçleri mantıksal uygulamanıza yönelik sonraki adımlarda kullanabileceğiniz şekilde kullanılabilir. Tüm JSON şeması aşağıda verilmiştir:

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

## <a name="create-nested-logic-apps"></a>İç içe mantıksal uygulamalar oluşturma

İstekleri alabilen diğer mantıksal uygulamaları ekleyerek mantıksal uygulamanıza iş akışlarını iç içe yerleştirebilirsiniz. Bu mantıksal uygulamaları eklemek için aşağıdaki adımları izleyin:

1. Başka bir mantıksal uygulamayı çağırmak istediğiniz adım altında **yeni adım**  >  **Eylem Ekle**' yi seçin.

1. **Eylem seçin**altında, **yerleşik**' i seçin. Arama kutusuna `logic apps` filtreniz olarak yazın. Eylemler listesinden **Logic Apps iş akışı**Seç ' i seçin.

   ![Geçerli mantıksal uygulamanın içinde iç içe mantıksal uygulama](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

   Tasarımcı, seçmeniz için uygun mantıksal uygulamaları gösterir.

1. Geçerli mantıksal uygulamanızdan çağrılacak mantıksal uygulamayı seçin.

   ![Geçerli mantıksal uygulamadan çağırmak için mantıksal uygulama seçin](./media/logic-apps-http-endpoint/select-logic-app-to-nest.png)

## <a name="reference-content-from-an-incoming-request"></a>Gelen bir istekten başvuru içeriği

Gelen isteğin içerik türü ise `application/json` , gelen istekteki özelliklere başvurabilirsiniz. Aksi takdirde, bu içerik diğer API 'lere geçirebilmeniz için tek bir ikili birim olarak değerlendirilir. Mantıksal uygulamanızın iş akışı içindeki bu içeriğe başvurmak için, önce bu içeriği dönüştürmeniz gerekir.

Örneğin, türü olan içerik `application/xml` geçirmekte olduğunuz bir XPath ayıklama işlemini gerçekleştirmek için [ `@xpath()` IFADESINI](../logic-apps/workflow-definition-language-functions-reference.md#xpath) KULLANABILIR veya XML 'yi JSON 'ye dönüştürmek için [ `@json()` ifadesini](../logic-apps/workflow-definition-language-functions-reference.md#json) kullanabilirsiniz. Desteklenen [içerik türleriyle](../logic-apps/logic-apps-content-type.md)çalışma hakkında daha fazla bilgi edinin.

Gelen bir istekten gelen çıktıyı almak için, [ `@triggerOutputs` ifadesini](../logic-apps/workflow-definition-language-functions-reference.md#triggerOutputs)kullanabilirsiniz. Örneğin, şu örneğe benzer bir çıktı olduğunu varsayalım:

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

Özellikle özelliğine erişmek için `body` , [ `@triggerBody()` ifadesini](../logic-apps/workflow-definition-language-functions-reference.md#triggerBody) bir kısayol olarak kullanabilirsiniz.

## <a name="respond-to-requests"></a>İsteklere yanıt verme

Bazen, çağrı yapana içerik döndürerek mantıksal uygulamanızı tetikleyen belirli isteklere yanıt vermek isteyebilirsiniz. Yanıtınız için durum kodunu, üstbilgiyi ve gövdesini oluşturmak için, yanıt eylemini kullanın. Bu eylem, yalnızca iş akışınızın sonunda değil, mantıksal uygulamanızda herhangi bir yerde görünebilir. Mantıksal uygulamanız bir yanıt eylemi içermiyorsa, uç nokta **202 kabul edilen** durumla *hemen* yanıt verir.

Özgün çağıranın yanıtı başarıyla alması için, tetiklenen mantıksal uygulama iç içe mantıksal uygulama olarak çağrıdıkça yanıt için gereken tüm adımların [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md) içinde bitmesi gerekir. Bu sınır içinde yanıt döndürülmezse, gelen istek zaman aşımına uğrar ve **408 istemci zaman aşımı** yanıtını alır.

İç içe mantıksal uygulamalar için, gerekli olan sürenin ne olursa olsun, ana mantıksal uygulama, tüm adımlar tamamlanana kadar yanıt beklemeye devam eder.

### <a name="construct-the-response"></a>Yanıtı oluşturun

Yanıt gövdesinde, birden fazla üstbilgiyi ve herhangi bir içerik türünü dahil edebilirsiniz. Örneğin, bu yanıtın üst bilgisi, yanıtın içerik türünün olduğunu `application/json` ve gövdenin, `town` `postalCode` istek tetikleyicisi için bu konuda daha önce açıklanan JSON şemasına dayanarak, ve özellikleri için değerler içerdiğini belirtir.

![HTTPS yanıt eylemi için yanıt içeriği sağlama](./media/logic-apps-http-endpoint/content-for-response-action.png)

Yanıtlar şu özelliklere sahiptir:

| Özellik (görüntüleme) | Özellik (JSON) | Açıklama |
|--------------------|-----------------|-------------|
| **Durum kodu** | `statusCode` | Gelen istek için yanıtta kullanılacak HTTPS durum kodu. Bu kod, 2xx, 4xx veya 5xx ile başlayan geçerli bir durum kodu olabilir. Ancak, 3xx durum kodlarına izin verilmez. |
| **Üst bilgiler** | `headers` | Yanıta eklenecek bir veya daha fazla üst bilgi |
| **Gövde** | `body` | Bir dize, JSON nesnesi veya hatta önceki adımdan başvurulan ikili içerik olabilecek bir gövde nesnesi |
||||

Yanıt eyleminin JSON tanımını ve mantıksal uygulamanızın tamamlanmış JSON tanımını görüntülemek için mantıksal uygulama Tasarımcısı araç çubuğunda **kod görünümü**' nü seçin.

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

#### <a name="q-what-about-url-security"></a>S: URL güvenliği hakkında ne olacak?

Y **: Azure**, [paylaşılan ERIŞIM imzasını (SAS)](https://docs.microsoft.com/rest/api/storageservices/delegate-access-with-shared-access-signature)kullanarak mantıksal uygulama geri çağırma URL 'lerini güvenli şekilde oluşturur. Bu imza bir sorgu parametresi olarak geçirilir ve mantıksal uygulamanızın çalıştırılabilmesi için önce doğrulanması gerekir. Azure, mantıksal uygulama başına bir gizli anahtar birleşimi, tetikleyici adı ve gerçekleştirilen işlem ile imza oluşturur. Bu nedenle, gizli mantıksal uygulama anahtarına birisi erişemediği takdirde, geçerli bir imza üretemiyor.

> [!IMPORTANT]
> Üretim ve daha yüksek güvenlik sistemleri için aşağıdaki nedenlerden dolayı mantıksal uygulamanızı doğrudan tarayıcıdan çağırmayı önemle tavsiye ederiz:
>
> * Paylaşılan erişim anahtarı URL 'de görüntülenir.
> * Azure Logic Apps müşteriler genelinde paylaşılan etki alanları nedeniyle güvenlik içeriği ilkelerini yönetemezsiniz.

#### <a name="q-can-i-configure-callable-endpoints-further"></a>S: çağrılabilir uç noktaları başka bir şekilde yapılandırabilir miyim?

Y **: Evet**, HTTPS uç noktaları [Azure API Management](../api-management/api-management-key-concepts.md)aracılığıyla daha gelişmiş yapılandırmayı destekler. Bu hizmet ayrıca Logic Apps, özel etki alanı adları ayarlama, daha fazla kimlik doğrulama yöntemi kullanma ve daha fazlasını içeren tüm API 'lerinizi sürekli olarak yönetmenize olanak sağlar. Örneğin:

* [İstek yöntemini değiştirme](../api-management/api-management-advanced-policies.md#SetRequestMethod)
* [İsteğin URL segmentlerini değiştirme](../api-management/api-management-transformation-policies.md#RewriteURL)
* [Azure portal](https://portal.azure.com/) API Management etki alanlarınızı ayarlama
* Temel kimlik doğrulamasını denetlemek için ilke ayarlama

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps kullanarak gelen HTTPS çağrılarını alma ve yanıtlama](../connectors/connectors-native-reqres.md)
