---
title: Tanılamayı ayarla
titleSuffix: Azure Digital Twins
description: Bkz. tanılama ayarlarıyla günlüğü etkinleştirme.
author: baanders
ms.author: baanders
ms.date: 7/28/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: d47bb7cc868c5733c6e36290f097fec783764cd3
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88003590"
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
        
        Bu seçenekler hakkında daha fazla ayrıntı için aşağıdaki [*Kategori ayrıntıları*](#category-details) bölümüne bakın.
     * **Hedef ayrıntıları**: günlükleri nereye göndermek istediğinizi seçin. Üç seçenekten herhangi bir birleşimini seçebilirsiniz:
        - Log Analytics’e gönderme
        - Bir depolama hesabına arşivle
        - Bir olay hub'ına akış yap

        Hedef seçiminiz için gerekliyse ek ayrıntılar doldurmanız istenebilir.  
    
4. Yeni ayarları kaydedin. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Tanılama Ayarları sayfasını ve eklenecek düğmeyi gösteren ekran görüntüsü":::

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Bundan sonra, olay, örneğiniz için **Tanılama ayarları** sayfasında yapılandırılan hedefte geri görüntülenir. 

## <a name="category-details"></a>Kategori ayrıntıları

Tanılama ayarlarını ayarlarken **Kategori ayrıntıları** altında seçilebilecekleri günlük kategorileri hakkında daha fazla ayrıntı aşağıda verilmiştir.

| Günlük kategorisi | Description |
| --- | --- |
| ADTModelsOperation | Modellerle ilgili tüm API çağrılarını günlüğe kaydet |
| ADTQueryOperation | Sorgularla ilgili tüm API çağrılarını günlüğe kaydet |
| ADTEventRoutesOperation | Olay rotalarıyla ilgili tüm API çağrılarını ve Azure dijital TWINS 'den olayların çıkış durumunu Event Grid, Event Hubs ve Service Bus gibi bir uç nokta hizmetine kaydedin |
| ADTDigitalTwinsOperation | Azure dijital TWINS ile ilgili tüm API çağrılarını günlüğe kaydet |

Her günlük kategorisi yazma, okuma, silme ve eylem işlemlerinden oluşur.  Bu haritanın REST API çağrılar aşağıdaki gibidir:

| Olay türü | REST API işlemler |
| --- | --- |
| Yazma | YERLEŞTIRME ve düzeltme eki uygulama |
| Okuma | GET |
| Sil | DELETE |
| Eylem | POST |

Her kategoride günlüğe kaydedilen işlemlerin ve karşılık gelen [Azure dijital TWINS REST API çağrılarının](https://docs.microsoft.com/rest/api/azure-digitaltwins/) kapsamlı bir listesi aşağıda verilmiştir. 

>[!NOTE]
> Her günlük kategorisi birkaç işlem/REST API çağrısı içerir. Aşağıdaki tabloda, her günlük kategorisi, sonraki günlük kategorisi listelenene kadar tüm işlemler/REST API çağrılarıyla eşlenir. 

| Günlük kategorisi | İşlem | Çağrılar ve diğer olaylar REST API |
| --- | --- | --- |
| ADTModelsOperation | Microsoft. DigitalTwins/modeller/yazma | Digital Ikizi modelleri güncelleştirme API 'SI |
|  | Microsoft. DigitalTwins/modeller/okuma | Kimlik ve liste API 'Lerine göre dijital Ikizi modelleri al |
|  | Microsoft. DigitalTwins/modeller/silme | Dijital Ikizi modelleri API 'YI Sil |
|  | Microsoft. DigitalTwins/modeller/eylem | Digital Ikizi modelleri API ekleme |
| ADTQueryOperation | Microsoft. DigitalTwins/sorgu/eylem | Sorgu TWINS API 'SI |
| ADTEventRoutesOperation | Microsoft. DigitalTwins/eventroutes/Write | Olay rotaları API ekleme |
|  | Microsoft. DigitalTwins/eventroutes/okuma | Kimliğe ve liste API 'Lerine göre olay yolları |
|  | Microsoft. DigitalTwins/eventrotalar/Sil | Olay rotaları API silme |
|  | Microsoft. DigitalTwins/eventroutes/eylem | Bir olayın bitiş noktası hizmetine çıkışı (API çağrısı değil) |
| ADTDigitalTwinsOperation | Microsoft. DigitalTwins/digitaltwıns/Write | Dijital TWINS ekleme, Ilişki ekleme, güncelleştirme, güncelleştirme bileşeni |
|  | Microsoft. DigitalTwins/digitaltwıns/okuma | Kimliğe göre dijital TWINS alma, bileşen alma, kimliğe göre Ilişki alma, gelen Ilişkileri listeleme, Ilişkileri listeleme |
|  | Microsoft. DigitalTwins/digitaltwıns/Delete | Dijital TWINS silme, Ilişkiyi silme |
|  | Microsoft. DigitalTwins/digitaltwıns/eylem | Dijital TWINS bileşen telemetrisi gönderme, telemetri gönderme |

## <a name="next-steps"></a>Sonraki adımlar

* Tanılamayı yapılandırma hakkında daha fazla bilgi için bkz. [*Azure kaynaklarınızdan günlük verilerini toplama ve*](../azure-monitor/platform/platform-logs-overview.md)kullanma.
* Azure dijital TWINS ölçümleri hakkında daha fazla bilgi için bkz. [*sorun giderme: Azure izleyici ile ölçümleri görüntüleme*](troubleshoot-metrics.md).
* Ölçümleriniz için uyarıların nasıl etkinleştirileceği hakkında bilgi için bkz. [*sorun giderme: uyarıları ayarlama*](troubleshoot-alerts.md).
