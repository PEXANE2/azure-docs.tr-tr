---
title: Tanılamayı ayarla
titleSuffix: Azure Digital Twins
description: Bkz. tanılama ayarlarıyla günlüğü etkinleştirme.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: 0376a57e3f2c1158e9da97fb291a28c99ce2463c
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87903977"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logging"></a>Azure dijital TWINS sorunlarını giderme: tanılama günlüğü

Azure dijital TWINS, kaynaklarınızın durumu hakkında bilgi veren hizmet örneğiniz için [ölçümleri](troubleshoot-metrics.md) toplar. Bu ölçümleri, Azure Digital TWINS hizmetinin genel sistem durumunu ve ona bağlı kaynakları değerlendirmek için kullanabilirsiniz. Bu kullanıcıya yönelik istatistikler, Azure dijital Tağınızda neler olduğunu görmenizi ve Azure desteği ile iletişim kurmaya gerek kalmadan sorunlar için kök neden analizi gerçekleştirmeye yardımcı olur.

Bu makalede, Azure dijital TWINS örneğinden ölçüm verileriniz için **tanılama günlüğü açma işleminin** nasıl yapılacağı gösterilir. Hizmet sorunlarını gidermenize yardımcı olması için bu günlükleri kullanabilir ve Azure dijital TWINS ölçümlerini farklı hedeflere göndermek için tanılama ayarlarını yapılandırabilirsiniz. Bu ayarlar hakkında daha fazla bilgi [*için Tanılama ayarları oluşturma ' da, farklı hedeflere platform günlükleri ve ölçümleri gönderebilirsiniz*](../azure-monitor/platform/diagnostic-settings.md).

## <a name="turn-on-diagnostic-settings-with-the-azure-portal"></a>Tanılama ayarlarını Azure portal açın

Azure dijital TWINS örneğiniz için tanılama ayarlarının nasıl etkinleştirileceği aşağıda verilmiştir:

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure dijital TWINS örneğinize gidin. Bunu, adını Portal arama çubuğuna yazarak bulabilirsiniz. 

2. Menüden **Tanılama ayarları** ' nı seçin ve ardından **Tanılama ayarı ekleyin**.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Tanılama Ayarları sayfasını ve eklenecek düğmeyi gösteren ekran görüntüsü":::

3. Aşağıdaki sayfada aşağıdaki değerleri girin:
     * **Tanılama ayarı adı**: Tanılama ayarlarına bir ad verin.
     * **Kategori ayrıntıları**: hangi işlemleri izlemek istediğinizi seçin ve bu işlemler için tanılamayı etkinleştirmek üzere kutulara göz atın. Tanılama ayarlarının rapor aldığı işlemler şunlardır:
        - DigitalTwinsOperation
        - EventRoutesOperation
        - ModelsOperation
        - Queryoperation başvurusu destekleniyor
        - Tüm Ölçümler
     * **Hedef ayrıntıları**: günlükleri nereye göndermek istediğinizi seçin. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:
        - Log Analytics’e gönderme
        - Bir depolama hesabına arşivle
        - Bir olay hub'ına akış yap

        Hedef seçiminiz için gerekliyse ek ayrıntılar doldurmanız istenebilir.  
    
4. Yeni ayarları kaydedin. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Tanılama Ayarları sayfasını ve eklenecek düğmeyi gösteren ekran görüntüsü":::

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, olay, örneğiniz için **Tanılama ayarları** sayfasında yapılandırılan hedefte geri görüntülenir. 

## <a name="next-steps"></a>Sonraki adımlar

* Tanılamayı yapılandırma hakkında daha fazla bilgi için bkz. [*Azure kaynaklarınızdan günlük verilerini toplama ve*](../azure-monitor/platform/platform-logs-overview.md)kullanma.
* Azure dijital TWINS ölçümleri hakkında daha fazla bilgi için bkz. [*sorun giderme: Azure izleyici ile ölçümleri görüntüleme*](troubleshoot-metrics.md).
* Ölçümleriniz için uyarıların nasıl etkinleştirileceği hakkında bilgi için bkz. [*sorun giderme: uyarıları ayarlama*](troubleshoot-alerts.md).
