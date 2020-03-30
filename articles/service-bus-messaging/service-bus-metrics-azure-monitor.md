---
title: Azure Monitör'de Azure Hizmet Veri Meskeni ölçümleri| Microsoft Dokümanlar
description: Bu makalede, Hizmet Veri Hizmeti kuruluşları (kuyruklar, konular ve abonelikler) izlemek için Azure Monitor'un nasıl kullanılacağı açıklanmaktadır.
services: service-bus-messaging
documentationcenter: .NET
author: axisc
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/27/2020
ms.author: aschhab
ms.openlocfilehash: 227dfaff211eb60c5c2b25b5c76ecc82b6ce3edc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240796"
---
# <a name="azure-service-bus-metrics-in-azure-monitor"></a>Azure Monitör'de Azure Hizmet Veri Meskeni ölçümleri

Service Bus ölçümleri, Azure aboneliğinizdeki kaynakların durumunu sağlar. Zengin bir ölçüm veri kümesiyle, Yalnızca ad alanı düzeyinde değil, varlık düzeyinde de Hizmet Veri Hizmeti Veri Birimi kaynaklarınızın genel durumunu değerlendirebilirsiniz. Bu istatistikler, Servis Veri Servisi'nin durumunu izlemenize yardımcı olduğu için önemli olabilir. Ölçümler, Azure desteğine başvurmaya gerek kalmadan kök neden sorunlarının giderilmesine de yardımcı olabilir.

Azure Monitor, çeşitli Azure hizmetlerinde izleme için birleşik kullanıcı arabirimleri sağlar. Daha fazla bilgi için Microsoft [Azure'da İzleme](../monitoring-and-diagnostics/monitoring-overview.md) ve GitHub'da .NET örneği [içeren Azure Monitörünü Al ölçütlerine](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) bakın.

> [!IMPORTANT]
> Bir varlıkla 2 saat boyunca herhangi bir etkileşim olmadığında, ölçümler varlık boşta kalana kadar "0"ı değer olarak göstermeye başlar.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure Monitor ölçümlere erişmek için birden çok yol sağlar. Azure [portalı](https://portal.azure.com)üzerinden ölçümlere erişebilir veya Azure Monitor API'lerini (REST ve .NET) ve Azure Monitor günlükleri ve Etkinlik Hub'ları gibi çözüm çözümlerini kullanabilirsiniz. Daha fazla bilgi için [Azure Monitor'daki Ölçümler'e](../azure-monitor/platform/data-platform-metrics.md)bakın.

Ölçümler varsayılan olarak etkinleştirilir ve en son 30 günlük verilere erişebilirsiniz. Verileri daha uzun süre saklamanız gerekiyorsa, metrik verileri bir Azure Depolama hesabına arşivleyebilirsiniz. Bu değer Azure Monitor'daki [tanı ayarlarında](../azure-monitor/platform/diagnostic-settings.md) yapılandırılır.

## <a name="access-metrics-in-the-portal"></a>Portaldaki erişim ölçümleri

[Azure portalında](https://portal.azure.com)ölçümleri zaman içinde izleyebilirsiniz. Aşağıdaki örnekte, hesap düzeyinde başarılı isteklerin ve gelen isteklerin nasıl görüntülenenedöneceği gösterilmektedir:

![][1]

Ölçümlere doğrudan ad alanı üzerinden de erişebilirsiniz. Bunu yapmak için, ad alanınızı seçin ve ardından **Ölçümler'i**tıklatın. Varlığın kapsamına filtre uygulanmış ölçümleri görüntülemek için varlığı seçin ve ardından **Ölçümler'i**tıklatın.

![][2]

Boyutları destekleyen ölçümler için, istenen boyut değerine filtre uygulamanız gerekir.

## <a name="billing"></a>Faturalandırma

Azure Monitor'daki Ölçümler ve Uyarılar, uyarı bazında ücretlendirilir. Bu ücretler, uyarı kurulumu olduğunda ve kaydedilmeden önce portalda kullanılabilir olmalıdır. 

Ölçümleri oluşturan verilerin doğrudan bu çözümler tarafından faturalandırıldırılan ek çözümler. Örneğin, metrik verileri bir Azure Depolama hesabına arşivlerseniz Azure Depolama tarafından faturalandırılırsınız. Gelişmiş analiz için metrik verileri Log Analytics'e aktarıyorsanız, Log Analytics tarafından faturalandırılırsınız.

Aşağıdaki ölçümler, hizmetinizin durumu hakkında genel bir bakış sağlar. 

> [!NOTE]
> Farklı bir ad altında taşındıkları için birkaç ölçümü küçümsüyoruz. Bu, başvurularınızı güncelleştirmenizi gerektirebilir. "Amortismana" anahtar kelimesi ile işaretlenmiş ölçümler ileriye dönük olarak desteklenmeyen.

Tüm ölçüm değerleri her dakika Azure Monitor'a gönderilir. Zaman tanecikleri, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm Servis Veri Yolunda ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="request-metrics"></a>İstek ölçümleri

Veri ve yönetim işlemleri isteklerinin sayısını sayar.

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
| Gelen İstekler| Belirli bir süre içinde Servis Otobüsü hizmetine yapılan istek sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Başarılı İstekler|Belirli bir süre içinde Servis Otobüsü hizmetine yapılan başarılı istek sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Sunucu Hataları|Belirli bir süre içinde Servis Veri Servisi'ndeki bir hata nedeniyle işlenmemiş istek sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Kullanıcı Hataları (aşağıdaki alt bölüme bakın)|Belirli bir dönemdeki kullanıcı hataları nedeniyle işlenmemiş istek sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Daraltılmış İstekler|Kullanım aşıldığından azaltılan istek sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|

### <a name="user-errors"></a>Kullanıcı hataları

Aşağıdaki iki hata türü kullanıcı hataları olarak sınıflandırılır:

1. İstemci tarafı hataları (HTTP bu 400 hata olacaktır).
2. [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception)gibi iletileri işlerken oluşan hatalar.


## <a name="message-metrics"></a>İleti ölçümleri

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
|Gelen Mesajlar|Belirli bir süre içinde Servis Veri Servisi'ne gönderilen etkinlik veya ileti sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Giden Mesajlar|Belirli bir süre içinde Servis Veri Servisi'nden alınan etkinlik veya ileti sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
| İletiler| Kuyruktaki/konudaki ileti sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Boyut: EntityName |
| ActiveMessages| Bir sırada/konudaki etkin iletilerin sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/> Boyut: EntityName |
| Ölü harfli mesajlar| Bir sırada/konudaki ölü harflerle yazılmış iletilerin sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama <br/>Boyut: EntityName |
| Zamanlanmış iletiler| Bir sırada/konudaki zamanlanmış iletilerin sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Ortalama  <br/> Boyut: EntityName |

> [!NOTE]
> Aşağıdaki ölçümler için değerler zaman içinde nokta değerleridir. Bu noktadan hemen sonra tüketilen gelen iletiler bu ölçümlere yansıtılmayabilir. 
> - İletiler
> - Etkin iletiler 
> - Ölü harfli mesajlar 
> - Zamanlanmış iletiler 

## <a name="connection-metrics"></a>Bağlantı ölçümleri

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
|Aktif Bağlantılar|Bir ad alanının yanı sıra bir varlıktaki etkin bağlantı sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|

## <a name="resource-usage-metrics"></a>Kaynak kullanım ölçümleri

> [!NOTE] 
> Aşağıdaki ölçümler yalnızca **premium** katmanda kullanılabilir. 
> 
> Premium katman ad alanı için herhangi bir kesinti için izlemek için önemli ölçümler şunlardır: **ad alanı başına CPU kullanımı** ve ad alanı başına bellek **boyutu.** Azure Monitor'u kullanarak bu ölçümler için [uyarılar ayarlayın.](../azure-monitor/platform/alerts-metric.md)
> 
> İzleyebileceğiniz diğer ölçüm şudur: **daraltılmış istekler.** Ad alanı bellek, CPU ve aracılı bağlantı sınırları içinde kaldığı sürece sorun olmamalıdır. Daha fazla bilgi için [Azure Hizmet Veri Hizmetleri Premium katmanında Azaltma'ya](service-bus-throttling.md#throttling-in-azure-service-bus-premium-tier) bakın

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
|Ad alanı başına CPU kullanımı|Ad alanının yüzde CPU kullanımı.<br/><br/> Birim: Yüzde <br/> Toplama Türü: Maksimum <br/> Boyut: EntityName|
|Ad alanı başına bellek boyutu kullanımı|Ad alanının yüzde bellek kullanımı.<br/><br/> Birim: Yüzde <br/> Toplama Türü: Maksimum <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Metrik boyutları

Azure Hizmet Veri Servisi, Azure Monitor'daki ölçümler için aşağıdaki boyutları destekler. Ölçümlerinize boyut ekleme isteğe bağlıdır. Boyutlar eklemezseniz, ölçümler ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|Varlıkadı| Service Bus, ad alanı altında ileti taraflarını destekler.|

## <a name="set-up-alerts-on-metrics"></a>Ölçümlerde uyarılar ayarlama

1. **Hizmet Veri Kurumu Ad Alanı** sayfasının **Ölçümler** **sekmesinde, uyarıları yapılandır'ı**seçin. 

    ![Ölçümler sayfası - Uyarıları yapılandır menüsünü yapılandır](./media/service-bus-metrics-azure-monitor/metrics-page-configure-alerts-menu.png)
2. Hedef **Seç** seçeneğini seçin ve **kaynak** seç sayfasında aşağıdaki eylemleri yapın: 
    1. Kaynak türü alanına göre Filtre için **Hizmet Veri Günü Ad Alanları'nı** seçin. **Filter by resource type** 
    2. Abonelik alanına **göre Filtre** için aboneliğinizi seçin.
    3. Listeden **servis veri günü ad alanını** seçin. 
    4. **Done** (Bitti) öğesini seçin. 
    
        ![Ad alanı seçin](./media/service-bus-metrics-azure-monitor/select-namespace.png)
1. **Ölçüt Ekle'yi**seçin ve **sinyal mantığı** sayfasında aşağıdaki işlemleri yapın:
    1. Sinyal türü için **Ölçümler'i**seçin. **Metrics** 
    2. Bir sinyal seçin. Örneğin: **Hizmet hataları**. 

        ![Sunucu hatalarını seçin](./media/service-bus-metrics-azure-monitor/select-server-errors.png)
    1. **Koşul**için **büyük'ün'ünü** seçin.
    2. **Zaman Toplamı**için **Toplam'ı** seçin. 
    3. **Eşik**için **5** girin. 
    4. **Done** (Bitti) öğesini seçin.    

        ![Koşul belirtin](./media/service-bus-metrics-azure-monitor/specify-condition.png)    
1. Oluştur **kuralı** **sayfasında, uyarı ayrıntılarını tanımla'yı**genişletin ve aşağıdaki eylemleri yapın:
    1. Uyarı için bir **ad** girin. 
    2. Uyarı için bir **açıklama** girin.
    3. Uyarı için **önem derecesini** seçin. 

        ![Uyarı ayrıntıları](./media/service-bus-metrics-azure-monitor/alert-details.png)
1. Oluştur **kuralı** sayfasında, **Eylem Grubunu Tanımla'yı**genişletin, **Yeni eylem grubunu**seçin ve eylem grubu ekle **sayfasında**aşağıdaki eylemleri yapın. 
    1. Eylem grubu için bir ad girin.
    2. Eylem grubu için kısa bir ad girin. 
    3. Aboneliğinizi seçin. 
    4. Kaynak grubunu seçin. 
    5. Bu geçiş için **EYLEM İsİm**için **e-posta gönder'i** girin.
    6. EYLEM TÜRÜ için **E-posta/SMS/Push/Voice** seçeneğini **belirleyin.** 
    7. **Ayrıntıları düzenle** seçeneğini belirleyin. 
    8. **E-posta/SMS/Push/Voice** sayfasında aşağıdaki işlemleri yapın:
        1. **E-posta**seçin. 
        2. **E-posta adresini**yazın. 
        3. **Tamam'ı**seçin.

            ![Uyarı ayrıntıları](./media/service-bus-metrics-azure-monitor/add-action-group.png)
        4. Eylem **grubu ekle** sayfasında **Tamam'ı**seçin. 
1. Oluştur **kuralı** sayfasında **uyarı kuralı nı oluştur'u**seçin. 

    ![Uyarı kuralı düğmesi oluşturma](./media/service-bus-metrics-azure-monitor/create-alert-rule.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure [Monitörü'ne genel bakışa](../monitoring-and-diagnostics/monitoring-overview.md)bakın.

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


