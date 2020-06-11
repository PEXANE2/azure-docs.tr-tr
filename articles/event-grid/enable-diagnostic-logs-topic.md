---
title: Azure Event Grid-konular veya etki alanları için tanılama günlüklerini etkinleştirme
description: Bu makalede, Azure Event Grid için tanılama günlüklerini etkinleştirme konusunda adım adım yönergeler sağlanmaktadır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 06/10/2020
ms.author: spelluru
ms.openlocfilehash: 253d1fb933c32735f68cf6a2d471a7687caf5301
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84670177"
---
#  <a name="enable-diagnostic-logs-for-azure-event-grid-topics-or-domains"></a>Azure Event Grid konuları veya etki alanları için tanılama günlüklerini etkinleştirme
Tanılama ayarları Event Grid kullanıcıların bir depolama hesabında, Olay Hub 'ında veya bir Log Analytics çalışma alanında **Yayımlama ve teslim hatası** günlüklerini yakalayıp görüntülemesine olanak tanır. Bu makalede, Event Grid konu başlığında bu ayarları etkinleştirmek için adım adım yönergeler sağlanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- Sağlanan olay Kılavuzu konusu
- Tanılama günlüklerini yakalamak için sağlanan hedef. Bu, olay Kılavuzu konusu ile aynı konumdaki aşağıdaki hedefden biri olabilir:
    - Azure depolama hesabı
    - Olay hub'ı
    - Log Analytics çalışma alanı

## <a name="enable-diagnostic-logs-for-a-custom-topic"></a>Özel konu için tanılama günlüklerini etkinleştirme

> [!NOTE]
> Aşağıdaki yordam, bir konu için tanılama günlüklerini etkinleştirmeye yönelik adım adım yönergeler sağlar. Bir etki alanı için tanılama günlüklerini etkinleştirme adımları çok benzerdir. 2. adımda, Azure portal olay Kılavuzu **etki alanına** gidin.  

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Tanılama günlük ayarlarını etkinleştirmek istediğiniz olay kılavuzu konusuna gidin. 
    1. Üstteki arama çubuğunda **Event Grid konular**' ı arayın. 
    
        ![Özel konuları ara](./media/enable-diagnostic-logs-topic/search-custom-topics.png)
    1. Tanılama ayarlarını yapılandırmak istediğiniz listeden **konuyu** seçin. 
1. Sol menüde Izleme altında **Tanılama ayarları** ' **nı** seçin.
1. **Tanılama ayarları** sayfasında **yeni tanılama ayarı Ekle**' yi seçin. 
    
    ![Tanılama ayarı Ekle düğmesi](./media/enable-diagnostic-logs-topic/diagnostic-settings-add.png)
5. Tanılama ayarı için bir **ad** belirtin. 
7. **Günlük** bölümünde **Deliveryhatalarıyla** ve **publisharızaları** seçeneklerini belirleyin. 
    ![Arızaları seçin](./media/enable-diagnostic-logs-topic/log-failures.png)
6. Günlükler için bir veya daha fazla yakalama hedefini etkinleştirin ve ardından önceden oluşturulmuş bir yakalama kaynağını seçerek bunları yapılandırın. 
    - **Bir depolama hesabına arşiv**' i seçerseniz, **depolama hesabı-Yapılandır**' ı seçin ve ardından Azure aboneliğinizde depolama hesabını seçin. 

        ![Azure depolama hesabına Arşivle](./media/enable-diagnostic-logs-topic/archive-storage.png)
    - **Bir olay hub 'ına akış**' yı seçerseniz, **Olay Hub**'ı ' nı seçin ve Event Hubs ad alanı, Olay Hub 'ı ve erişim ilkesini seçin. 
        ![Bir olay hub 'ına akış](./media/enable-diagnostic-logs-topic/archive-event-hub.png)
    - **Log Analytics gönder**' i seçerseniz Log Analytics çalışma alanını seçin.
        ![Log Analytics’e gönderme](./media/enable-diagnostic-logs-topic/send-log-analytics.png)
8. **Kaydet**'i seçin. Sonra sağ köşedeki **X** ' i seçerek sayfayı kapatın. 
9. Şimdi **Tanılama ayarları** sayfasına geri döndüğünüzde, **Tanılama ayarları** tablosunda yeni bir giriş görtığınızdan emin olun. 
    ![Listedeki tanılama ayarı](./media/enable-diagnostic-logs-topic/diagnostic-setting-list.png)

     Konunun tüm ölçümlerinin toplanmasını de etkinleştirebilirsiniz. 

## <a name="enable-diagnostic-logs-for-a-system-topic"></a>Bir sistem konusu için tanılama günlüklerini etkinleştirme

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Tanılama günlük ayarlarını etkinleştirmek istediğiniz olay kılavuzu konusuna gidin. 
    1. Üstteki arama çubuğunda **Event Grid sistem konuları**' nı arayın. 
    
        ![Sistem konularını ara](./media/enable-diagnostic-logs-topic/search-system-topics.png)
    1. Tanılama ayarlarını yapılandırmak istediğiniz **Sistem konusunu** seçin. 
    
        ![Sistem konusunu seçin](./media/enable-diagnostic-logs-topic/select-system-topic.png)
3. Sol menüde **izleme** altında **Tanılama ayarları** ' nı seçin ve ardından **Tanılama ayarı Ekle**' yi seçin. 

    ![Tanılama ayarları ekleme-düğme](./media/enable-diagnostic-logs-topic/system-topic-add-diagnostic-settings-button.png)
4. Tanılama ayarı için bir **ad** belirtin. 
7. **Günlük** bölümünde **deliverybaşarısızlıklarını** seçin. 
    ![Teslim başarısızlıklarını seçin](./media/enable-diagnostic-logs-topic/system-topic-select-delivery-failures.png)
6. Günlükler için bir veya daha fazla yakalama hedefini etkinleştirin ve ardından önceden oluşturulmuş bir yakalama kaynağını seçerek bunları yapılandırın. 
    - **Log Analytics gönder**' i seçerseniz Log Analytics çalışma alanını seçin.
        ![Log Analytics’e gönderme](./media/enable-diagnostic-logs-topic/system-topic-select-log-workspace.png) 
    - **Bir depolama hesabına arşiv**' i seçerseniz, **depolama hesabı-Yapılandır**' ı seçin ve ardından Azure aboneliğinizde depolama hesabını seçin. 

        ![Azure depolama hesabına Arşivle](./media/enable-diagnostic-logs-topic/system-topic-select-storage-account.png)
    - **Bir olay hub 'ına akış**' yı seçerseniz, **Olay Hub**'ı ' nı seçin ve Event Hubs ad alanı, Olay Hub 'ı ve erişim ilkesini seçin. 
        ![Bir olay hub 'ına akış](./media/enable-diagnostic-logs-topic/system-topic-select-event-hub.png)
8. **Kaydet**'i seçin. Sonra sağ köşedeki **X** ' i seçerek sayfayı kapatın. 
9. Şimdi **Tanılama ayarları** sayfasına geri döndüğünüzde, **Tanılama ayarları** tablosunda yeni bir giriş görtığınızdan emin olun. 
    ![Listedeki tanılama ayarı](./media/enable-diagnostic-logs-topic/system-topic-diagnostic-settings-targets.png)

     Ayrıca sistem konusunun tüm **ölçümlerinin** toplanmasını etkinleştirebilirsiniz.

    ![Sistem konusu-tüm ölçümleri etkinleştir](./media/enable-diagnostic-logs-topic/system-topics-metrics.png)

## <a name="view-diagnostic-logs-in-azure-storage"></a>Azure depolama 'da tanılama günlüklerini görüntüleme 

1. Bir depolama hesabını yakalama hedefi olarak etkinleştirdikten ve Event Grid tanılama günlüklerini yaymaya başladıktan sonra depolama hesabında **Öngörüler-logs-deliveryhatalarıyla** ve içgörüler- **günlük-publishhatalarıyla** adlı yeni kapsayıcılar görmeniz gerekir. 

    ![Tanılama günlükleri için depolama kapsayıcıları](./media/enable-diagnostic-logs-topic/storage-containers.png)
2. Kapsayıcılardan birine gitmeniz sırasında JSON biçiminde bir bloba bitecaksınız. Dosya, bir teslim hatası ya da bir yayımlama hatası için günlük girişleri içeriyor. Gezinti yolu, olay Kılavuzu konusunun **RESOURCEID** ve günlük girişleri yayıldıkları zaman damgasına (dakika düzeyi) temsil eder. İndirilebilen blob/JSON dosyası, sonraki bölümde açıklanan şemaya uyar. 

    [![Depolama ](./media/enable-diagnostic-logs-topic/select-json.png) alanındaki JSON dosyası](./media/enable-diagnostic-logs-topic/select-json.png)
3. Aşağıdaki örneğe benzer şekilde JSON dosyasında içerik görmeniz gerekir: 

    ```json
    {
        "time": "2019-11-01T00:17:13.4389048Z",
        "resourceId": "/SUBSCRIPTIONS/SAMPLE-SUBSCTIPTION-ID /RESOURCEGROUPS/SAMPLE-RESOURCEGROUP-NAME/PROVIDERS/MICROSOFT.EVENTGRID/TOPICS/SAMPLE-TOPIC-NAME ",
        "eventSubscriptionName": "SAMPLEDESTINATION",
        "category": "DeliveryFailures",
        "operationName": "Deliver",
        "message": "Message:outcome=NotFound, latencyInMs=2635, systemId=17284f7c-0044-46fb-84b7-59fda5776017, state=FilteredFailingDelivery, deliveryTime=11/1/2019 12:17:10 AM, deliveryCount=0, probationCount=0, deliverySchema=EventGridEvent, eventSubscriptionDeliverySchema=EventGridEvent, fields=InputEvent, EventSubscriptionId, DeliveryTime, State, Id, DeliverySchema, LastDeliveryAttemptTime, SystemId, fieldCount=, requestExpiration=1/1/0001 12:00:00 AM, delivered=False publishTime=11/1/2019 12:17:10 AM, eventTime=11/1/2019 12:17:09 AM, eventType=Type, deliveryTime=11/1/2019 12:17:10 AM, filteringState=FilteredWithRpc, inputSchema=EventGridEvent, publisher=DIAGNOSTICLOGSTEST-EASTUS.EASTUS-1.EVENTGRID.AZURE.NET, size=363, fields=Id, PublishTime, SerializedBody, EventType, Topic, Subject, FilteringHashCode, SystemId, Publisher, FilteringTopic, TopicCategory, DataVersion, MetadataVersion, InputSchema, EventTime, fieldCount=15, url=sb://diagnosticlogstesting-eastus.servicebus.windows.net/, deliveryResponse=NotFound: The messaging entity 'sb://diagnosticlogstesting-eastus.servicebus.windows.net/eh-diagnosticlogstest' could not be found. TrackingId:c98c5af6-11f0-400b-8f56-c605662fb849_G14, SystemTracker:diagnosticlogstesting-eastus.servicebus.windows.net:eh-diagnosticlogstest, Timestamp:2019-11-01T00:17:13, referenceId: ac141738a9a54451b12b4cc31a10dedc_G14:"
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar
Günlük şeması ve konular veya etki alanları için tanılama günlükleri hakkında diğer kavramsal bilgiler için bkz. [tanılama günlükleri](diagnostic-logs.md).
