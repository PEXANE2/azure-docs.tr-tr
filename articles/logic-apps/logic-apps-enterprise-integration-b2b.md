---
title: B2B kurumsal tümleştirme senaryoları için ileti alışverişi
description: Kurumsal Tümleştirme Paketini kullanarak Azure Logic Apps'taki ticaret ortakları arasında B2B iletileri alın ve gönderin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/10/2020
ms.openlocfilehash: 01b2bd464db51e255930fe83a3f4321687322275
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77151197"
---
# <a name="receive-and-send-b2b-messages-by-using-azure-logic-apps-and-enterprise-integration-pack"></a>Azure Mantık Uygulamaları ve Kurumsal Tümleştirme Paketi'ni kullanarak B2B iletileri alın ve gönderin

Ticaret ortaklarını ve anlaşmaları tanımlayan bir entegrasyon hesabınız olduğunda, [Kurumsal Tümleştirme Paketi](../logic-apps/logic-apps-enterprise-integration-overview.md)ile [Azure Logic Apps'ı](../logic-apps/logic-apps-overview.md) kullanarak ticari ortaklar arasında ileti alışverişinde bulunarak otomatik bir işletme (B2B) iş akışı oluşturabilirsiniz. Azure Logic Apps, AS2, X12, EDIFACT ve RosettaNet endüstri standardı protokolleri destekleyen bağlayıcılarla çalışır. Ayrıca, bu bağlayıcıları Logic [Apps'ta](../connectors/apis-list.md)(örneğin Salesforce ve Office 365 Outlook) kullanılabilen diğer bağlayıcılarla birleştirebilirsiniz.

Bu makalede, bir İstek tetikleyicisi kullanarak bir HTTP isteği alan bir mantık uygulamasının nasıl oluşturulup oluşturulacak, AS2 ve X12 eylemlerini kullanarak ileti içeriğini nasıl çözer ve yanıt eylemini kullanarak yanıt verir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz aboneliğiniz yoksa, ücretsiz [bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* Aşağıdaki eylemleri izleyen [İstek](../connectors/connectors-native-reqres.md) tetikleyicisini kullanarak B2B iş akışını oluşturabilmeniz için boş bir mantık uygulaması:

  * [AS2 Şifresini Çöz](../logic-apps/logic-apps-enterprise-integration-as2.md)

  * [AS2](../logic-apps/logic-apps-control-flow-conditional-statement.md)Decode eyleminin başarılı olup olmadığına bağlı olarak [yanıt](../connectors/connectors-native-reqres.md) gönderen Koşul

  * [X12 iletisini çözme](../logic-apps/logic-apps-enterprise-integration-x12.md) 

  Mantık uygulamalarında yeniyseniz, [Azure Mantık Uygulamaları nedir?ve](../logic-apps/logic-apps-overview.md) [Quickstart'ı inceleyin: İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Azure aboneliğinizle ilişkili ve mantık uygulamanıza bağlı bir [entegrasyon hesabı.](../logic-apps/logic-apps-enterprise-integration-accounts.md) Hem mantık uygulamanız hem de tümleştirme hesabınız aynı konumda veya Azure bölgesinde bulunmalıdır.

* Bu ortaklar için [AS2 ve X12 anlaşmaları](logic-apps-enterprise-integration-agreements.md) ile birlikte entegrasyon hesabınızda tanımladığınız en az iki [ticaret ortağı.](../logic-apps/logic-apps-enterprise-integration-partners.md)

## <a name="add-request-trigger"></a>İstek tetikleyicisi ekle

Bu örnek, Azure portalında Logic App Designer'ı kullanır, ancak Visual Studio'daki Logic App Designer için benzer adımları izleyebilirsiniz.

1. Azure [portalında,](https://portal.azure.com)Boş mantık uygulamanızı Logic App Designer'da açın.

1. Arama kutusuna girin `when a http request`ve tetikleyici olarak kullanmak üzere **bir HTTP isteği nin ne zaman alındığını** seçin.

   ![Mantık uygulaması iş akışınızı başlatmak için İstek tetikleyicisini seçin](./media/logic-apps-enterprise-integration-b2b/select-http-request-trigger.png)

1. X12 iletisi düz bir dosya olduğundan **İstek gövdesi JSON Schema** kutusunu boş bırakın.

   ![Bırak "İstek vücut JSON Schema" boş](./media/logic-apps-enterprise-integration-b2b/receive-trigger-message-body-json-schema.png)

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

   Bu adım, mantık uygulamasını tetikleyen isteği göndermek için kullanılacak **HTTP POST URL'sini** oluşturur. Bu URL'yi kopyalamak için URL'nin yanındaki kopya simgesini seçin.

   ![Çağrıları almak için İstek tetikleyicisi için oluşturulan URL](./media/logic-apps-enterprise-integration-b2b/generated-url-request-trigger.png)

## <a name="add-as2-decode-action"></a>AS2 deşifre eylemi ekleme

Şimdi kullanmak istediğiniz B2B eylemlerini ekleyin. Bu örnek, AS2 ve X12 eylemlerini kullanır.

1. Tetikleyicinin altında **Yeni adım'ı**seçin. Tetikleyici ayrıntılarını gizlemek için tetikleyicinin başlık çubuğuna tıklayın.

   ![Mantık uygulaması iş akışınıza bir adım daha ekleme](./media/logic-apps-enterprise-integration-b2b/add-new-action-under-trigger.png)

1. Altında arama kutusuna bir eylem `as2 decode` **seçin,** girin ve **AS2 Decode (v2)** seçin.

   !["AS2 Decode (v2)" seçeneğini bulun ve seçin.](./media/logic-apps-enterprise-integration-b2b/add-as2-decode-action.png)

1. İleti özelliğinin **şifresini çözmek için,** AS2 eyleminin çözmesini istediğiniz girişi `body` girin, bu da HTTP isteği tetikleyicisi tarafından alınan içeriktir. Dinamik içerik listesinden veya bir ifade olarak bu içeriği giriş olarak belirtmenin birden çok yolu vardır:

   * Kullanılabilir tetikleyici çıktılarını gösteren bir listeden seçim yapmak için, kutuyu **çözmek için İleti'nin** içini tıklatın. Dinamik içerik listesi görüntülenince, **http isteği aldığında,** örneğin **Gövde** özellik değerini seçin:

     ![Tetikleyiciden "Gövde" değerini seçin](./media/logic-apps-enterprise-integration-b2b/select-body-content-from-trigger.png)

   * Tetikleyicinin `body` çıktısına başvuran bir ifade girmek için, kutuyu **çözmek için İleti'nin** içine tıklayın. Dinamik içerik listesi göründükten sonra **İfade'yi**seçin. İfade düzenleyicisinde, ifadeyi buraya girin ve **Tamam'ı**seçin:

     `triggerOutputs()['body']`

     Veya, İleti kutusunu **çözmek için,** doğrudan bu ifadeyi girin:

     `@triggerBody()`

     İfade **Vücut** belirteci giderir.

     ![Tetikten çözülmüş gövde çıkışı](./media/logic-apps-enterprise-integration-b2b/resolved-trigger-outputs-body-expression.png)

1. İleti **üstbilgileri** özelliği için, HTTP istek tetikleyicisi tarafından alınan `headers` içerik tarafından açıklanan AS2 eylemi için gereken üstbilgi girin.

   Tetikleyicinin `headers` çıktısına atıfta bulunulan bir ifade girmek için **İleti üstbilgileri** kutusunun içine tıklayın. Dinamik içerik listesi göründükten sonra **İfade'yi**seçin. İfade düzenleyicisinde, ifadeyi buraya girin ve **Tamam'ı**seçin:

   `triggerOutputs()['Headers']`

   Bu ifadenin bu belirteç olarak çözülmesini sağlamak için, tasarımcı ve kod görünümü arasında geçiş yapabilirsiniz:

   ![Tetikten çözülmüş üstbilgi çıkışı](./media/logic-apps-enterprise-integration-b2b/resolved-trigger-outputs-headers-expression.png)

## <a name="add-response-action-for-message-receipt-notification"></a>İleti makbuzu bildirimi için Yanıt eylemi ekleme

İşlem ortağına iletinin alındığını bildirmek **için, Yanıt** eylemini kullanarak AS2 İleti Verme Bildirimi (MDN) içeren bir yanıt döndürebilirsiniz. **AS2 Decode** eyleminden hemen sonra bu eylemi ekleyerek, bu eylem başarısız olursa, mantık uygulaması işlemeye devam etmez.

1. **AS2 Kod Çözme** eyleminin altında **Yeni adım'ı**seçin.

1. Arama kutusunun altında **bir eylem seçin,** **Dahili'yi**seçin. Arama kutusuna `condition` yazın. **Eylemler** listesinden **Koşul'u**seçin.

   !["Koşul" eylemini ekleme](./media/logic-apps-enterprise-integration-b2b/add-condition-action.png)

   Şimdi koşul şekli, koşulun karşılanıp karşılanmadığına yönelik yollar da dahil olmak üzere görüntülenir.

   ![Karar yolları ile koşul şekli](./media/logic-apps-enterprise-integration-b2b/added-condition-action.png)

1. Şimdi değerlendirmek için koşul belirtin. Değer **seç kutusuna** şu ifadeyi girin:

   `@body('AS2_Decode')?['AS2Message']?['MdnExpected']`

   Orta kutuda, karşılaştırma işleminin '' `is equal to`olarak ayarlandıklarına emin olun Sağ taraftaki kutuya değeri `Expected`girin. İfadenin bu belirteç olarak çözülmesini sağlamak için, tasarımcı ve kod görünümü arasında geçiş yapabilirsiniz.

   ![Karar yolları ile koşul şekli](./media/logic-apps-enterprise-integration-b2b/expression-for-evaluating-condition.png)

1. Şimdi, **AS2 Kod Çözme** eyleminin başarılı olup olmadığını döndürmek için yanıtları belirtin.

   1. **AS2 Kod Çözme** eyleminin başarılı olduğu durum için, **If true** shape'de **eylem ekle'yi**seçin. Altında arama **kutusuna bir eylem seçin,** girin `response`ve **Yanıt'ı**seçin.

      !["Yanıt" eylemini bulma ve seçme](./media/logic-apps-enterprise-integration-b2b/select-http-response-action.png)

   1. **AS2 Decode** eyleminin çıktısından AS2 MDN'ye erişmek için şu ifadeleri belirtin:

      * **Yanıt** eyleminin **Üstbilgileri** özelliğine şu ifadeyi girin:

        `@body('AS2_Decode')?['OutgoingMdn']?['OutboundHeaders']`

      * **Yanıt** eyleminin **Gövde** özelliğine şu ifadeyi girin:

        `@body('AS2_Decode')?['OutgoingMdn']?['Content']`

   1. İfadelerin belirteç olarak çözülmesini sağlamak için, tasarımcı ve kod görünümü arasında geçiş yapabilirsiniz:

      ![AS2 MDN'ye erişmek için çözümlenmiş ifade](./media/logic-apps-enterprise-integration-b2b/response-action-success-resolved-expression.png)

   1. **AS2 Kod Çözme** eyleminin başarısız olduğu durum **için, If false** şeklinde eylem **ekle'yi**seçin. Altında arama **kutusuna bir eylem seçin,** girin `response`ve **Yanıt'ı**seçin. İstediğiniz durumu ve hatayı döndürmek için **Yanıt** eylemini ayarlayın.

1. Mantıksal uygulamanızı kaydedin.

## <a name="add-decode-x12-message-action"></a>Decode X12 ileti eylemi ekle

1. Şimdi **Decode X12 ileti** eylemini ekleyin. **Yanıt** eyleminin altında, **eylem ekle'yi**seçin.

1. Altında arama kutusuna bir eylem `x12 decode` **seçin,** girin ve **Decode X12 iletisini**seçin.

   !["X12 iletisini çöz" eylemini bulun ve seçin](./media/logic-apps-enterprise-integration-b2b/add-x12-decode-action.png)

1. X12 eylemi bağlantı bilgilerinizi isterse, bağlantının adını sağlayın, kullanmak istediğiniz entegrasyon hesabını seçin ve sonra **Oluştur'u**seçin.

   ![Tümleştirme hesabına X12 bağlantısı oluşturma](./media/logic-apps-enterprise-integration-b2b/create-x12-integration-account-connection.png)

1. Şimdi X12 eylemi için giriş belirtin. Bu örnek, ileti içeriği olan AS2 eyleminin çıktısını kullanır, ancak bu içeriğin JSON nesne biçiminde olduğunu ve base64 kodlu olduğunu unutmayın. Yani, bu içeriği bir dize dönüştürmek zorunda.

   Kutuyu **çözmek için X12 Flat dosya iletisinde,** AS2 çıktısını dönüştürmek için bu ifadeyi girin:

   `@base64ToString(body('AS2_Decode')?['AS2Message']?['Content'])`

    İfadenin bu belirteç olarak çözülmesini sağlamak için, tasarımcı ve kod görünümü arasında geçiş yapabilirsiniz.

    ![Base64 kodlanmış içeriği bir dize dönüştürme](./media/logic-apps-enterprise-integration-b2b/x12-decode-message-content.png)

1. Mantıksal uygulamanızı kaydedin.

   Bu mantık uygulaması için ek adımlara ihtiyacınız varsa, örneğin, json nesne biçimindeki ileti içeriğini ve çıktısını çözmek için, mantık uygulamanızı oluşturmaya devam edin.

Artık B2B mantık uygulamanızı kurmayı bitirdiniz. Gerçek bir uygulamada, kodlanmış X12 verilerini bir iş satırı (LOB) uygulamasında veya veri deposunda depolamak isteyebilirsiniz. Örneğin, şu makalelere bakın:

* [Azure Logic Apps’ten SAP sistemlerine bağlanma](../logic-apps/logic-apps-using-sap-connector.md)
* [SSH ve Azure Mantık Uygulamalarını kullanarak SFTP dosyalarını izleme, oluşturma ve yönetme](../connectors/connectors-sftp-ssh.md)

Kendi LOB uygulamalarınızı bağlamak ve bu API'leri mantık uygulamanızda kullanmak için daha fazla eylem ekleyebilir veya [özel API'ler yazabilirsiniz.](../logic-apps/logic-apps-create-api-app.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Gelen HTTPS çağrılarını alma ve yanıtla](../connectors/connectors-native-reqres.md)
* [B2B kurumsal tümleştirme için AS2 iletilerini değiştirme](../logic-apps/logic-apps-enterprise-integration-as2.md)
* [B2B kurumsal tümleştirmesi için X12 iletilerini değiştirme](../logic-apps/logic-apps-enterprise-integration-x12.md)
* Kurumsal Entegrasyon [Paketi](../logic-apps/logic-apps-enterprise-integration-overview.md) hakkında daha fazla bilgi edinin