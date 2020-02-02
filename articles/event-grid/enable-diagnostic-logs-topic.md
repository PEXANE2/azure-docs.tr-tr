---
title: Azure Event Grid-bir konu için tanılama günlüklerini etkinleştirme
description: Bu makalede, Azure Event Grid için tanılama günlüklerini etkinleştirme konusunda adım adım yönergeler sağlanmaktadır.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: a32d26629bb9efed4a4f1f49eee2f0534e1873a9
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76960509"
---
#  <a name="diagnostic-logs-for-an-azure-event-grid-topic"></a>Bir Azure olay Kılavuzu konusu için tanılama günlükleri
Tanılama ayarları Event Grid kullanıcıların yayımlama ve teslim hatası günlüklerini şu konumlardan birinde yakalayıp görüntülemesine izin verir: bir Azure depolama hesabı, bir olay hub 'ı veya bir Log Analytics çalışma alanı. Bu makalede, bir olay Kılavuzu konusu için tanılama günlüklerini etkinleştirmek üzere adım adım yönergeler sağlanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

- Sağlanan olay Kılavuzu konusu
- Tanılama günlüklerini yakalamak için sağlanan hedef. Bu, aşağıdaki hedefden biri olabilir:
    - Azure depolama hesabı
    - Olay hub'ı
    - Log Analytics çalışma alanı


## <a name="steps-for-enabling-diagnostic-logs-for-a-topic"></a>Bir konu için tanılama günlüklerini etkinleştirme adımları

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Tanılama günlük ayarlarını etkinleştirmek istediğiniz olay kılavuzu konusuna gidin. 
3. Sol menüde Izleme altında **Tanılama ayarları** ' **nı** seçin.
4. **Tanılama ayarları** sayfasında **yeni tanılama ayarı Ekle**' yi seçin. 
    
    ![Tanılama ayarı Ekle düğmesi](./media/enable-diagnostic-logs-topic/diagnostic-settings-add.png)
5. Tanılama ayarı için bir **ad** belirtin. 

    ![Tanılama ayarları-ad](./media/enable-diagnostic-logs-topic/diagnostic-settings-name.png)     
6. Günlükler için bir veya daha fazla yakalama hedefini etkinleştirin ve ardından önceden oluşturulmuş bir yakalama kaynağını seçerek bunları yapılandırın. 
    - **Bir depolama hesabına arşiv**' i seçerseniz, **depolama hesabı-Yapılandır**' ı seçin ve ardından Azure aboneliğinizde depolama hesabını seçin. 

        ![Azure depolama hesabına Arşivle](./media/enable-diagnostic-logs-topic/archive-storage.png)
    - **Bir olay hub 'ına akış**' yı seçerseniz, **Olay Hub**'ı ' nı seçin ve Event Hubs ad alanı, Olay Hub 'ı ve erişim ilkesini seçin. 
        bir olay hub 'ına akış ![](./media/enable-diagnostic-logs-topic/archive-event-hub.png)
    - **Log Analytics gönder**' i seçerseniz Log Analytics çalışma alanını seçin.
        Log Analytics](./media/enable-diagnostic-logs-topic/send-log-analytics.png) ![gönder
7. **Günlük** bölümünde **Deliveryhatalarıyla** ve **publisharızaları** seçeneklerini belirleyin. 
    ![sorunları seçin](./media/enable-diagnostic-logs-topic/log-failures.png)
8. **Kaydet**’i seçin. Sağ köşedeki **X** ' i seçerek sayfayı kapatın. 
9. Şimdi **Tanılama ayarları** sayfasına geri döndüğünüzde, **Tanılama ayarları** tablosunda yeni bir giriş görtığınızdan emin olun. 
    Listedeki tanılama ayarını ![](./media/enable-diagnostic-logs-topic/diagnostic-setting-list.png)

     Konunun tüm ölçümlerinin toplanmasını de etkinleştirebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa sorununuzu [Stack Overflow forumuna](https://stackoverflow.com/questions/tagged/azure-eventgrid) gönderin veya bir [destek bileti](https://azure.microsoft.com/support/options/)açın. 
