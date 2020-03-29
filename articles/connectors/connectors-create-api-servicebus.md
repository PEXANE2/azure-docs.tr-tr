---
title: Azure Hizmet Veri Servisi ile ileti alışverişinde bulunma
description: Azure Mantıksal Apps'ta Azure Hizmet Veri Servisi'ni kullanarak ileti gönderip alan otomatik görevler ve iş akışları oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 09/19/2019
tags: connectors
ms.openlocfilehash: 1b38b8508dbe17d42bf191149410f5db638cf834
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76261628"
---
# <a name="exchange-messages-in-the-cloud-by-using-azure-logic-apps-and-azure-service-bus"></a>Azure Logic Apps ve Azure Hizmet Veri Servisi'ni kullanarak bulutta ileti alışverişi yapın

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve Azure Hizmet Veri Hizmeti veri tonlarında, kuruluşunuz için uygulamalar arasında satış ve satınalma siparişleri, dergiler ve stok hareketleri gibi veri aktarımı yapan otomatik görevler ve iş akışları oluşturabilirsiniz. [Azure Service Bus](../service-bus-messaging/service-bus-messaging-overview.md) Bağlayıcı yalnızca iletileri izlemekle kalmayıp, aynı zamanda kuyruklar, oturumlar, konular, abonelikler ve benzeri eylemler gerçekleştirir:

* İletiler geldiğinde (otomatik olarak tamamlandı) veya kuyruklarda, konularda ve konu aboneliklerinde alınan (peek-lock) izleyin.
* Mesaj gönderin.
* Konu abonelikleri oluşturun ve silin.
* Örneğin, sıralar ve konu abonelikleri ndeki iletileri yönetin, ertelenmiş olsun, tamamlayın, erteler, terk edin ve ölü harf alın.
* Kuyruklarda ve konu aboneliklerinde iletiler ve oturumlar üzerindeki kilitleri yenileyin.
* Sıralar ve konulardaki oturumları kapatın.

Hizmet Veri Servisi'nden yanıt alan tetikleyiciler kullanabilir ve çıktıyı mantık uygulamalarınızdaki diğer eylemleriçin kullanılabilir hale getirebilirsiniz. Diğer eylemlerin Hizmet Veri Servisi eylemlerinden çıktı kullanmasını da sağlayabilirsiniz. Servis Veri Ve Mantık Uygulamalarında yeniyseniz, [Azure Hizmet Veri Servisi Veri Servisi nedir](../service-bus-messaging/service-bus-messaging-overview.md) ve Azure Mantık Uygulamaları [nedir'i](../logic-apps/logic-apps-overview.md)inceleyin?

[!INCLUDE [Warning about creating infinite loops](../../includes/connectors-infinite-loops.md)]

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Sıra gibi bir Hizmet Veri Günü ad alanı ve ileti varlığı. Bu öğelerin ve mantık uygulamanızın aynı Azure aboneliğini kullanması gerekir. Bu öğelere sahip değilseniz, [Hizmet Veri Yolu ad alanınızı ve sıranızı nasıl oluşturabileceğinizi](../service-bus-messaging/service-bus-create-namespace-portal.md)öğrenin.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Hizmet Veri Aracı ad alanını ve mesajlaşma varlığını kullandığınız mantık uygulaması. Mantık uygulamanızın ve servis veri tonunuzun aynı Azure aboneliğini kullanması gerekir. İş akışınızı Bir Hizmet Veri Servisi tetikleyicisi ile başlatmak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) İş akışınızda bir Hizmet Veri Servisi eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, [örneğin, Yineleme](../connectors/connectors-native-recurrence.md)tetikleyicisi.

<a name="permissions-connection-string"></a>

## <a name="check-permissions"></a>İzinleri denetle

Mantık uygulamanızın Hizmet Veri Günü ad alanına erişmek için izinleri olduğunu doğrulayın.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Servis Veri Günü *ad alanınıza*gidin. Ad alanı sayfasında, **Ayarlar**altında **Paylaşılan erişim ilkelerini**seçin. **Talepler**altında, bu ad alanı için **Yönet** izinlerine sahip olup olmadığınızı denetleyin.

   ![Service Bus ad alanı için izinleri yönetme](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

1. Service Bus ad alanınız için bağlantı dizesini alın. Mantık uygulamanızdaki bağlantı bilgilerini sağlarken bu dizeye ihtiyacınız var.

   1. Paylaşılan **erişim ilkeleri** bölmesine **RootManageSharedAccessKey'i**seçin.
   
   1. Birincil bağlantı dizenizin yanında kopyala düğmesini seçin. Bağlantı dizesini daha sonra kullanmak üzere kaydedin.

      ![Hizmet Veri Çiş ad alanı bağlantı dizesi kopyalama](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Bağlantı dizenizin Hizmet Veri Ekibi ad alanınız veya kuyruk gibi bir ileti varlığıyla ilişkili `EntityPath`  olup olmadığını doğrulamak için, parametre için bağlantı dizesini arayın. Bu parametreyi bulursanız, bağlantı dizesi belirli bir varlık içindir ve mantık uygulamanızla kullanılacak doğru dize değildir.

## <a name="add-service-bus-trigger"></a>Servis Veri Servisi tetikleyicisi ekle

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve Logic App Designer'da boş mantık uygulamanızı açın.

1. Arama kutusuna filtreniz olarak "azure servis veri nesi"ni girin. Tetikleyiciler listesinden, istediğiniz tetikleyiciyi seçin.

   Örneğin, yeni bir öğe Hizmet Veri Servisi kuyruğuna gönderildiğinde mantık uygulamanızı tetiklemek **için, bir ileti sırada ne zaman (otomatik tamamlama) tetikleyicisi alınır'ı** seçin.

   ![Servis Veri Servisi tetikleyicisi seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   Tüm Servis Otobüsü tetikleyicileri *uzun yoklama* tetikleyicileridir. Bu açıklama, tetikleyici çaldığında, tetikleyicinin tüm iletileri işlediği ve daha fazla iletinin kuyrukta veya konu aboneliğinde görünmesi için 30 saniye beklediği anlamına gelir. 30 saniye içinde ileti görünmüyorsa, tetikleyici çalıştırma atlanır. Aksi takdirde, tetikleyici sıra veya konu aboneliği boşalana kadar iletileri okumaya devam eder. Bir sonraki tetikleyici yoklaması, tetikleyicinin özelliklerinde belirtilen yineleme aralığını temel alar.

   **Bir veya daha fazla ileti bir kuyruk (otomatik tamamlama) tetikleyicisi geldiğinde,** bir veya daha fazla ileti döndürebilir gibi bazı tetikleyiciler. Bunlar yangını tetiklediğinde, tetikleyicinin **Maksimum ileti sayısı** özelliği tarafından belirtilen ileti sayısı ile bir arasında döndürüler.

1. Tetikleyiciniz Hizmet Veri Kurumu ad alanınıza ilk kez bağlanıyorsa, Logic App Designer bağlantı bilgilerinizi istediğinizde aşağıdaki adımları izleyin.

   1. Bağlantınız için bir ad sağlayın ve Servis Veri Aracı ad alanınızı seçin.

      ![Servis Veri Servisi bağlantısı oluşturma, bölüm 1](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-trigger-1.png)

      Bunun yerine bağlantı dizesini el ile girmek için **bağlantı bilgilerini El Ile girin'i**seçin. Bağlantı dizeniz yoksa, bağlantı [dizenizi nasıl bulacağınızı](#permissions-connection-string)öğrenin.

   1. Servis Veri Servisi ilkenizi seçin ve **Oluştur'u**seçin.

      ![Servis Veri Servisi bağlantısı oluşturma, bölüm 2](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-trigger-2.png)

   1. Sıra veya konu gibi istediğiniz ileti tüzel kişiliğini seçin. Bu örnekiçin, Servis Veri Servisi sıranızı seçin.
   
      ![Servis Veri Servisi kuyruğuseçin](./media/connectors-create-api-azure-service-bus/service-bus-select-queue-trigger.png)

1. Seçtiğiniz tetikleyici için gerekli bilgileri sağlayın. Eyleme diğer kullanılabilir özellikleri eklemek için **yeni parametre** listesi ekleyin ve istediğiniz özellikleri seçin.

   Bu örneğin tetikleyicisi için, sırayı denetlemek için yoklama aralığını ve sıklığını seçin.

   ![Yoklama aralığını ayarlama](./media/connectors-create-api-azure-service-bus/service-bus-trigger-details.png)

   Kullanılabilir tetikleyiciler ve özellikler hakkında daha fazla bilgi için bağlayıcının [başvuru sayfasına](/connectors/servicebus/)bakın.

1. İstediğiniz eylemleri ekleyerek mantık uygulamanızı oluşturmaya devam edin.

   Örneğin, yeni bir ileti geldiğinde e-posta gönderen bir eylem ekleyebilirsiniz. Tetikleyiciniz kuyruğunuzu denetleyip yeni bir ileti bulduğunda, mantık uygulamanız bulunan ileti için seçtiğiniz eylemleri çalıştırZ.

## <a name="add-service-bus-action"></a>Servis Veri Servisi eylemi ekleme

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı açın.

1. Eylem eklemek istediğiniz adımın altında **Yeni adımı**seçin.

   Veya adımlar arasında bir eylem eklemek için işaretçinizin bu adımlar arasında okun üzerine taşıyın. Görünen artı işaretini (**+**) seçin ve eylem **ekle'yi**seçin.

1. Arama **kutusuna bir eylem seçin**altında, filtreniz olarak "azure servis veri nesi"ni girin. Eylemler listesinden, istediğiniz eylemi seçin. 

   Bu örnekiçin, **ileti gönder** eylemini seçin.

   ![Servis Veri Aracı eylemini seçin](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png) 

1. Eyleminiz Hizmet Veri Kurumu ad alanınıza ilk kez bağlanıyorsa, Logic App Designer bağlantı bilgilerinizi istediğinizde aşağıdaki adımları izleyin.

   1. Bağlantınız için bir ad sağlayın ve Servis Veri Aracı ad alanınızı seçin.

      ![Servis Veri Servisi bağlantısı oluşturma, bölüm 1](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-action-1.png)

      Bunun yerine bağlantı dizesini el ile girmek için **bağlantı bilgilerini El Ile girin'i**seçin. Bağlantı dizeniz yoksa, bağlantı [dizenizi nasıl bulacağınızı](#permissions-connection-string)öğrenin.

   1. Servis Veri Servisi ilkenizi seçin ve **Oluştur'u**seçin.

      ![Servis Veri Servisi bağlantısı oluşturma, bölüm 2](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-action-2.png)

   1. Sıra veya konu gibi istediğiniz ileti tüzel kişiliğini seçin. Bu örnekiçin, Servis Veri Servisi sıranızı seçin.

      ![Servis Veri Servisi kuyruğuseçin](./media/connectors-create-api-azure-service-bus/service-bus-select-queue-action.png)

1. Seçtiğiniz eylem için gerekli ayrıntıları sağlayın. Eyleme diğer kullanılabilir özellikleri eklemek için **yeni parametre** listesi ekleyin ve istediğiniz özellikleri seçin.

   Örneğin, **eyleme** eklemek için İçerik ve **İçerik Türü** özelliklerini seçin. Ardından, göndermek istediğiniz iletinin içeriğini belirtin.

   ![İleti içeriği ve ayrıntıları sağlama](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)

   Kullanılabilir eylemler ve özellikleri hakkında daha fazla bilgi için bağlayıcının [başvuru sayfasına](/connectors/servicebus/)bakın.

1. İstediğiniz diğer eylemleri ekleyerek mantık uygulamanızı oluşturmaya devam edin.

   Örneğin, iletinizin gönderildiğini onaylamak için e-posta gönderen bir eylem ekleyebilirsiniz.

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Servis Veri Servisi konektörü, servis veri yollarından konektör önbelleğine kadar aynı anda 1.500'e kadar benzersiz oturumdan tasarruf edebilir. Oturum sayısı bu sınırı aşarsa, eski oturumlar önbellekten kaldırılır. Daha fazla bilgi için [İleti oturumlarına](../service-bus-messaging/message-sessions.md)bakın.

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlamalar hakkındaki diğer teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/servicebus/)gözden geçirin. Azure Servis Veri Servisi Mesajlaşması hakkında daha fazla şey için Azure [Servis Veri Servisi Veri Servisi nedir?](../service-bus-messaging/service-bus-messaging-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
