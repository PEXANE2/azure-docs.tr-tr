---
title: Azure Izleyici 'de ölçümleri Azure Service Bus | Microsoft Docs
description: Bu makalede, Azure Izleyici 'nin Service Bus varlıkları (kuyruklar, konular ve abonelikler) izlemek için nasıl kullanılacağı açıklanmaktadır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 57b791e67157908447956a14fae99545843f3bc0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85340287"
---
# <a name="azure-service-bus-metrics-in-azure-monitor"></a>Azure Izleyici 'de Azure Service Bus ölçümleri

Service Bus ölçümler, Azure aboneliğinizdeki kaynakların durumunu sağlar. Zengin bir ölçüm verileri kümesiyle, yalnızca ad alanı düzeyinde değil, aynı zamanda varlık düzeyinde Service Bus kaynaklarınızın genel durumunu değerlendirebilirsiniz. Bu istatistikler Service Bus durumunu izlemenize yardımcı olduklarından önemli olabilir. Ölçümler Ayrıca, Azure desteği ile iletişim kurmaya gerek kalmadan, kök neden sorunları gidermenize yardımcı olabilir.

Azure Izleyici, çeşitli Azure hizmetleri genelinde izleme için birleştirilmiş kullanıcı arabirimleri sağlar. Daha fazla bilgi için bkz. [Microsoft Azure 'de izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve GitHub 'da [.net örneği ile Azure izleyici ölçümlerini alma](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) .

> [!IMPORTANT]
> 2 saatlik bir varlıkla hiçbir etkileşim kalmadığında, varlık artık boşta kalmayacağı sürece ölçümler "0" değerini bir değer olarak göstermeye başlayacaktır.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure Izleyici, ölçümlere erişmek için birden çok yol sağlar. Ölçümlere [Azure Portal](https://portal.azure.com)aracılığıyla erişebilir veya Azure Izleyici API 'LERINI (REST ve .net) ve Azure izleyici günlükleri ve Event Hubs gibi çözümleme çözümlerini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici 'de ölçümler](../azure-monitor/platform/data-platform-metrics.md).

Ölçümler varsayılan olarak etkinleştirilmiştir ve en son 30 güne kadar verilere erişebilirsiniz. Verileri daha uzun bir süre tutmanız gerekiyorsa, ölçüm verilerini bir Azure depolama hesabına arşivleyebilirsiniz. Bu değer, Azure Izleyici 'de [Tanılama ayarları](../azure-monitor/platform/diagnostic-settings.md) 'nda yapılandırılır.

## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümleri

[Azure Portal](https://portal.azure.com)ölçümleri zaman içinde izleyebilirsiniz. Aşağıdaki örnek, başarılı isteklerin ve gelen isteklerin hesap düzeyinde nasıl görüntüleneceğini göstermektedir:

![][1]

Ölçümlere doğrudan ad alanı aracılığıyla da erişebilirsiniz. Bunu yapmak için ad alanınızı seçin ve ardından **ölçümler**' e tıklayın. Varlığın kapsamına filtrelenen ölçümleri göstermek için varlığı seçin ve ardından **ölçümler**' e tıklayın.

![][2]

Boyutları destekleyen ölçümler için, istenen boyut değeri ile filtrelemeniz gerekir.

## <a name="billing"></a>Faturalandırma

Azure Izleyici 'de ölçümler ve uyarılar her uyarı temelinde ücretlendirilir. Bu ücretler, uyarı kurulum ve kaydedilmeden önce portalda kullanılabilir olmalıdır. 

Ölçüm verilerini alan ek çözümler, bu çözümler tarafından doğrudan faturalandırılır. Örneğin, ölçüm verilerini bir Azure depolama hesabına arşivlerseniz Azure Storage tarafından faturalandırılırsınız. Ayrıca, gelişmiş analizler için ölçüm verilerini Log Analytics akışı halinde Log Analytics olarak faturalandırılırsınız.

Aşağıdaki ölçümler, hizmetinizin sistem durumuna ilişkin bir genel bakış sağlar. 

> [!NOTE]
> Farklı bir ad altında taşındığı için birkaç ölçümü kullanımdan kaldıracağız. Bu işlem, başvurularınızı güncelleştirmenizi gerektirebilir. "Kullanım dışı" anahtar sözcüğüyle işaretlenen ölçümler bundan sonra desteklenmez.

Tüm ölçüm değerleri her dakika Azure Izleyici 'ye gönderilir. Zaman ayrıntı düzeyi, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm Service Bus ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="request-metrics"></a>İstek ölçümleri

Veri ve yönetim işlemleri isteklerinin sayısını sayar.

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
| Gelen İstekler| Belirli bir süre içinde Service Bus hizmetine yapılan istek sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Başarılı İstekler|Belirli bir süre içinde Service Bus hizmetine yapılan başarılı istek sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Sunucu hataları|Belirli bir süre içinde Service Bus hizmetindeki bir hata nedeniyle işlenmemiş istek sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Kullanıcı hataları (aşağıdaki alt bölüme bakın)|Belirtilen bir süre içinde Kullanıcı hataları nedeniyle işlenmeyen isteklerin sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Kısıtlanmış Istekler|Kullanım aşıldığı için kısıtlanan isteklerin sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

### <a name="user-errors"></a>Kullanıcı hataları

Aşağıdaki iki tür hata, Kullanıcı hataları olarak sınıflandırılmaktadır:

1. İstemci tarafı hataları (HTTP olarak 400 hata olacaktır).
2. [Mesajgelocklostexception](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception)gibi iletileri işlerken oluşan hatalar.


## <a name="message-metrics"></a>İleti ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Gelen İletiler|Belirli bir süre boyunca Service Bus gönderilen olay veya ileti sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Giden İletiler|Belirli bir süre içinde Service Bus alınan olay veya ileti sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
| İletiler| Kuyruktaki/konudaki iletilerin sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/> Boyut: EntityName |
| ActiveMessages| Bir kuyruktaki/konudaki etkin ileti sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/> Boyut: EntityName |
| Kullanılmayan iletiler| Bir kuyruktaki/konudaki kullanılmayan iletilerin sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama <br/>Boyut: EntityName |
| Zamanlanan iletiler| Bir kuyruktaki/konudaki zamanlanan ileti sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Ortalama  <br/> Boyut: EntityName |

> [!NOTE]
> Aşağıdaki ölçümler için değerler, zaman içinde değerlerdir. Söz konusu noktadan hemen sonra tüketilen gelen iletiler bu ölçümlere yansıtılmayabilir. 
> - İletiler
> - Etkin iletiler 
> - Kullanılmayan iletiler 
> - Zamanlanan iletiler 

## <a name="connection-metrics"></a>Bağlantı ölçümleri

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|ActiveConnections|Bir ad alanındaki etkin bağlantıların yanı sıra bir varlık.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Açılan bağlantılar |Açık bağlantı sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Kapalı bağlantılar |Kapalı bağlantı sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

## <a name="resource-usage-metrics"></a>Kaynak kullanım ölçümleri

> [!NOTE] 
> Aşağıdaki ölçümler yalnızca **Premium** katmanıyla birlikte kullanılabilir. 
> 
> Premium katman ad alanı için kesintileri izlemek için önemli ölçümler: ad **alanı başına CPU kullanımı** ve **ad alanı başına bellek boyutu**. Azure Izleyici 'yi kullanarak bu ölçümler için [Uyarılar ayarlayın](../azure-monitor/platform/alerts-metric.md) .
> 
> İzleyebilmeniz için kullanabileceğiniz diğer ölçüm: **Kısıtlanmış istekleriniz**. Ad alanı bellek, CPU ve aracılı bağlantı sınırları içinde kaldığında bir sorun olması gerekmez. Daha fazla bilgi için bkz. [Azure Service Bus Premium katmanında daraltma](service-bus-throttling.md#throttling-in-azure-service-bus-premium-tier)

| Ölçüm Adı | Açıklama |
| ------------------- | ----------------- |
|Ad alanı başına CPU kullanımı|Ad alanının CPU kullanım yüzdesi.<br/><br/> Birim: yüzde <br/> Toplama türü: en fazla <br/> Boyut: EntityName|
|Ad alanı başına bellek boyutu kullanımı|Ad alanının bellek kullanım yüzdesi.<br/><br/> Birim: yüzde <br/> Toplama türü: en fazla <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçüm boyutları

Azure Service Bus, Azure Izleyici 'de ölçümler için aşağıdaki boyutları destekler. Ölçümlere boyutlar eklemek isteğe bağlıdır. Boyut eklemeyin, ölçümler ad alanı düzeyinde belirtilir. 

|Boyut adı|Açıklama|
| ------------------- | ----------------- |
|EntityName| Service Bus ad alanı altındaki mesajlaşma varlıklarını destekler.|

## <a name="set-up-alerts-on-metrics"></a>Ölçümler üzerinde uyarıları ayarlama

1. **Service Bus ad alanı** sayfasının **ölçümler** sekmesinde **uyarıları Yapılandır**' ı seçin. 

    ![Ölçümler sayfası-uyarıları yapılandırma menüsü](./media/service-bus-metrics-azure-monitor/metrics-page-configure-alerts-menu.png)
2. **Hedef Seç** seçeneğini belirleyin ve **Kaynak Seç** sayfasında aşağıdaki işlemleri yapın: 
    1. **Kaynak türüne göre filtrele** alanı Için **Service Bus ad alanlarını** seçin. 
    2. **Aboneliğe göre filtrele** alanı için aboneliğinizi seçin.
    3. Listeden **Service Bus ad alanını** seçin. 
    4. **Done** (Bitti) öğesini seçin. 
    
        ![Ad alanı seçin](./media/service-bus-metrics-azure-monitor/select-namespace.png)
1. **Ölçüt Ekle**' yi seçin ve **sinyal mantığını Yapılandır** sayfasında aşağıdaki işlemleri yapın:
    1. **Sinyal türü**için **ölçümleri** seçin. 
    2. Bir sinyal seçin. Örneğin: **hizmet hataları**. 

        ![Sunucu hatalarını seçin](./media/service-bus-metrics-azure-monitor/select-server-errors.png)
    1. Condition **değerinden büyük** ' **Condition**ı seçin.
    2. **Zaman toplama**için **Toplam** ' u seçin. 
    3. **Eşik**için **5** girin. 
    4. **Done** (Bitti) öğesini seçin.    

        ![Koşul Belirt](./media/service-bus-metrics-azure-monitor/specify-condition.png)    
1. **Kural oluştur** sayfasında, **uyarı ayrıntılarını tanımla**' yı genişletin ve aşağıdaki eylemleri yapın:
    1. Uyarı için bir **ad** girin. 
    2. Uyarı için bir **Açıklama** girin.
    3. Uyarı için **önem derecesi** ' ni seçin. 

        ![Uyarı ayrıntıları](./media/service-bus-metrics-azure-monitor/alert-details.png)
1. **Kural oluştur** sayfasında, **eylem grubu tanımla**' yı genişletin, **yeni eylem grubu**' nu seçin ve **eylem grubu Ekle sayfasında**aşağıdaki işlemleri yapın. 
    1. Eylem grubu için bir ad girin.
    2. Eylem grubu için kısa bir ad girin. 
    3. Aboneliğinizi seçin. 
    4. Kaynak grubunu seçin. 
    5. Bu izlenecek yol için **eylem adı**için **e-posta gönder** ' i girin.
    6. **Eylem türü**Için **e-posta/SMS/Push/Voice** ' ı seçin. 
    7. **Ayrıntıları düzenle** seçeneğini belirleyin. 
    8. **E-posta/SMS/Push/Voice** sayfasında aşağıdaki işlemleri yapın:
        1. **E-posta**seçeneğini belirleyin. 
        2. **E-posta adresini**yazın. 
        3. **Tamam**’ı seçin.

            ![Uyarı ayrıntıları](./media/service-bus-metrics-azure-monitor/add-action-group.png)
        4. **Eylem grubu Ekle** sayfasında **Tamam**' ı seçin. 
1. **Kural oluştur** sayfasında, **Uyarı kuralı oluştur**' u seçin. 

    ![Uyarı kuralı oluştur düğmesi](./media/service-bus-metrics-azure-monitor/create-alert-rule.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [Azure izleyiciye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


