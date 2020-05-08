---
title: B2B kurumsal tümleştirme senaryoları için Exchange iletileri
description: Enterprise Integration Pack kullanarak Azure Logic Apps ticari iş ortakları arasında B2B iletileri alın ve gönderin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/10/2020
ms.openlocfilehash: b576fc99e2f203bb3d690a8135ee76cee26b3de8
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792369"
---
# <a name="receive-and-confirm--b2b-as2-messages-by-using-azure-logic-apps-and-enterprise-integration-pack"></a>Azure Logic Apps ve Enterprise Integration Pack kullanarak B2B AS2 iletileri alma ve onaylama

Ticari iş ortakları ve anlaşmaları tanımlayan bir tümleştirme hesabınız olduğunda, [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) [Azure Logic Apps](../logic-apps/logic-apps-overview.md) kullanarak ticari iş ortakları arasında ileti alışverişi yapan bir OTOMATIK iş kolu (B2B) iş akışı oluşturabilirsiniz. Azure Logic Apps, AS2, x12, EDIOLGU ve RosettaNet sektör standardı protokollerini destekleyen bağlayıcılarla birlikte kullanılabilir. Ayrıca, bu bağlayıcıları Logic Apps, örneğin Salesforce ve Office 365 Outlook gibi diğer [Bağlayıcılar](../connectors/apis-list.md)ile birleştirebilirsiniz.

Bu makalede, bir Istek tetikleyicisi kullanarak HTTP isteği alan ve AS2 ve x12 eylemlerini kullanarak ileti içeriğinin kodunu çözen ve yanıt eylemini kullanarak bir yanıt döndüren mantıksal uygulamanın nasıl oluşturulacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bu eylemler tarafından izlenen [istek](../connectors/connectors-native-reqres.md) TETIKLEYICISINI kullanarak B2B iş akışını oluşturabilmeniz için boş bir mantıksal uygulama:

  * [AS2 kodunu çöz](../logic-apps/logic-apps-enterprise-integration-as2.md)

  * AS2 kod çözme eyleminin başarılı veya başarısız olmasına bağlı olarak bir [Yanıt](../connectors/connectors-native-reqres.md) Gönderen [koşul](../logic-apps/logic-apps-control-flow-conditional-statement.md)

  * [X12 iletisinin kodunu çöz](../logic-apps/logic-apps-enterprise-integration-x12.md) 

  Logic Apps 'e yeni başladıysanız, [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç: Ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)' yı gözden geçirin.

* Azure aboneliğinizle ilişkili ve mantıksal uygulamanıza bağlanmış bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-accounts.md) . Hem mantıksal uygulamanız hem de tümleştirme hesabınız aynı konumda veya Azure bölgesinde bulunmalıdır.

* Tümleştirme hesabınızda zaten tanımlamış olduğunuz en az iki [ticari iş ortağı](../logic-apps/logic-apps-enterprise-integration-partners.md) , bu iş ortakları için [AS2 ve x12 sözleşmeleri](logic-apps-enterprise-integration-agreements.md) ile birlikte.

## <a name="add-request-trigger"></a>Istek tetikleyicisi Ekle

Bu örnek Azure portal Logic App Designer 'ı kullanır, ancak Visual Studio 'daki Logic App Designer için benzer adımları izleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com), mantıksal uygulama tasarımcısında boş mantıksal uygulamanızı açın.

1. Arama kutusuna, öğesini girin `when a http request`ve tetikleyici olarak kullanılacak **bir http isteği alındığında** öğesini seçin.

   ![Mantıksal uygulama iş akışınızı başlatmak için Istek tetikleyicisi seçin](./media/logic-apps-enterprise-integration-b2b/select-http-request-trigger.png)

1. X12 iletisi düz bir dosya olduğundan, **istek GÖVDESI JSON şeması** kutusunu boş bırakın.

   !["Istek gövdesi JSON şeması" boş bırakın](./media/logic-apps-enterprise-integration-b2b/receive-trigger-message-body-json-schema.png)

1. İşiniz bittiğinde, Tasarımcı araç çubuğunda **Kaydet**' i seçin.

   Bu adım, mantıksal uygulamayı tetikleyen isteği göndermek için kullanılacak **http post URL 'sini** oluşturur. Bu URL 'yi kopyalamak için URL 'nin yanındaki Kopyala simgesini seçin.

   ![Çağrıları almak için Istek tetikleyicisi için oluşturulan URL](./media/logic-apps-enterprise-integration-b2b/generated-url-request-trigger.png)

## <a name="add-as2-decode-action"></a>AS2 kod çözme eylemi Ekle

Şimdi kullanmak istediğiniz B2B eylemlerini ekleyin. Bu örnek AS2 ve x12 eylemlerini kullanır.

1. Tetikleyici altında **yeni adım**' ı seçin. Tetikleyici ayrıntılarını gizlemek için tetikleyicinin başlık çubuğuna tıklayın.

   ![Mantıksal uygulama iş akışınıza başka bir adım ekleyin](./media/logic-apps-enterprise-integration-b2b/add-new-action-under-trigger.png)

1. **Eylem seçin**altında, arama kutusuna girin `as2 decode`ve **AS2 kodunu çöz (v2)** seçeneğini belirleyin.

   !["AS2 kodunu çöz (v2)" öğesini bulun ve seçin](./media/logic-apps-enterprise-integration-b2b/add-as2-decode-action.png)

1. **İleti kodunu çözmek** IÇIN, AS2 eyleminin kodu kodunu çözmek ISTEDIĞINIZ, http istek tetikleyicisi tarafından alınan `body` içerik olan girdiyi girin. Bu içeriği, dinamik içerik listesinden ya da bir ifade olarak belirtmek için birden çok yol vardır:

   * Kullanılabilir tetikleyici çıkışlarını gösteren bir listeden seçmek için, **kodu çözmek Için ileti** kutusuna tıklayın. Dinamik içerik listesi görüntülendikten sonra, **BIR http isteği alındığında**altında **gövde** özelliği değeri ' ni seçin, örneğin:

     ![Tetikleyiciden "gövde" değeri Seç](./media/logic-apps-enterprise-integration-b2b/select-body-content-from-trigger.png)

   * Tetikleyicinin `body` çıktısına başvuran bir ifade girmek için, **kodu çözmek için ileti** kutusuna tıklayın. Dinamik içerik listesi görüntülendikten sonra **ifade**' i seçin. İfade düzenleyicisinde, ifadeyi buraya girin ve **Tamam**' ı seçin:

     `triggerOutputs()['body']`

     Veya **kod çözme iletisi** kutusuna doğrudan bu ifadeyi girin:

     `@triggerBody()`

     İfade, **gövde** belirtecine çözümleniyor.

     ![Tetikleyiciden gelen gövde çıkışı çözüldü](./media/logic-apps-enterprise-integration-b2b/resolved-trigger-outputs-body-expression.png)

1. **İleti üstbilgileri** özelliği IÇIN, http istek tetikleyicisi tarafından alınan `headers` içerik tarafından tanımlanan AS2 eylemi için gereken tüm üst bilgileri girin.

   Tetikleyicinin `headers` çıktısına başvuran bir ifade girmek için **ileti üstbilgileri** kutusunun içine tıklayın. Dinamik içerik listesi görüntülendikten sonra **ifade**' i seçin. İfade düzenleyicisinde, ifadeyi buraya girin ve **Tamam**' ı seçin:

   `triggerOutputs()['Headers']`

   Bu ifadeyi bu belirteç olarak çözmek için, tasarımcı ve kod görünümü arasında geçiş yapın, örneğin:

   ![Çözümlenmiş üstbilgiler çıkışı tetikleyiciden](./media/logic-apps-enterprise-integration-b2b/resolved-trigger-outputs-headers-expression.png)

## <a name="add-response-action-for-message-receipt-notification"></a>İleti alındı bildirimi için yanıt ekleme eylemi

Ticaret ortağına iletinin alındığını bildirmek için, **Yanıt** EYLEMINI kullanarak AS2 Ileti değerlendirmesi bildirimi (MDN) içeren bir yanıt döndürebilirsiniz. Bu eylemi, **AS2 kod çözme** eyleminden hemen sonra ekleyerek, bu eylem başarısız olursa, mantıksal uygulama işlemeye devam etmez.

1. **AS2 kod çözme** eylemi altında **yeni adım**' ı seçin.

1. **Eylem seçin**altında, arama kutusunda, **yerleşik**' i seçin. Arama kutusuna `condition` yazın. **Eylemler** listesinden **koşul**' ı seçin.

   !["Koşul" eylemini ekleyin](./media/logic-apps-enterprise-integration-b2b/add-condition-action.png)

   Koşul şekli, koşulun karşılanıp karşılanmayacağı gibi yollar da dahil görüntülenir.

   ![Karar yollarla koşul şekli](./media/logic-apps-enterprise-integration-b2b/added-condition-action.png)

1. Şimdi değerlendirilecek koşulu belirtin. **Değer seçin** kutusuna şu ifadeyi girin:

   `@body('AS2_Decode')?['AS2Message']?['MdnExpected']`

   Ortadaki kutuda karşılaştırma işleminin olarak `is equal to`ayarlandığından emin olun. Sağ taraftaki kutuya değeri `Expected`girin. Bu belirteç olarak çözülecek ifadeyi almak için tasarımcı ve kod görünümü arasında geçiş yapın.

   ![Karar yollarla koşul şekli](./media/logic-apps-enterprise-integration-b2b/expression-for-evaluating-condition.png)

1. Şimdi **AS2 kod çözme** eyleminin başarılı olup olmadığını döndüren yanıtları belirtin.

   1. **AS2 kod çözme** eyleminin başarılı olması durumunda, **If true** şeklinde **bir eylem Ekle**' yi seçin. **Eylem seçin**altında, arama kutusuna girin `response`ve **Yanıt**' ı seçin.

      !["Yanıt" eylemini bulup seçin](./media/logic-apps-enterprise-integration-b2b/select-http-response-action.png)

   1. **AS2 kod çözme** EYLEMININ çıktısından AS2 MDN 'sine erişmek için şu ifadeleri belirtin:

      * **Yanıt** eyleminin **üstbilgiler** özelliğinde şu ifadeyi girin:

        `@body('AS2_Decode')?['OutgoingMdn']?['OutboundHeaders']`

      * **Yanıt** eyleminin **gövde** özelliğinde şu ifadeyi girin:

        `@body('AS2_Decode')?['OutgoingMdn']?['Content']`

   1. Belirteç olarak çözülecek ifadeleri almak için tasarımcı ve kod görünümü arasında geçiş yapın:

      ![AS2 MDN erişimi için çözümlenen ifade](./media/logic-apps-enterprise-integration-b2b/response-action-success-resolved-expression.png)

   1. **AS2 kod çözme** eyleminin başarısız olduğu durum Için, **IF false** şeklinde **bir eylem Ekle**' yi seçin. **Eylem seçin**altında, arama kutusuna girin `response`ve **Yanıt**' ı seçin. İstediğiniz durum ve hatayı döndürmek için **Yanıt** eylemini ayarlayın.

1. Mantıksal uygulamanızı kaydedin.

## <a name="add-decode-x12-message-action"></a>Kod çözme x12 ileti eylemi Ekle

1. Şimdi **kod çözme x12 ileti** eylemini ekleyin. **Yanıt** eylemi altında **Eylem Ekle**' yi seçin.

1. **Eylem seçin**altında, arama kutusuna girin `x12 decode`ve **x12 ileti kodunu çöz**' ü seçin.

   !["X12 ileti kodunu çöz" eylemini bul ve Seç](./media/logic-apps-enterprise-integration-b2b/add-x12-decode-action.png)

1. X12 eylemi sizden bağlantı bilgilerini isterse, bağlantı adını belirtin, kullanmak istediğiniz tümleştirme hesabını seçin ve ardından **Oluştur**' u seçin.

   ![Tümleştirme hesabına x12 bağlantısı oluşturma](./media/logic-apps-enterprise-integration-b2b/create-x12-integration-account-connection.png)

1. Şimdi x12 eyleminin girdisini belirtin. Bu örnek, ileti içeriği olan, ancak bu içeriğin JSON nesne biçiminde olduğunu ve Base64 kodlamalı olduğunu aklınızda AS2 eylemi olan çıktıyı kullanır. Bu nedenle, bu içeriği bir dizeye dönüştürmeniz gerekir.

   **Kodu çözmek Için x12 düz dosya iletisi** kutusuna AS2 çıkışını dönüştürmek için bu ifadeyi girin:

   `@base64ToString(body('AS2_Decode')?['AS2Message']?['Content'])`

    Bu belirteç olarak çözülecek ifadeyi almak için tasarımcı ve kod görünümü arasında geçiş yapın.

    ![Base64 ile kodlanmış içeriği dizeye Dönüştür](./media/logic-apps-enterprise-integration-b2b/x12-decode-message-content.png)

1. Mantıksal uygulamanızı kaydedin.

   Bu mantıksal uygulama için ek adımlara ihtiyacınız varsa, örneğin ileti içeriğinin kodunu çözün ve bu içeriğin JSON nesne biçiminde çıkışını almak için mantıksal uygulamanızı oluşturmaya devam edin.

Artık B2B mantıksal uygulamanızı ayarlamayı tamamladınız. Gerçek bir dünya uygulamasında, kodu çözülmüş x12 verilerini iş kolu (LOB) uygulamasında veya veri deposunda depolamak isteyebilirsiniz. Örneğin, aşağıdaki makalelere bakın:

* [Azure Logic Apps’ten SAP sistemlerine bağlanma](../logic-apps/logic-apps-using-sap-connector.md)
* [SSH ve Azure Logic Apps kullanarak SFTP dosyalarını izleme, oluşturma ve yönetme](../connectors/connectors-sftp-ssh.md)

Kendi LOB uygulamalarınızı bağlamak ve mantıksal uygulamanızda bu API 'Leri kullanmak için, daha fazla eylem ekleyebilir veya [özel API 'ler yazabilirsiniz](../logic-apps/logic-apps-create-api-app.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Gelen HTTPS çağrılarını alma ve yanıtlama](../connectors/connectors-native-reqres.md)
* [B2B kurumsal tümleştirme için Exchange AS2 iletileri](../logic-apps/logic-apps-enterprise-integration-as2.md)
* [B2B kurumsal tümleştirme için Exchange x12 iletileri](../logic-apps/logic-apps-enterprise-integration-x12.md)
* [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) hakkında daha fazla bilgi edinin
