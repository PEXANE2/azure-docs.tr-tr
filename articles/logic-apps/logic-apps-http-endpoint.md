---
title: HTTP uç noktaları ile çağrı, tetikleyici veya iç içe iş akışları-Azure Logic Apps
description: Azure Logic Apps için iş akışlarını çağırmak, tetiklemek veya iç içe aktarmak için HTTP uç noktalarını ayarlama
services: logic-apps
ms.service: logic-apps
ms.workload: integration
author: ecfan
ms.author: klam
ms.reviewer: jehollan, klam, LADocs
manager: carmonm
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 03/31/2017
ms.openlocfilehash: f93e90ef442740e4fb17f166023fbe3d5f0bae66
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875947"
---
# <a name="call-trigger-or-nest-workflows-with-http-endpoints-in-azure-logic-apps"></a>Azure Logic Apps içindeki HTTP uç noktaları ile çağrı, tetikleyici veya iç içe geçme

Logic Apps 'te bir URL aracılığıyla tetikleyebilmeniz veya çağrabilmeniz için mantıksal uygulamalarda Tetikleyiciler olarak zaman uyumlu HTTP uç noktalarını yerel olarak kullanıma sunabilirsiniz. Ayrıca, çağrılabilir uç noktaların bir modelini kullanarak mantıksal uygulamalarınızda iş akışlarını iç içe geçirebilirsiniz.

HTTP uç noktaları oluşturmak için, mantıksal uygulamalarınızın gelen istekleri alabilmesi için bu Tetikleyicileri ekleyebilirsiniz:

* [İstek](../connectors/connectors-native-reqres.md)

* [API bağlantısı Web kancası](../logic-apps/logic-apps-workflow-actions-triggers.md#apiconnection-trigger)

* [HTTP Web kancası](../connectors/connectors-native-webhook.md)

   > [!NOTE]
   > Bu örneklerde **istek** tetikleyicisi kullanılsa da, listelenen http tetikleyicilerinin herhangi birini kullanabilir ve diğer tüm ilkeler diğer tetikleyici türleri için de geçerlidir.

## <a name="set-up-an-http-endpoint-for-your-logic-app"></a>Mantıksal uygulamanız için bir HTTP uç noktası ayarlama

Bir HTTP uç noktası oluşturmak için gelen istekleri alabilen bir tetikleyici ekleyin.

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın. Mantıksal uygulamanıza gidin ve mantıksal uygulama Tasarımcısı ' nı açın.

2. Mantıksal uygulamanızın gelen istekleri almasına imkan tanıyan bir tetikleyici ekleyin. Örneğin, mantıksal uygulamanıza **istek** tetikleyicisini ekleyin.

3.  **Istek GÖVDESI JSON şeması**altında, isteğe bağlı olarak, tetikleyicinin almasını beklediğinizi bekleyen yük (veri) IÇIN bir JSON şeması girebilirsiniz.

    Tasarımcı, mantıksal uygulamanızın iş akışınız aracılığıyla tetikleyiciden verileri tüketmek, ayrıştırmak ve geçirebilmesi için kullanabileceği belirteçler oluşturmak için bu şemayı kullanır. 
    [JSON şemalardan oluşturulan belirteçler](#generated-tokens)hakkında daha fazla bilgi.

    Bu örnekte, tasarımcıda gösterilen şemayı girin:

    ```json
    {
      "type": "object",
      "properties": {
        "address": {
          "type": "string"
        }
      },
      "required": [
        "address"
      ]
    }
    ```

    ![Istek eylemini ekleyin][1]

    > [!TIP]
    > 
    > [Jsonschema.net](https://jsonschema.net/)gibi bir araçtan örnek bir JSON yükü için bir şema oluşturabilir veya **şema oluşturmak Için örnek yük kullan**' ı seçerek **istek** tetikleyebilirsiniz. 
    > Örnek yükünüzü girip **bitti**' yi seçin.

    Örneğin, bu örnek yük:

    ```json
    {
       "address": "21 2nd Street, New York, New York"
    }
    ```

    Bu şemayı oluşturur:

    ```json
    {
       "type": "object",
       "properties": {
          "address": {
             "type": "string" 
          }
       }
    }
    ```

4.  Mantıksal uygulamanızı kaydedin. **Bu URL 'ye http post**' ın altında, şu örnekte olduğu gibi oluşturulmuş bir geri çağırma URL 'si bulmanız gerekir:

    ![Uç nokta için geri çağırma URL 'SI oluşturuldu](./media/logic-apps-http-endpoint/generated-endpoint-url.png)

    Bu URL, kimlik doğrulaması için kullanılan sorgu parametrelerinde bir paylaşılan erişim Imzası (SAS) anahtarı içerir. 
    Ayrıca, Azure portal mantıksal uygulamanıza genel bakış üzerinden HTTP uç noktası URL 'sini de alabilirsiniz. **Tetikleyici geçmişi**altında tetikleyiciyi seçin:

    ![Azure portal HTTP uç noktası URL 'SI al][2]

    Ya da bu çağrıyı yaparak URL 'YI edinebilirsiniz:

    ```
    POST https://management.azure.com/{logic-app-resourceID}/triggers/{myendpointtrigger}/listCallbackURL?api-version=2016-06-01
    ```

## <a name="change-the-http-method-for-your-trigger"></a>Tetikleyicinizin HTTP yöntemini değiştirme

Varsayılan olarak, **istek** TETIKLEYICISI BIR http post isteği bekler, ancak farklı bir http yöntemi de kullanabilirsiniz. 

> [!NOTE]
> Yalnızca bir yöntem türü belirtebilirsiniz.

1. **İstek** tetikleyicisinde **Gelişmiş seçenekleri göster**' i seçin.

2. **Yöntem** listesini açın. Bu örnek için, HTTP uç noktanızın URL 'sini daha sonra sınayabilmeniz için **Al** ' ı seçin.

    > [!NOTE]
    > Başka herhangi bir HTTP yöntemini seçebilir veya kendi mantıksal uygulamanız için özel bir yöntem belirtebilirsiniz.

    ![HTTP yöntemini değiştir](./media/logic-apps-http-endpoint/change-method.png)

## <a name="accept-parameters-through-your-http-endpoint-url"></a>HTTP uç noktası URL 'niz aracılığıyla parametreleri kabul etme

HTTP uç noktası URL 'nizin parametreleri kabul etmesini istediğinizde, tetikleyicinizin göreli yolunu özelleştirin.

1. **İstek** tetikleyicisinde **Gelişmiş seçenekleri göster**' i seçin. 

2. **Yöntemi**altında, isteğinizin kullanmasını istediğiniz http yöntemini belirtin. Bu örnek için, henüz yapmadıysanız, HTTP uç noktanızın URL 'nizi test edebilmeniz için **Get** yöntemini seçin.

      > [!NOTE]
      > Tetikleyiciniz için göreli bir yol belirttiğinizde, Tetikleyiciniz için de açıkça bir HTTP yöntemi belirtmeniz gerekir.

3. **Göreli yol**altında URL 'nizin kabul edileceği parametrenin göreli yolunu belirtin, örneğin, `customers/{customerID}`.

    ![Parametre için HTTP yöntemini ve göreli yolu belirtin](./media/logic-apps-http-endpoint/relativeurl.png)

4. Parametresini kullanmak için, mantıksal uygulamanıza bir **Yanıt** eylemi ekleyin. (Tetikleyiciniz altında **yeni adım** > **eylem** > **yanıtı**Ekle ' yi seçin) 

5. Yanıtınızın **gövdesinde**, tetikleyicinizin göreli yolunda belirttiğiniz parametrenin belirtecini ekleyin.

    Örneğin, döndürmek `Hello {customerID}`için, yanıt **gövdesini** ile `Hello {customerID token}`güncelleştirin. 
    Dinamik içerik listesi görünmelidir ve seçtiğiniz `customerID` belirteci gösterir.

    ![Yanıt gövdesine parametre Ekle](./media/logic-apps-http-endpoint/relativeurlresponse.png)

    **Gövde'niz** şu örnekteki gibi görünmelidir:

    ![Parametreli yanıt gövdesi](./media/logic-apps-http-endpoint/relative-url-with-parameter.png)

6. Mantıksal uygulamanızı kaydedin. 

    HTTP uç noktası URL 'niz artık göreli yolu içerir, örneğin: 

    https&#58;//prod-00.southcentralus.Logic.Azure.com/Workflows/f90cb66c52ea4e9cabe0abf4e197deff/Triggers/Manual/Paths/invoke/Customers/{CustomerID}...

7. HTTP uç noktanızı test etmek için, güncelleştirilmiş URL 'yi kopyalayıp başka bir tarayıcı penceresine yapıştırın, ve ile `{customerID}` `123456`değiştirin ve ENTER tuşuna basın.

    Tarayıcınız şu metni göstermelidir: 

    `Hello 123456`

<a name="generated-tokens"></a>

### <a name="tokens-generated-from-json-schemas-for-your-logic-app"></a>Mantıksal uygulamanıza yönelik JSON şemalardan oluşturulan belirteçler

**İstek** Tetikleyiciniz IÇIN bir JSON şeması sağladığınızda, mantıksal uygulama Tasarımcısı bu şemadaki özellikler için belirteçler oluşturur. Daha sonra bu belirteçleri, mantıksal uygulama iş akışınız aracılığıyla veri geçirmek için kullanabilirsiniz.

Bu örnekte, `title` ve `name` özelliklerini JSON şemanıza eklerseniz, belirteçleri daha sonra iş akışı adımlarında kullanılmak üzere kullanılabilir. 

Tüm JSON şeması aşağıda verilmiştir:

```json
{
   "type": "object",
   "properties": {
      "address": {
         "type": "string"
      },
      "title": {
         "type": "string"
      },
      "name": {
         "type": "string"
      }
   },
   "required": [
      "address",
      "title",
      "name"
   ]
}
```

## <a name="create-nested-workflows-for-logic-apps"></a>Mantıksal uygulamalar için iç içe iş akışları oluşturma

İstekleri alabilen diğer mantıksal uygulamaları ekleyerek mantıksal uygulamanıza iş akışlarını iç içe yerleştirebilirsiniz. Bu mantıksal uygulamaları eklemek için, tetikleyicinizin **Azure Logic Apps bir Logic Apps iş akışı eylemi seçin** . Daha sonra uygun Logic Apps arasından seçim yapabilirsiniz.

![Başka bir mantıksal uygulama ekleyin](./media/logic-apps-http-endpoint/choose-logic-apps-workflow.png)

## <a name="call-or-trigger-logic-apps-through-http-endpoints"></a>HTTP uç noktaları aracılığıyla mantıksal uygulamaları çağırma veya tetikleme

HTTP uç noktanızı oluşturduktan sonra, mantıksal uygulamanızı tam URL 'ye bir `POST` yöntem aracılığıyla tetikleyebilirsiniz. Logic Apps, doğrudan erişimli uç noktalar için yerleşik desteğe sahiptir.

> [!NOTE] 
> Mantıksal uygulamayı dilediğiniz zaman el ile çalıştırmak için, mantıksal uygulama Tasarımcısı veya mantıksal uygulama kod görünümü araç çubuğunda **Çalıştır**' ı seçin.

## <a name="reference-content-from-an-incoming-request"></a>Gelen bir istekten başvuru içeriği

İçeriğin türü ise `application/json`, gelen istekten özelliklere başvurabilirsiniz. Aksi takdirde, içerik diğer API 'lere geçirebilmeniz için tek bir ikili birim olarak değerlendirilir. İş akışı içindeki bu içeriğe başvurmak için bu içeriği dönüştürmeniz gerekir. Örneğin, içerik geçirirseniz `application/xml` , bir XPath ayıklama veya `@json()` XML 'i `@xpath()` JSON 'a dönüştürmek için kullanabilirsiniz. [İçerik türleriyle çalışma](../logic-apps/logic-apps-content-type.md)hakkında bilgi edinin.

Gelen bir istekten çıktıyı almak için `@triggerOutputs()` işlevini kullanabilirsiniz. Çıktı şu örnekteki gibi görünebilir:

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

Özellikle `body` özelliğe erişmek için `@triggerBody()` kısayolunu kullanabilirsiniz. 

## <a name="respond-to-requests"></a>İsteklere yanıt verme

Çağrıyı yapana içerik döndürerek mantıksal uygulama başlatan belirli isteklere yanıt vermek isteyebilirsiniz. Yanıtınız için durum kodunu, üstbilgiyi ve gövdesini oluşturmak için, **Yanıt** eylemini kullanabilirsiniz. Bu eylem, yalnızca iş akışınızın sonunda değil, mantıksal uygulamanızda herhangi bir yerde görünebilir.

> [!NOTE] 
> Mantıksal uygulamanız bir **Yanıt**IÇERMIYORSA, HTTP uç noktası *hemen* **202 kabul edilen** bir durumla yanıt verir. Ayrıca, yanıtı almak için özgün istek için, iş akışını iç içe bir mantıksal uygulama olarak çağırmadığınız sürece yanıt için gereken tüm adımların [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md) içinde bitmesi gerekir. Bu sınırın içinde yanıt yoksa, gelen istek zaman aşımına uğrar ve HTTP yanıt **408 istemci zaman aşımını**alır. İç içe mantıksal uygulamalar için, ne kadar süre gerekli olursa olsun, ana mantıksal uygulama tamamlanana kadar bir yanıt beklemeye devam eder.

### <a name="construct-the-response"></a>Yanıtı oluşturun

Yanıt gövdesine birden fazla üstbilgi ve herhangi bir içerik türü dahil edebilirsiniz. Örnek yanıtta, üst bilgi yanıtın içerik türüne `application/json`sahip olduğunu belirtir. ve gövde, daha `title` önce `name` **istek** tetikleyicisi için güncelleştirilmiş JSON şemasına göre ve içerir.

![HTTP yanıt eylemi][3]

Yanıtlar şu özelliklere sahiptir:

| Özellik | Açıklama |
| --- | --- |
| Durum |Gelen isteğe yanıt vermek için HTTP durum kodunu belirtir. Bu kod, 2xx, 4xx veya 5xx ile başlayan geçerli bir durum kodu olabilir. Ancak, 3xx durum kodlarına izin verilmez. |
| Bilgisinde |Yanıta dahil edilecek herhangi bir sayıda üstbilgiyi tanımlar. |
| bölümü |Bir dize, JSON nesnesi veya hatta bir önceki adımdan başvurulan ikili içerik olabilecek bir Body nesnesini belirtir. |

**Yanıt** eylemi için şu anda JSON şeması şöyle görünür:

``` json
"Response": {
   "inputs": {
      "body": {
         "title": "@{triggerBody()?['title']}",
         "name": "@{triggerBody()?['name']}"
      },
      "headers": {
           "content-type": "application/json"
      },
      "statusCode": 200
   },
   "runAfter": {},
   "type": "Response"
}
```

> [!TIP]
> Mantıksal uygulamanızın tüm JSON tanımını görüntülemek için mantıksal uygulama Tasarımcısı 'nda **kod görünümü**' ne tıklayın.

## <a name="q--a"></a>Soru - Yanıt

#### <a name="q-what-about-url-security"></a>S: URL güvenliği hakkında ne olacak?

Y: Azure, paylaşılan erişim Imzası (SAS) kullanarak mantıksal uygulama geri çağırma URL 'Lerini güvenli şekilde oluşturur. Bu imza bir sorgu parametresi olarak geçirilir ve mantıksal uygulamanızın tetiklenmesi için doğrulanması gerekir. Azure, mantıksal uygulama başına bir gizli anahtar birleşimi, tetikleyici adı ve gerçekleştirilen işlem ile imza oluşturur. Bu nedenle, gizli mantıksal uygulama anahtarına birisi erişemediği takdirde, geçerli bir imza üretemiyor.

   > [!IMPORTANT]
   > Üretim ve güvenli sistemler için, mantıksal uygulamanızı doğrudan tarayıcıdan çağırmaya kesinlikle önerilir, çünkü:
   > 
   > * Paylaşılan erişim anahtarı URL 'de görüntülenir.
   > * Mantıksal uygulama müşterileri genelinde paylaşılan etki alanları nedeniyle güvenli içerik ilkelerini yönetemezsiniz.

#### <a name="q-can-i-configure-http-endpoints-further"></a>S: HTTP uç noktalarını daha fazla yapılandırabilir miyim?

Y: Evet, HTTP uç noktaları [**API Management**](../api-management/api-management-key-concepts.md)aracılığıyla daha gelişmiş yapılandırmayı destekler. Bu hizmet ayrıca Logic Apps, özel etki alanı adları ayarlama, daha fazla kimlik doğrulama yöntemi kullanma ve daha fazlasını içeren tüm API 'lerinizi sürekli olarak yönetmenize olanak sağlar. Örneğin:

* [İstek yöntemini değiştirme](https://docs.microsoft.com/azure/api-management/api-management-advanced-policies#SetRequestMethod)
* [İsteğin URL segmentlerini değiştirme](https://docs.microsoft.com/azure/api-management/api-management-transformation-policies#RewriteURL)
* [Azure portal]API Management etki alanlarınızı ayarlayın(https://portal.azure.com/ "Azure Portal")
* Temel kimlik doğrulamasını denetlemek için ilke ayarlama

#### <a name="q-what-changed-when-the-schema-migrated-from-the-december-1-2014-preview"></a>S: Şema 1 Aralık 2014 önizlemesinden geçirildiğinde ne değişti?

Y: Bu değişiklikler hakkında bir özet aşağıda verilmiştir:

| 1 Aralık 2014 Preview | 1 Haziran 2016 |
| --- | --- |
| **Http dinleyicisi** API uygulaması ' na tıklayın |**El ile tetikleme** (API uygulaması gerekmez) seçeneğine tıklayın |
| "*Yanıtı otomatik olarak gönderir*" http dinleyicisi ayarı |Bir **Yanıt** eylemi ekleyin veya iş akışı tanımında değil |
| Temel veya OAuth kimlik doğrulamasını yapılandırma |API Management aracılığıyla |
| HTTP yöntemini yapılandırma |**Gelişmiş seçenekleri göster**altında bir http yöntemi seçin |
| Göreli yolu Yapılandır |**Gelişmiş seçenekleri göster**altında göreli bir yol ekleyin |
| Gelen gövdeye şu şekilde başvur`@triggerOutputs().body.Content` |Başvuru`@triggerOutputs().body` |
| Http dinleyicisinde **http yanıtı gönderme** eylemi |**Http Isteğine Yanıtla** ' ya tıklayın (API uygulaması gerekmez) |

## <a name="get-help"></a>Yardım alın

Sorular sormak, soruları yanıtlamak ve diğer Azure Logic Apps kullanıcılarının neler yaptığını öğrenmek için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

Azure Logic Apps ve bağlayıcıları geliştirmeye yardımcı olmak için, [Azure Logic Apps kullanıcı geri bildirim sitesinde](https://aka.ms/logicapps-wish) oy kullanın veya fikirlerinizi paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal uygulama tanımları yazma](./logic-apps-author-definitions.md)
* [Hataları ve özel durumları işleme](./logic-apps-exception-handling.md)

[1]: ./media/logic-apps-http-endpoint/manualtrigger.png
[2]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[3]: ./media/logic-apps-http-endpoint/response.png