---
title: B2B kurumsal tümleştirmeleri oluşturma
description: Enterprise Integration Pack ile Azure Logic Apps B2B verileri alma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 07/08/2016
ms.openlocfilehash: 39966b8171296a8608b9436485f7682d114c8410
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74793106"
---
# <a name="receive-b2b-data-with-azure-logic-apps-and-enterprise-integration-pack"></a>Azure Logic Apps ve Enterprise Integration Pack B2B verileri alma

İş ortakları ve anlaşmaları olan bir tümleştirme hesabı oluşturduktan sonra, [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md)mantıksal uygulamanız için bir iş kolu (B2B) iş akışı oluşturmaya hazırlanın.

## <a name="prerequisites"></a>Önkoşullar

AS2 ve x12 eylemlerini kullanmak için bir Kurumsal Tümleştirme hesabınızın olması gerekir. [Kurumsal tümleştirme hesabı oluşturmayı](../logic-apps/logic-apps-enterprise-integration-accounts.md)öğrenin.

## <a name="create-a-logic-app-with-b2b-connectors"></a>B2B bağlayıcılarıyla mantıksal uygulama oluşturma

Bir ticaret ortağından veri almak için AS2 ve x12 eylemlerini kullanan bir B2B Logic uygulaması oluşturmak için aşağıdaki adımları izleyin:

1. Bir mantıksal uygulama oluşturun, ardından [uygulamanızı tümleştirme hesabınıza bağlayın](../logic-apps/logic-apps-enterprise-integration-accounts.md).

2. Bir Istek ekleyin-mantıksal uygulamanıza bir **http isteği alındığında** tetiklenir.

    ![](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)

3. **Kod çözme AS2** eylemini eklemek Için **Eylem Ekle**' yi seçin.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-2.png)

4. Tüm eylemleri istediğiniz bir şekilde filtrelemek için, arama kutusuna **AS2** sözcüğünü girin.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-5.png)

5. **AS2-kodunu çöz AS2 Message** eylemini seçin.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-6.png)

6. Giriş olarak kullanmak istediğiniz **gövdesini** ekleyin. 
   Bu örnekte, mantıksal uygulamayı tetikleyen HTTP isteği gövdesini seçin. Ya da **üstbilgiler** alanındaki üst bilgileri gösteren bir ifade girin:

    @triggerOutputs() [' üstbilgiler ']

7. HTTP istek üst bilgilerinde bulabileceğiniz, AS2 için gerekli **üst bilgileri** ekleyin. 
   Bu örnekte, mantıksal uygulamayı tetikleyen HTTP isteğinin üst bilgilerini seçin.

8. Şimdi kod çözme x12 ileti eylemini ekleyin. **Eylem Ekle**' yi seçin.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-9.png)

9. Tüm eylemleri istediğiniz bir şekilde filtrelemek için, arama kutusuna **x12** sözcüğünü girin.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-10.png)

10. **X12-kodunu çöz x12 Message** eylemini seçin.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-as2message.png)

11. Şimdi bu eyleme yönelik girişi belirtmeniz gerekir. 
    Bu giriş, önceki AS2 eyleminin çıktıdır.

    Gerçek ileti içeriği bir JSON nesnesi içinde ve Base64 kodlamalı olduğundan girdi olarak bir ifade belirtmeniz gerekir. 
    Giriş alanının **kodunu çözmek Için x12 düz dosya iletisine** aşağıdaki ifadeyi girin:
    
    @base64ToString(gövde (' Decode_AS2_message ')? [' AS2Message']? [' İçerik '])

    Şimdi, bir JSON nesnesindeki ticari iş ortağı ve çıkış öğelerinden alınan x12 verilerinin kodunu çözmeye yönelik adımları ekleyin. 
    Verilerin alındığı iş ortağına bildirimde bulunmak için, bir HTTP yanıt eyleminde AS2 Message disposition bildirimini (MDN) içeren bir yanıtı geri gönderebilirsiniz.

12. **Yanıt** eylemini eklemek Için **Eylem Ekle**' yi seçin.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-14.png)

13. Tüm eylemleri istediğiniz bir şekilde filtrelemek için, arama kutusuna **Yanıt** sözcüğünü girin.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-15.png)

14. **Yanıt** eylemini seçin.

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-16.png)

15. **Kod çözme x12 iletisi** eyleminin çıktısından MDN 'ye erişmek için, yanıt **gövdesi** alanını şu ifadeyle ayarlayın:

    @base64ToString(gövde (' Decode_AS2_message ')? [' OutgoingMdn ']? [' İçerik '])

    ![](./media/logic-apps-enterprise-integration-b2b/b2b-17.png)  

16. Çalışmanızı kaydedin.

    ![](./media/logic-apps-enterprise-integration-b2b/transform-5.png)  

Artık B2B mantıksal uygulamanızı ayarlamayı tamamladınız. Gerçek bir dünya uygulamasında, çözülmüş x12 verilerini iş kolu (LOB) uygulamasında veya veri deposunda depolamak isteyebilirsiniz. Kendi LOB uygulamalarınızı bağlamak ve mantıksal uygulamanızda bu API 'Leri kullanmak için, daha fazla eylem ekleyebilir veya özel API 'Ler yazabilirsiniz.

## <a name="features-and-use-cases"></a>Özellikler ve kullanım örnekleri

* AS2 ve x12 kod çözme ve kodlama eylemleri, mantıksal uygulamalarda sektör standardı protokollerini kullanarak ticari iş ortakları arasında veri alışverişi yapmanızı sağlar.
* Ticari iş ortaklarıyla veri alışverişi yapmak için, AS2 ve x12 'yi birbirleriyle veya olmadan kullanabilirsiniz.
* B2B eylemleri tümleştirme hesabınızda kolayca iş ortakları ve anlaşmalar oluşturmanıza ve bunları bir mantıksal uygulamada kullanmanıza yardımcı olur.
* Mantıksal uygulamanızı diğer eylemlerle genişletmediğiniz zaman, SalesForce gibi diğer uygulamalar ve hizmetler arasında veri gönderebilir ve aktarabilirsiniz.

## <a name="learn-more"></a>Daha fazla bilgi
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md)
