---
title: Azure Olay Kılavuz - Bir konu için tanılama günlüklerini etkinleştir
description: Bu makalede, bir Azure olay ızgarası konusu için tanı günlüklerinin nasıl etkinleştirilen hakkında adım adım yönergeler verilmektedir.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: how-to
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: a32d26629bb9efed4a4f1f49eee2f0534e1873a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76960509"
---
#  <a name="diagnostic-logs-for-an-azure-event-grid-topic"></a>Azure olay ızgarası konusu için tanılama günlükleri
Tanılama ayarları, Event Grid kullanıcılarının yayımlama ve teslim hatası Günlüklerini aşağıdaki yerlerden birinde yakalamasına ve görüntülemesine olanak tanır: Azure depolama hesabı, etkinlik merkezi veya Günlük Analizi çalışma alanı. Bu makalede, bir olay ızgarası konusu için tanılama günlüklerini etkinleştirmek için adım adım yönergeler sağlar.

## <a name="prerequisites"></a>Ön koşullar

- Sağlanan olay ızgarası konusu
- Tanılama günlüklerini yakalamak için uygun bir hedef. Aşağıdaki noktalardan biri olabilir:
    - Azure depolama hesabı
    - Olay hub'ı
    - Log Analytics çalışma alanı


## <a name="steps-for-enabling-diagnostic-logs-for-a-topic"></a>Bir konu için tanıgünlüklerini etkinleştirme adımları

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Tanılama günlüğü ayarlarını etkinleştirmek istediğiniz olay ızgarası konusuna gidin. 
3. Sol menüde **İzleme** altında **Tanılama ayarlarını** seçin.
4. **Tanılama ayarları** sayfasında **Yeni Tanılama Ayarı Ekle'yi**seçin. 
    
    ![Tanılama ayar düğmesi ekle](./media/enable-diagnostic-logs-topic/diagnostic-settings-add.png)
5. Tanılama ayarı için bir **ad** belirtin. 

    ![Tanılama ayarları - ad](./media/enable-diagnostic-logs-topic/diagnostic-settings-name.png)     
6. Günlükler için yakalama hedeflerinden birini veya birkaçını etkinleştirin ve daha önce oluşturulmuş bir yakalama kaynağı seçerek bunları yapılandırın. 
    - **Bir depolama hesabına Arşiv'i**seçerseniz, Depolama hesabı - **Yapılandır'ı**seçin ve ardından Azure aboneliğinizdeki depolama hesabını seçin. 

        ![Azure depolama hesabına arşivleme](./media/enable-diagnostic-logs-topic/archive-storage.png)
    - **Bir olay hub'ına Akış'ı**seçerseniz, Olay hub'ını seçin - **Yapılandır'** ı seçin ve ardından Olay Hub'ları ad alanını, etkinlik merkezini ve erişim ilkesini seçin. 
        ![Olay merkezine akış](./media/enable-diagnostic-logs-topic/archive-event-hub.png)
    - **Günlük Analitiğine Gönder'i**seçerseniz, Günlük Analitiği çalışma alanını seçin.
        ![Log Analytics’e gönderme](./media/enable-diagnostic-logs-topic/send-log-analytics.png)
7. **Günlük** **bölümündeki Teslim Hataları** ve **Yayımlama Hataları** seçeneklerini seçin. 
    ![Hataları seçin](./media/enable-diagnostic-logs-topic/log-failures.png)
8. **Kaydet'i**seçin. Sayfayı kapatmak için sağ köşede **X'i** seçin. 
9. Şimdi, **Tanılama ayarları** sayfasında, **Tanılama Ayarları** tablosunda yeni bir giriş gördüğünüzden onaylayın. 
    ![Listedeki tanılama ayarı](./media/enable-diagnostic-logs-topic/diagnostic-setting-list.png)

     Ayrıca, konuyla ilgili tüm ölçümlerin toplanmasını da etkinleştirebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa, sorununuzu [Stack Overflow forumunda](https://stackoverflow.com/questions/tagged/azure-eventgrid) yayınla veya bir [destek bileti](https://azure.microsoft.com/support/options/)açın. 
