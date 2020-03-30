---
title: Azure Etkinlik Hub'larına bağlanma
description: Azure Etkinlik Hub'ları ve Azure Mantık Uygulamaları kullanarak olayları izleyen ve yöneten otomatik görevler ve iş akışları oluşturun
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 04/23/2019
tags: connectors
ms.openlocfilehash: 32fa54ef0d8eccaf8745ee37cb028d4f3c6d73eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247299"
---
# <a name="monitor-receive-and-send-events-with-azure-event-hubs-and-azure-logic-apps"></a>Azure Etkinlik Hub'ları ve Azure Mantıksal Uygulamaları ile etkinlikleri izleme, alma ve gönderme

Bu makalede, Azure Etkinlik Hub'ları bağlayıcısı olan bir mantık uygulaması içinden [Azure Etkinlik Hub'larına](../event-hubs/event-hubs-what-is-event-hubs.md) gönderilen etkinlikleri nasıl izleyip yönetebileceğiniz gösterilmektedir. Böylece, Olay Hub’ınızdan gelen olayları almaya, bu olayları denetlemeye ve göndermeye yönelik görevleri ve iş akışlarını otomatikleştiren mantıksal uygulamalar oluşturabilirsiniz. Bağlayıcıya özel teknik bilgiler için [Azure Etkinlik Hub'ları bağlayıcı referansına](https://docs.microsoft.com/connectors/eventhubs/)</a>bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Azure [Etkinlik Hub'ları ad alanı ve Etkinlik Hub'ı](../event-hubs/event-hubs-create.md)

* Etkinlik Hub'ınıza erişmek istediğiniz mantık uygulaması. Mantıksal uygulamanızı Azure Etkinlik Hub'ları tetikleyicisiyle başlatmak için boş bir [mantık uygulamasına](../logic-apps/quickstart-create-first-logic-app-workflow.md)ihtiyacınız vardır.
Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları ve](../logic-apps/logic-apps-overview.md) Quickstart nedir'yi inceleyin: İlk [mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="check-permissions-and-get-connection-string"></a>İzinleri denetleyin ve bağlantı dizesi alın

Mantık uygulamanızın Etkinlik Hub'ınıza erişebilmesini sağlamak için izinlerinizi kontrol edin ve Olay Hub'larınız için bağlantı dizesini alın.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Belirli bir Olay Hub'ı değil, Olay Hub'ları *ad alanınıza*gidin. 

1. Ad alanı menüsünde, **Ayarlar**altında **Paylaşılan erişim ilkelerini**seçin. **Talepler**altında, bu ad alanı için **Yönet** izinlerine sahip olup olmadığınızı denetleyin.

   ![Etkinlik Hub ad alanınız için izinleri yönetme](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

1. Bağlantı bilgilerinizi daha sonra el ile girmek istiyorsanız, Olay Hub'larınızın ad alanı nın bağlantı dizesini alın.

   1. **İlke**altında **RootManageSharedAccessKey'i**seçin.

   1. Birincil anahtarınızın bağlantı dizelerini bulun. Kopyalama düğmesini seçin ve bağlantı dizesini daha sonra kullanmak üzere kaydedin.

      ![Olay Hub'larını ad alanı bağlantı dizelerini kopyalama](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

      > [!TIP]
      > Bağlantı dizenizin Olay Hub'larınız ad alanınızla mı yoksa belirli bir olay hub'ıyla `EntityPath`  mı ilişkili olduğunu doğrulamak için bağlantı dizesinin parametreye sahip olmadığından emin olun. Bu parametreyi bulursanız, bağlantı dizesi belirli bir Olay Hub'ı "varlık" içindir ve mantık uygulamanızla kullanılacak doğru dize değildir.

1. Şimdi [olay hub'ları tetikleyici ekle](#add-trigger) veya olay [hub'ları eylemi ekle](#add-action)ile devam edin.

<a name="add-trigger"></a>

## <a name="add-event-hubs-trigger"></a>Olay Hub'ları tetikleyici ekle

Azure Logic Apps'ta, her mantık uygulaması, belirli bir olay olduğunda veya belirli bir koşul yerine getirildiğinde ateş eden bir [tetikleyiciyle](../logic-apps/logic-apps-overview.md#logic-app-concepts)başlamalıdır. Tetikleyici her ateşleninher yerinde, Logic Apps motoru bir mantık uygulaması örneği oluşturur ve uygulamanızın iş akışını çalıştırmaya başlar.

Bu örnek, Olay Hub'ınıza yeni olaylar gönderildiğinde bir mantık uygulaması iş akışını nasıl başlatabileceğinizi gösterir. 

1. Azure portalında veya Visual Studio'da, Logic Apps Designer'ı açan boş bir mantık uygulaması oluşturun. Bu örnekte Azure portalı kullanır.

1. Arama kutusuna filtreniz olarak "olay hub'ları" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Olaylar Event Hub'ında kullanılabilirolduğunda - Olay Hub'ları**

   ![Tetikleyiciyi seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

1. Bağlantı ayrıntıları için istenirseniz, [Olay Hub'ları bağlantınızı şimdi oluşturun.](#create-connection) 

1. Tetikleyicide, izlemek istediğiniz Olay Hub'ı hakkında bilgi sağlayın. Daha fazla özellik için **yeni parametre** ekle listesini açın. Bir parametre seçmek, bu özelliği tetik kartına ekler.

   ![Tetikleyici özellikleri](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Olay Hub'ı adı** | Evet | İzlemek istediğiniz Olay Hub'ı adı |
   | **İçerik türü** | Hayır | Etkinliğin içerik türü. Varsayılan değer: `application/octet-stream`. |
   | **Tüketici grubu adı** | Hayır | Olayları okumak için kullanılacak [Event Hub tüketici grubunun adı.](../event-hubs/event-hubs-features.md#consumer-groups) Belirtilmemişse, varsayılan tüketici grubu kullanılır. |
   | **Maksimum olay sayısı** | Hayır | En fazla olay sayısı. Tetikleyici, bir ile bu özellik tarafından belirtilen olay sayısı arasında döndürür. |
   | **Interval** | Evet | İş akışının sıklığına bağlı olarak ne sıklıkta çalıştığını açıklayan pozitif bir tamsayı |
   | **Frequency** | Evet | Nüks için zaman birimi |
   ||||

   **Ek özellikler**

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **İçerik şeması** | Hayır | Etkinlik Hub'ından okunacak etkinlikler için JSON içeriği şeması. Örneğin, içerik şemasını belirtirseniz, yalnızca şema ile eşleşen olaylar için mantık uygulamasını tetikleyebilirsiniz. |
   | **Minimum bölme anahtarı** | Hayır | Okumak için minimum [bölüm](../event-hubs/event-hubs-features.md#partitions) kimliğini girin. Varsayılan olarak, tüm bölümler okunur. |
   | **Maksimum bölme tuşu** | Hayır | Okumak için maksimum [bölüm](../event-hubs/event-hubs-features.md#partitions) kimliğini girin. Varsayılan olarak, tüm bölümler okunur. |
   | **Saat dilimi** | Hayır | Bu tetikleyici UTC ofset kabul etmediğinden, yalnızca bir başlangıç saati belirttiğiniz zaman geçerlidir. Uygulamak istediğiniz saat dilimini seçin. <p>Daha fazla bilgi için bkz. Azure Mantık Uygulamaları ile yinelenen görevler ve iş akışları oluşturun ve [çalıştırın.](../connectors/connectors-native-recurrence.md) |
   | **Başlangıç saati** | Hayır | Bu biçimde bir başlangıç zamanı sağlayın: <p>Saat dilimi seçerseniz YYYY-MM-DDThh:mm:ss<p>-veya-<p>Saat dilimi seçmezseniz YYYY-MM-DDThh:mm:ssZ<p>Daha fazla bilgi için bkz. Azure Mantık Uygulamaları ile yinelenen görevler ve iş akışları oluşturun ve [çalıştırın.](../connectors/connectors-native-recurrence.md) |
   ||||

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

1. Şimdi, tetikleyici sonuçlarıyla gerçekleştirmek istediğiniz görevler için mantık uygulamanıza bir veya daha fazla eylem eklemeye devam edin. 

   Örneğin, olayları kategori gibi belirli bir değere göre filtrelemek için, **olay** gönder eyleminin yalnızca durumunuzu karşılayan olayları göndermesi için bir koşul ekleyebilirsiniz. 

> [!NOTE]
> Tüm Event Hub tetikleyicileri *uzun yoklama* tetikleyicileridir, bu da tetikleyici nin tüm olayları işlediği ve olay hub'ınızda daha fazla olayın görünmesi için 30 saniye beklediği anlamına gelir.
> 30 saniye içinde hiçbir olay alınmazsa, tetikleyici çalıştırma atlanır. Aksi takdirde, tetikleyici Olay Hub'ınız boşalana kadar olayları okumaya devam edin.
> Bir sonraki tetikleyici yoklaması, tetikleyicinin özelliklerinde belirttiğiniz yineleme aralığına göre gerçekleşir.

<a name="add-action"></a>

## <a name="add-event-hubs-action"></a>Olay Hub'ları eylemi ekleme

Azure Logic Apps'ta [eylem,](../logic-apps/logic-apps-overview.md#logic-app-concepts) iş akışınızda bir tetikleyici veya başka bir eylemi izleyen bir adımdır. Bu örnekte, mantık uygulaması Olay Hub'ınızdaki yeni olayları denetleyen bir Olay Hub'ları tetikleyicisiyle başlar.

1. Azure portalında veya Visual Studio'da mantık uygulamanızı Logic Apps Designer'da açın. Bu örnekte Azure portalı kullanır.

1. Tetikleyici veya eylem altında **Yeni adım'ı**seçin.

   Varolan adımlar arasında eylem eklemek için farenizi bağlanan okun üzerine taşıyın. 
   Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Arama kutusuna filtreniz olarak "olay hub'ları" girin.
Eylemler listesinden şu eylemi seçin: **Olay gönder - Olay Hub'ları**

   !["Olay gönder" eylemini seçin](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

1. Bağlantı ayrıntıları için istenirseniz, [Olay Hub'ları bağlantınızı şimdi oluşturun.](#create-connection) 

1. Eylemde, göndermek istediğiniz olaylar hakkında bilgi sağlayın. Daha fazla özellik için **yeni parametre** ekle listesini açın. Bir parametre seçmek, bu özelliği eylem kartına ekler.

   ![Olay Hub adını seçin ve etkinlik içeriği sağlayın](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Olay Hub'ı adı** | Evet | Olayı göndermek istediğiniz Olay Hub'ı |
   | **İçerik** | Hayır | Göndermek istediğiniz etkinliğin içeriği |
   | **Özellikler** | Hayır | Gönderilecek uygulama özellikleri ve değerleri |
   | **Bölüm anahtarı** | Hayır | Olayın nereye gönderilen [bölüm](../event-hubs/event-hubs-features.md#partitions) kimliği |
   ||||

   Örneğin, Olay Hub'ları tetikleyicinizden çıktıyı başka bir Olay Hub'ına gönderebilirsiniz:

   ![Olay örneği gönder](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action-example.png)

1. Bittiğinde, tasarımcı araç çubuğunda **Kaydet'i**seçin.

<a name="create-connection"></a>

## <a name="connect-to-your-event-hub"></a>Etkinlik Merkezinize bağlanın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Bağlantı bilgileri istendiğinde şu ayrıntıları sağlayın:

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Bağlantı Adı** | Evet | <*bağlantı adı*> | Bağlantınız için oluşturulacak ad |
   | **Olay Hub'ları Ad Alanı** | Evet | <*olay hub'ları-ad alanı*> | Kullanmak istediğiniz Etkinlik Hub'ları ad alanını seçin. |
   |||||  

   Örnek:

   ![Olay Hub bağlantısı oluşturma](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

   Bağlantı dizesini el ile girmek için **bağlantı bilgilerini El Ile girin'i**seçin. 
   [Bağlantı dizenizi nasıl bulacağınızı](#permissions-connection-string)öğrenin.

2. Zaten seçilmemişse kullanılacak Olay Hub'ları ilkesini seçin. **Oluştur**’u seçin.

   ![Olay Hub bağlantısı oluşturma, bölüm 2](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Bağlantınızı oluşturduktan [sonra, Olay Hub'ları Ekle tetikleyicisi](#add-trigger) veya [Olay Hub'ları Ekle eylemiyle](#add-action)devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve sınırlar gibi teknik ayrıntılar için, bağlayıcının Swagger dosyasında açıklandığı gibi, [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/eventhubs/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin