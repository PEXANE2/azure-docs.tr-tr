---
title: HTTP veya HTTPS isteklerine yanıt verme-Azure Logic Apps
description: Azure Logic Apps kullanarak, HTTP veya HTTPS üzerinden olaylara gerçek zamanlı olarak yanıt verin
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
ms.openlocfilehash: c707326c810f111db220be1ffdfbcd4a52dbb2d6
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70898414"
---
# <a name="respond-to-http-or-https-requests-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak HTTP veya HTTPS isteklerine yanıt verme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve yerleşik istek tetikleyicisi veya yanıtı EYLEMIYLE, http veya https üzerinden gönderilen olaylara gerçek zamanlı olarak tepki veren ve yanıt veren otomatikleştirilmiş görevler ve iş akışları oluşturabilirsiniz. Örneğin, mantıksal uygulamanızı kullanabilirsiniz:

* Şirket içi bir veritabanındaki veriler için HTTP isteğine yanıt verme.
* Dış Web kancası olayı gerçekleştiğinde iş akışı tetikleyin.
* Başka bir mantıksal uygulamanın içinden bir mantıksal uygulama çağırın.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolabilirsiniz](https://azure.microsoft.com/free/).

* [Logic Apps](../logic-apps/logic-apps-overview.md)hakkında temel bilgi. Logic Apps 'e yeni başladıysanız, [ilk mantıksal uygulamanızı oluşturmayı](../logic-apps/quickstart-create-first-logic-app-workflow.md)öğrenin.

<a name="add-request"></a>

## <a name="add-the-request-trigger"></a>Istek tetikleyicisini ekleme

Bu yerleşik tetikleyici, HTTP veya HTTPS üzerinden gelen bir isteği bekleyen ve alan bir uç nokta oluşturur. Bu olay gerçekleştiğinde tetikleyici ateşlenir ve mantıksal uygulamayı çalıştırır.

1. [Azure Portal](https://portal.azure.com) oturum açın. Boş bir mantıksal uygulama oluşturma.

1. Mantıksal uygulama Tasarımcısı açıldıktan sonra, arama kutusuna filtreniz olarak "http isteği" yazın. Tetikleyiciler listesinden, mantıksal uygulama iş akışınızın ilk adımı olan **BIR http isteği alındığında** tetiklenir ' ı seçin.

   ![HTTP istek tetikleyicisini seçme](./media/connectors-native-reqres/select-request-trigger.png)

1. **Istek GÖVDESI JSON şeması** kutusu için isteğe bağlı olarak, gelen ISTEKTE beklenen http istek gövdesini açıklayan bir JSON şeması girebilirsiniz, örneğin:

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

1. İstek için kullanılan beklenen yöntem veya göreli bir yol gibi ek özellikler belirtmek için **yeni parametre Ekle** listesini açın ve eklemek istediğiniz parametreleri seçin.

   Bu örnek, **yöntemi** parametresini ekler:

   ![Yöntem parametresi Ekle](./media/connectors-native-reqres/add-parameters.png)

   Artık, listeden bir yöntem seçebilmeniz için tetikleyici içinde **Yöntem** parametresi belirir.

   ![Yöntem seçin](./media/connectors-native-reqres/select-method.png)

1. Şimdi, iş akışınızda sonraki adım olarak başka bir eylem ekleyin. Tetikleyici altında, eklemek istediğiniz eylemi bulabilmeniz için bir **sonraki adım** ' ı seçin.

   Örneğin, bu konunun ilerleyen kısımlarında açıklanan [bir yanıt eylemi ekleyerek](#add-response)isteğe yanıt verebilirsiniz.

   > [!NOTE]
   > Mantıksal uygulama iş akışınızda bir yanıt eylemi eklemezseniz ve tanımlayamazsınız, mantıksal uygulamanız doğrudan çağırana bir `202 ACCEPTED` yanıt döndürür. Yanıtı özelleştirmek için yanıt eylemini kullanabilirsiniz.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin. 

   Bu adım, mantıksal uygulamayı tetikleyen isteği göndermek için kullanılacak URL 'YI oluşturur. Bu URL 'yi kopyalamak için URL 'nin yanındaki Kopyala simgesini seçin.

   ![Mantıksal uygulamanızı tetiklemenin kullanılacağı URL](./media/connectors-native-reqres/generated-url.png)

1. Mantıksal uygulamanızı tetiklemek için, oluşturulan URL 'ye bir HTTP GÖNDERISI gönderin. Örneğin [Postman](https://www.getpostman.com/)gibi bir araç kullanabilirsiniz.

### <a name="trigger-outputs"></a>Tetikleme çıkışları

Istek tetikleyicisinden alınan çıktılar hakkında daha fazla bilgi aşağıda verilmiştir:

| Özellik adı | Veri türü | Açıklama |
|---------------|-----------|-------------|
| `headers` | Object | İstekten üstbilgileri açıklayan bir JSON nesnesi |
| `body` | Object | İstekten gelen gövde içeriğini açıklayan bir JSON nesnesi |
||||

<a name="add-response"></a>

## <a name="add-a-response-action"></a>Yanıt eylemi ekleme

Yanıt eylemini yalnızca bir HTTP isteği tarafından tetiklenen bir Logic App iş akışında bir yanıtı özelleştirmek için kullanabilirsiniz. Yanıt eylemini iş akışınızın herhangi bir noktasına ekleyebilirsiniz.

Mantıksal uygulamanız gelen isteği yalnızca bir dakika boyunca açık tutar. Mantıksal uygulama iş akışınızın bir yanıt eylemi içerdiğini varsayarsak, mantıksal uygulama bu süre geçtikten sonra bir yanıt döndürmezse, mantıksal uygulamanız bir `504 GATEWAY TIMEOUT` arayana döndürür. Ancak, mantıksal uygulamanız bir yanıt eylemi içermiyorsa, mantıksal uygulamanız hemen çağırana bir `202 ACCEPTED` yanıt döndürür. 

1. Mantıksal uygulama Tasarımcısı ' nda, yanıt eklemek istediğiniz adım altında **yeni adım**' ı seçin.

   Örneğin, daha önce Istek tetikleyicisi kullanımı:

   ![Yeni adım Ekle](./media/connectors-native-reqres/add-response.png)

   Adımlar arasında bir eylem eklemek için, işaretçinizi Bu adımlar arasındaki oka taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

1. **Eylem seçin**altında, arama kutusuna filtreniz olarak "yanıt" yazın ve **Yanıt** eylemini seçin.

   ![Yanıt eylemini seçin](./media/connectors-native-reqres/select-response-action.png)

   Istek tetikleyicisi kolaylık sağlaması için bu örnekte daraltılır.

1. Yanıt iletisi için gereken tüm değerleri ekleyin. 

   Bazı alanlarda, kutularının içine tıklanması dinamik içerik listesini açar. Daha sonra iş akışındaki önceki adımlardan bulunan mevcut çıkışları temsil eden belirteçleri seçebilirsiniz. Önceki örnekte belirtilen şemadan özellikler artık dinamik içerik listesinde görünür.

   Örneğin, **üst bilgiler** alanında, `Content-Type` bu konuda daha önce bahsedildiği gibi, anahtar adını ve anahtar değerini `application/json`olarak ayarlayın. **Gövde** alanı için dinamik içerik listesinden tetikleyici gövdesini seçebilirsiniz.

   ![Yanıt eylemi ayrıntıları](./media/connectors-native-reqres/response-details.png)

   Üst bilgileri JSON biçiminde görüntülemek için **metin görünümüne geç**' i seçin.

   ![Üstbilgiler-metin görünümüne geç](./media/connectors-native-reqres/switch-to-text-view.png)

   Yanıt eyleminde ayarlayabileceğiniz özellikler hakkında daha fazla bilgi bulabilirsiniz. 

   | Display name | Özellik adı | Gerekli | Açıklama |
   |--------------|---------------|----------|-------------|
   | Durum Kodu | `statusCode` | Evet | Döndürülecek HTTP durum kodu |
   | Üst bilgiler | `headers` | Hayır | Dahil edilecek yanıt üstbilgilerini açıklayan bir JSON nesnesi.  |
   | Body | `body` | Hayır | Yanıt gövdesi |
   |||||

1. Yanıt için JSON şeması gibi ek özellikler belirtmek için **yeni parametre Ekle** listesini açın ve eklemek istediğiniz parametreleri seçin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps için bağlayıcılar](../connectors/apis-list.md)