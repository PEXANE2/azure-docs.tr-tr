---
title: Azure Service Bus Azure Logic Apps ile ileti gönderme ve alma | Microsoft Docs
description: Azure Logic Apps Azure Service Bus ile Kurumsal bulut mesajlaşma ayarlama
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 944bac44c1fc6504dfe1a93df5760ccf4ee46fa0
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69982204"
---
# <a name="exchange-messages-in-the-cloud-with-azure-service-bus-and-azure-logic-apps"></a>Azure Service Bus ve Azure Logic Apps ile bulutta ileti alışverişi yapın

Azure Logic Apps ve Azure Service Bus bağlayıcısı sayesinde, kuruluşunuz için uygulamalar arasında satış ve satın alma siparişleri, günlükler ve stok hareketleri gibi verileri aktaran otomatik görevler ve iş akışları oluşturabilirsiniz. Bağlayıcı yalnızca iletileri izleyip yönetmez, gönderir ve yönetir, ancak aynı zamanda kuyruklar, oturumlar, konular, abonelikler vb. ile eylemler gerçekleştirir, örneğin:

* Kuyruklar, konular ve konu aboneliklerinde iletiler ne zaman ulaştığında (otomatik olarak tamamlanır) veya alındığında (Peek kilidi). 
* İleti gönderin.
* Konu abonelikleri oluşturun ve silin.
* Kuyruklar ve konu aboneliklerinde iletileri yönetme (örneğin, Get, Gecikmeli, tam, erteleme, bırakma ve atılacak harf).
* Kuyruklar ve konu aboneliklerindeki ileti ve oturumlardaki kilitleri yenileyin.
* Kuyruklar ve konular 'daki oturumları kapatın.

Service Bus yanıt alan Tetikleyicileri kullanabilir ve çıktıyı mantıksal uygulamalarınızda diğer eylemler için kullanılabilir hale getirebilirsiniz. Ayrıca, Service Bus eylemlerdeki çıktıyı kullanan başka eylemlere de sahip olabilirsiniz. Service Bus ve Logic Apps yeni başladıysanız [Azure Service Bus ne olduğunu](../service-bus-messaging/service-bus-messaging-overview.md) gözden geçirin. [ne Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Kuyruk gibi bir Service Bus ad alanı ve mesajlaşma varlığı. Bu öğeleriniz yoksa, [Service Bus ad alanınızı ve bir kuyruğu oluşturmayı](../service-bus-messaging/service-bus-create-namespace-portal.md)öğrenin. 

  Bu öğeler, bu öğeleri kullanan mantıksal uygulamalarınızla aynı Azure aboneliğinde bulunmalıdır.

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Service Bus kullanmak istediğiniz mantıksal uygulama. Mantıksal uygulamanız, Service Bus ile aynı Azure aboneliğinde bulunmalıdır. Service Bus tetikleyicisinden başlamak için [boş bir mantıksal uygulama oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md). Service Bus eylemi kullanmak için, mantıksal uygulamanızı başka bir tetikleyici ile başlatın, örneğin **yineleme** tetikleyicisi.

<a name="permissions-connection-string"></a>

## <a name="check-permissions"></a>İzinleri denetle

Mantıksal uygulamanızın Service Bus ad alanına erişim izinleri olduğunu doğrulayın. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 

2. Service Bus *ad alanına*gidin. Ad alanı sayfasında, **Ayarlar**' ın altında, **paylaşılan erişim ilkeleri**' ni seçin. **Talepler**' in altında, bu ad alanı Için izinleri **Yönet** ' in olup olmadığını denetleyin

   ![Service Bus ad alanınız için izinleri yönetme](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

3. Service Bus ad alanınız için bağlantı dizesini alın. Mantıksal uygulamanıza bağlantı bilgilerinizi girerken bu dizeye ihtiyacınız vardır.

   1. Seçin **RootManageSharedAccessKey**. 
   
   1. Birincil bağlantı dizeniz yanındaki Kopyala düğmesini seçin. Bağlantı dizesini daha sonra kullanmak üzere kaydedin.

      ![Service Bus ad alanı bağlantı dizesini Kopyala](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Bağlantı dizeniz Service Bus ad alanınız veya bir kuyruk gibi bir mesajlaşma varlığı ile ilişkili olup olmadığını doğrulamak için, `EntityPath`  parametre için bağlantı dizesini arayın. Bu parametreyi bulursanız, bağlantı dizesi belirli bir varlık içindir ve mantıksal uygulamanızla birlikte kullanılacak doğru dize değildir.

## <a name="add-trigger-or-action"></a>Tetikleyici veya eylem ekleme

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure Portal](https://portal.azure.com)oturum açın ve daha önce açık değilse mantıksal uygulama Tasarımcısı 'nda mantıksal uygulamanızı açın.

1. Boş bir mantıksal uygulamaya *tetikleyici* eklemek için, arama kutusuna filtreniz olarak "Azure Service Bus" yazın. Tetikleyiciler listesinde istediğiniz tetikleyiciyi seçin. 

   Örneğin, mantıksal uygulamanızı yeni bir öğe Service Bus kuyruğuna gönderildiğinde tetiklemeniz için, bu tetikleyiciyi seçin: **Kuyrukta bir ileti alındığında (otomatik olarak tamamlanır)**

   ![Service Bus tetikleyicisi seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   > [!NOTE]
   > Bazı Tetikleyiciler, bir veya **daha fazla ileti bir sıraya ulaştığında (otomatik tamamlanır) bir veya daha fazla**ileti döndürebilir. Bu Tetikleyiciler tetiklendiğinde, tetikleyicinin **en yüksek ileti sayısı** özelliği tarafından belirtilen ileti sayısı arasında bir değer döndürür.

   *Tüm Service Bus Tetikleyicileri uzun yoklama tetiklerdir*, bu da tetikleyici tetiklendiğinde tetikleyici tüm iletileri işler ve sonra sıra veya konu aboneliğinde daha fazla ileti görünmesi için 30 saniye bekler. 
   30 saniye içinde herhangi bir ileti görünmezse, tetikleyici çalıştırması atlanır. 
   Aksi takdirde tetikleyici, kuyruk veya konu aboneliği boş olana kadar iletileri okumaya devam eder. Sonraki tetikleyici yoklama, tetikleyicisinin özelliklerinde belirtilen yinelenme aralığına göre belirlenir.

1. Mevcut bir mantıksal uygulamaya *eylem* eklemek için aşağıdaki adımları izleyin: 

   1. Eylem eklemek istediğiniz son adım altında **yeni adım**' ı seçin. 

      Adımlar arasında bir eylem eklemek için, işaretçinizi adımlar arasındaki oka taşıyın. 
      Görüntülenen artı işaretini ( **+** ) seçin ve ardından **Eylem Ekle**' yi seçin.

   1. Arama kutusuna filtreniz olarak "Azure Service Bus" yazın. 
   Eylemler listesi altında istediğiniz eylemi seçin. 
 
      Örneğin, şu eylemi seçin: **İleti gönder**

      ![Service Bus eylemi seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png) 

1. Mantıksal uygulamanızı ilk kez Service Bus ad alanına bağlıyorsanız, Logic App Designer artık sizden bağlantı bilgilerinizi ister. 

   1. Bağlantınız için bir ad girin ve Service Bus ad alanınızı seçin.

      ![Service Bus bağlantı oluştur, Bölüm 1](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-1.png)

      Bunun yerine bağlantı dizesini el ile girmek için **bağlantı bilgilerini el ile girin**' i seçin. 
      Bağlantı dizeniz yoksa, [Bağlantı dizenizi bulmayı](#permissions-connection-string)öğrenin.

   1. Şimdi Service Bus ilkenizi seçin ve ardından **Oluştur**' u seçin.

      ![Service Bus bağlantı oluştur, Bölüm 2](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-2.png)

1. Bu örnekte, bir kuyruk veya konu gibi istediğiniz mesajlaşma varlığını seçin. Bu örnekte Service Bus kuyruğunuzu seçin. 
   
   ![Service Bus kuyruğu seçin](./media/connectors-create-api-azure-service-bus/service-bus-select-queue.png)

1. Tetikleyiciniz veya eyleminiz için gerekli ayrıntıları sağlayın. Bu örnekte, Tetikleyiciniz veya eyleminiz için ilgili adımları izleyin: 

   * **Örnek tetikleyici için**: Kuyruğu denetlemek için yoklama aralığını ve sıklığını ayarlayın.

     ![Yoklama aralığını ayarla](./media/connectors-create-api-azure-service-bus/service-bus-trigger-details.png)

     İşiniz bittiğinde, istediğiniz eylemleri ekleyerek mantıksal uygulamanızın iş akışını oluşturmaya devam edin. Örneğin, yeni bir ileti geldiğinde e-posta gönderen bir eylem ekleyebilirsiniz.
     Tetikleyiciniz kuyruğunuzu denetlediğinde ve yeni bir ileti bulduğunda, mantıksal uygulamanız bulunan ileti için seçtiğiniz eylemleri çalıştırır.

   * **Örnek eylem için**: İleti içeriğini ve diğer ayrıntıları girin. 

     ![İleti içeriğini ve ayrıntılarını sağlayın](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)

     İşiniz bittiğinde, istediğiniz diğer eylemleri ekleyerek mantıksal uygulamanızın iş akışını oluşturmaya devam edin. Örneğin, iletinizin gönderildiğini onaylayan e-posta gönderen bir eylem ekleyebilirsiniz.

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Service Bus Bağlayıcısı, bir hizmet veri yolundan bağlayıcı önbelleğine en çok 1.500 benzersiz oturum kaydedebilir. Oturum sayısı bu sınırı aşarsa, eski oturumlar önbellekten kaldırılır. Bağlayıcının Openapı (eski adıyla Swagger) açıklaması tarafından tanımlanan Tetikleyiciler, Eylemler ve limitlerle ilgili diğer teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/servicebus/)gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
