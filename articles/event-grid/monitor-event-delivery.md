---
title: Azure Event Grid ölçümleri görüntüleme ve uyarıları ayarlama
description: Bu makalede, Azure Event Grid konu ve Aboneliklerle ilgili ölçümleri görüntülemek ve bunlar üzerinde uyarı oluşturmak için Azure portal nasıl kullanılacağı açıklanır.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: spelluru
ms.openlocfilehash: e74d2d8982cac961aa65d6576c80a92cb53ce387
ms.sourcegitcommit: 51718f41d36192b9722e278237617f01da1b9b4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85100542"
---
# <a name="monitor-event-grid-message-delivery"></a>İleti teslimini izleme Event Grid 
Bu makalede, Event Grid konular ve abonelikler için ölçümleri görmek ve bunlar üzerinde uyarı oluşturmak üzere portalın nasıl kullanılacağı açıklanır. 

## <a name="metrics"></a>Ölçümler

Portal, olay iletilerinin teslim durumunun ölçümlerini görüntüler.

Konular için bazı ölçümler aşağıda verilmiştir:

* **Yayımlama başarılı**: olay konuya başarıyla gönderildi ve 2xx yanıtıyla işlendi.
* **Yayımlama başarısız oldu**: konuya gönderilen olay, ancak bir hata koduyla reddedildi.
* **Eşleşmeyen**: olay, konuya başarıyla yayımlandı, ancak olay aboneliğiyle eşleşmedi. Olay bırakıldı.

Abonelikler için bazı ölçümler aşağıda verilmiştir:

* **Teslim başarılı**: etkinlik, aboneliğin uç noktasına başarıyla teslim edildi ve 2xx yanıtı aldı.
* **Teslim başarısız oldu**: hizmet teslim etmeye her seferinde ve olay işleyicisi başarılı 2xx Kodu döndürmezse, **teslim başarısız** sayacı artırılır. Aynı olayı birden çok kez teslim etmeye ve başarısız olursa, her başarısızlık için **teslim başarısız** sayacı artırılır.
* **Vadesi geçen olaylar**: olay teslim edilmemiş ve tüm yeniden deneme girişimleri gönderildi. Olay bırakıldı.
* **Eşleşen olaylar**: konudaki olay, olay aboneliği tarafından eşleşti.

    > [!NOTE]
    > Ölçümlerin tam listesi için bkz. [Azure Event Grid tarafından desteklenen ölçümler](metrics.md).

## <a name="view-custom-topic-metrics"></a>Özel konu ölçümlerini görüntüleme

Özel bir konu yayımladıysanız, onun ölçümlerini görüntüleyebilirsiniz. 

1. [Azure Portal](https://portal.azure.com/)oturum açın.
2. Konunun arama çubuğunda **Event Grid konular**yazın ve ardından aşağı açılan listeden **Event Grid konular** ' ı seçin. 

    :::image type="content" source="./media/custom-event-quickstart-portal/select-event-grid-topics.png" alt-text="Event Grid konuları arayın ve seçin":::
3. Konu listesinden özel konu başlığı ' nı seçin. 

    :::image type="content" source="./media/monitor-event-delivery/select-custom-topic.png" alt-text="Özel konu başlığını seçin":::
4. **Event Grid konu** sayfasındaki özel olay konusu için ölçümleri görüntüleyin. Aşağıdaki görüntüde, abonelik vb. kaynak grubunu gösteren **temel** bileşenler bölümü simge durumuna küçültülmüş. 

    :::image type="content" source="./media/monitor-event-delivery/custom-topic-metrics.png" alt-text="Olay ölçümlerini görüntüle":::

**Event Grid konu** sayfasının **ölçümler** sekmesini kullanarak desteklenen ölçümlerle grafik oluşturabilirsiniz.

:::image type="content" source="./media/monitor-event-delivery/topics-metrics-page.png" alt-text="Konu-ölçümler sayfası":::

Ölçümler hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de ölçümler](../azure-monitor/platform/data-platform-metrics.md)

Örneğin, **yayımlanan olaylar** ölçümü için ölçüm grafiğine bakın.

:::image type="content" source="./media/monitor-event-delivery/custom-topic-metrics-example.png" alt-text="Yayımlanan olaylar ölçümü":::


## <a name="view-subscription-metrics"></a>Abonelik ölçümlerini görüntüle
1. Önceki bölümde bulunan adımları izleyerek **Event Grid konu** sayfasına gidin. 
2. Aşağıdaki örnekte gösterildiği gibi alt bölmeden aboneliği seçin. 

    :::image type="content" source="./media/monitor-event-delivery/select-event-subscription.png" alt-text="Olay aboneliği seçin":::    

    Ayrıca, bir olay aboneliğini görmek için Azure portal arama çubuğunda **Event Grid abonelikleri** arayabilir, **konu türü**, **abonelik**ve **konum** ' u seçin. 

    :::image type="content" source="./media/monitor-event-delivery/event-subscriptions-page.png" alt-text="Event Grid abonelikler sayfasından olay aboneliği seçin":::        

    Özel Konular için **konu türü**olarak **Event Grid konular** ' ı seçin. Sistem konuları için Azure kaynağının türünü seçin (örneğin, **depolama hesapları (blob, GPv2)**. 
3. Bir grafikteki aboneliğin ana sayfasında, abonelik ölçümlerine bakın. Son 1 saat, 6 saat, 12 saat, 1 gün, 7 gün veya 30 gün için **genel**, **hata**, **gecikme süresi**ve **atılacak mektup** ölçümlerini görebilirsiniz. 

    :::image type="content" source="./media/monitor-event-delivery/subscription-home-page-metrics.png" alt-text="Abonelik giriş sayfasındaki ölçümler":::    

## <a name="view-system-topic-metrics"></a>Sistem konu ölçümlerini görüntüleme

1. [Azure Portal](https://portal.azure.com/)oturum açın.
2. Konunun arama çubuğunda **Event Grid sistem konuları**yazın ve ardından açılır listeden **Event Grid sistem konuları** ' nı seçin. 

    :::image type="content" source="./media/monitor-event-delivery/search-system-topics.png" alt-text="Event Grid sistem konularını arayın ve seçin":::
3. Konu listesinden sistem konu başlığını seçin. 

    :::image type="content" source="./media/monitor-event-delivery/select-system-topic.png" alt-text="Sistem konusunu seçin":::
4. **Event Grid sistem konusu** sayfasında sistem konusunun ölçümlerini görüntüleyin. Aşağıdaki görüntüde, abonelik vb. kaynak grubunu gösteren **temel** bileşenler bölümü simge durumuna küçültülmüş. 

    :::image type="content" source="./media/monitor-event-delivery/system-topic-overview-metrics.png" alt-text="Genel Bakış sayfasında sistem konu ölçümlerini görüntüleme":::

**Event Grid konu** sayfasının **ölçümler** sekmesini kullanarak desteklenen ölçümlerle grafik oluşturabilirsiniz.

:::image type="content" source="./media/monitor-event-delivery/system-topic-metrics-page.png" alt-text="Sistem konusu-ölçümler sayfası":::

Ölçümler hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de ölçümler](../azure-monitor/platform/data-platform-metrics.md)


## <a name="set-alerts"></a>Uyarı ayarlama
Azure portal konular ve etki alanları için ölçümler üzerinde uyarılar ayarlayabilirsiniz. 

Aşağıdaki yordamda, özel bir konu için **kullanılmayan olaylar** ölçümünde uyarı oluşturma işlemi gösterilmektedir. Bu örnekte, bir konu için atılacak olay sayısı 10 ' dan fazla kaldığında Azure Kaynak grubu sahibine bir e-posta gönderilir. 

1. Konunun **Event Grid konu** sayfasında, sol taraftaki menüden **Uyarılar** ' ı seçin ve ardından **+ Yeni uyarı kuralı**' nı seçin. 

    :::image type="content" source="./media/monitor-event-delivery/new-alert-button.png" alt-text="Uyarılar sayfası-yeni uyarı kuralı düğmesi":::
    
    
    Ayrıca **ölçümler** sayfasını kullanarak da uyarılar oluşturabilirsiniz. Adımlar benzerdir. 

    :::image type="content" source="./media/monitor-event-delivery/metric-page-create-alert-button.png" alt-text="Ölçümler sayfası-uyarı oluştur düğmesi":::   
    
2. **Uyarı kuralı oluştur** sayfasında, konunun kaynak için seçildiğini doğrulayın. Sonra **Koşul Seç**' e tıklayın. 

    :::image type="content" source="./media/monitor-event-delivery/alert-select-condition.png" alt-text="Uyarılar sayfası-koşul Seç":::    
3. **Sinyal mantığını Yapılandır** sayfasında, aşağıdaki adımları izleyin:
    1. Bir ölçüm veya etkinlik günlüğü girişi seçin. Bu örnekte, **atılacak olaylar** seçilidir. 

        :::image type="content" source="./media/monitor-event-delivery/select-dead-lettered-events.png" alt-text="Kullanılmayan olayları seçin":::        
    2. Boyutları seçin (isteğe bağlı). 
        
        :::image type="content" source="./media/monitor-event-delivery/configure-signal-logic.png" alt-text="Sinyal mantığını yapılandırma":::        
    3. Aşağı kaydırın. **Uyarı mantığı** bölümünde bir **operatör**, **toplama türü**seçin ve bir **eşik değeri**girin ve **bitti**' yi seçin. Bu örnekte, toplam ölü olay sayısı 10 ' dan büyük olduğunda bir uyarı tetiklenir. 
    
        :::image type="content" source="./media/monitor-event-delivery/alert-logic.png" alt-text="Uyarı mantığı":::                
4. **Uyarı kuralı oluştur** sayfasında, **eylem grubu seç**' e tıklayın.

    :::image type="content" source="./media/monitor-event-delivery/select-action-group-button.png" alt-text="Eylem grubu Seç düğmesi":::
5. Yeni bir eylem grubu oluşturmak için araç çubuğunda **eylem grubu oluştur** ' u seçin. Ayrıca var olan bir eylem grubunu seçebilirsiniz.        
6. **Eylem grubu Ekle** sayfasında, şu adımları izleyin:
    1. **Eylem grubu için bir ad**girin.
    1. Eylem grubu için **kısa bir ad** girin.
    1. Eylem grubunu oluşturmak istediğiniz **Azure aboneliğini** seçin.
    1. Eylem grubunu oluşturmak istediğiniz **Azure kaynak grubunu** seçin.
    1. **Eylem için bir ad**girin. 
    1. **Eylem türünü**seçin. Bu örnekte, **e-posta Azure Resource Manager rolü** , özellikle **sahipler** rolü seçilidir. 
    1. Sayfayı kapatmak için **Tamam ' ı** seçin. 
    
        :::image type="content" source="./media/monitor-event-delivery/add-action-group-page.png" alt-text="Eylem grubu Ekle sayfası":::                   
7. **Uyarı kuralı oluştur** sayfasına dönün, uyarı kuralı için bir ad girin ve ardından **Uyarı kuralı oluştur**' u seçin.

    :::image type="content" source="./media/monitor-event-delivery/alert-rule-name.png" alt-text="Uyarı kuralı adı":::  
8. Artık, konunun **Uyarılar** sayfasında uyarı kurallarını yönetmek için henüz uyarı yoksa bir bağlantı görürsünüz. Uyarılar varsa, araç çubuğunda **Yönetici uyarı kuralları** ' nı seçin.  

    :::image type="content" source="./media/monitor-event-delivery/manage-alert-rules.png" alt-text="Uyarıları yönetme":::

    > [!NOTE]
    > Bu makalede, bir uyarı oluşturmak için kullanabileceğiniz tüm farklı adımlar ve birleşimler ele alınmaktadır. Uyarılara genel bakış için bkz. [uyarılara genel bakış](../azure-monitor/platform/alerts-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeler hakkında daha fazla bilgi için [Event Grid ileti teslimi ve yeniden deneyin](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid kullanmaya hızlıca başlamak için bkz. [özel olayları oluşturma ve Azure Event Grid ile yönlendirme](custom-event-quickstart.md).
