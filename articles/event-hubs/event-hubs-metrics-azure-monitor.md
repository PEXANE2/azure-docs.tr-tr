---
title: Azure Monitöründe Ölçümler - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, Azure Olay Hub'larını izlemek için Azure İzleme'nin nasıl kullanılacağı hakkında bilgi
services: event-hubs
documentationcenter: .NET
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 09/18/2019
ms.author: shvija
ms.openlocfilehash: 96c346f4359740fda5638dfdbe5735c5bdfce8c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162659"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor"></a>Azure İzleyici'deki Azure Event Hubs ölçümleri

Etkinlik Hub'ları ölçümleri, Azure aboneliğinizde Etkinlik Hub'ları kaynaklarının durumunu sağlar. Zengin bir ölçüm veri kümesiyle, etkinlik hub'larınızın genel durumunu yalnızca ad alanı düzeyinde değil, varlık düzeyinde de değerlendirebilirsiniz. Bu istatistikler, olay hub'larınızın durumunu izlemenize yardımcı olduğu için önemli olabilir. Ölçümler, Azure desteğine başvurmaya gerek kalmadan kök neden sorunlarının giderilmesine de yardımcı olabilir.

Azure Monitor, çeşitli Azure hizmetlerinde izleme için birleşik kullanıcı arabirimleri sağlar. Daha fazla bilgi için Microsoft [Azure'da İzleme](../monitoring-and-diagnostics/monitoring-overview.md) ve GitHub'da .NET örneği [içeren Azure Monitörünü Al ölçütlerine](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) bakın.

## <a name="access-metrics"></a>Erişim ölçümleri

Azure Monitor ölçümlere erişmek için birden çok yol sağlar. Azure [portalı](https://portal.azure.com)üzerinden ölçümlere erişebilir veya Azure Monitör API'lerini (REST ve .NET) ve Log Analytics ve Olay Hub'ları gibi analiz çözümlerini kullanabilirsiniz. Daha fazla bilgi için azure [monitor tarafından toplanan verileri izleme](../azure-monitor/platform/data-platform.md)'ye bakın.

Ölçümler varsayılan olarak etkinleştirilir ve en son 30 günlük verilere erişebilirsiniz. Verileri daha uzun süre saklamanız gerekiyorsa, metrik verileri bir Azure Depolama hesabına arşivleyebilirsiniz. Bu, Azure Monitor'daki [tanılama ayarlarında](../azure-monitor/platform/diagnostic-settings.md) yapılandırılır.


## <a name="access-metrics-in-the-portal"></a>Portaldaki erişim ölçümleri

[Azure portalında](https://portal.azure.com)ölçümleri zaman içinde izleyebilirsiniz. Aşağıdaki örnekte, hesap düzeyinde başarılı isteklerin ve gelen isteklerin nasıl görüntülenenedöneceği gösterilmektedir:

![Başarılı ölçümleri görüntüleme][1]

Ölçümlere doğrudan ad alanı üzerinden de erişebilirsiniz. Bunu yapmak için, ad alanınızı seçin ve ardından **Ölçümler'i**tıklatın. Olay merkezinin kapsamına filtre edilmiş ölçümleri görüntülemek için olay merkezini seçin ve ardından **Ölçümler'i**tıklatın.

Boyutları destekleyen ölçümler için, aşağıdaki örnekte gösterildiği gibi istenen boyut değerine filtre uygulamanız gerekir:

![Boyut değeri olan filtreleme][2]

## <a name="billing"></a>Faturalandırma

Azure Monitor'da ölçümleri kullanmak şu anda ücretsizdir. Ancak, ölçümler verilerini sindiren ek çözümler kullanırsanız, bu çözümler tarafından faturalandırılabilir. Örneğin, metrik verileri bir Azure Depolama hesabına arşivlerseniz Azure Depolama tarafından faturalandırılırsınız. Gelişmiş analizler için azure monitor günlüklerine metrik verileri aktarıyorsanız, Azure tarafından faturalandırılırsınız.

Aşağıdaki ölçümler, hizmetinizin durumu hakkında genel bir bakış sağlar. 

> [!NOTE]
> Farklı bir ad altında taşındıkları için birkaç ölçümü küçümsüyoruz. Bu, başvurularınızı güncelleştirmenizi gerektirebilir. "Amortismana" anahtar kelimesi ile işaretlenmiş ölçümler ileriye dönük olarak desteklenmeyen.

Tüm ölçüm değerleri her dakika Azure Monitor'a gönderilir. Zaman tanecikleri, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm Olay Hub'ları ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="request-metrics"></a>İstek ölçümleri

Veri ve yönetim işlemleri isteklerinin sayısını sayar.

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
| Gelen İstekler  | Belirli bir dönemde Azure Etkinlik Hub'ları hizmetine yapılan istek sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName |
| Başarılı İstekler    | Belirli bir dönemde Azure Etkinlik Hub'ları hizmetine yapılan başarılı istek sayısı. <br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName |
| Sunucu Hataları  | Azure Etkinlik Hub'ları hizmetinde belirli bir süre içinde yapılan bir hata nedeniyle işlenmemiş istek sayısı. <br/><br/>Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName |
|Kullanıcı Hataları |Belirli bir dönemdeki kullanıcı hataları nedeniyle işlenmemiş istek sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Kota Aşıldı Hatalar |İstek sayısı kullanılabilir kotayı aştı. Olay Hub kotaları hakkında daha fazla bilgi için [bu makaleye](event-hubs-quotas.md) bakın.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|

## <a name="throughput-metrics"></a>Elde iş ölçümleri

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
|Daraltılmış İstekler |İşleme birimi kullanımı aşıldığı için daraltılmış olan istek sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|

## <a name="message-metrics"></a>İleti ölçümleri

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
|Gelen Mesajlar |Belirli bir dönemde Olay Hub'larına gönderilen olay veya ileti sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Giden Mesajlar |Belirli bir dönemde Olay Hub'larından alınan olay veya ileti sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Gelen Baytlar |Belirli bir süre içinde Azure Etkinlik Hub'ları hizmetine gönderilen bayt sayısı.<br/><br/> Birim: Bayt <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Giden Baytlar |Belirli bir dönemde Azure Etkinlik Hub'ları hizmetinden alınan bayt sayısı.<br/><br/> Birim: Bayt <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|

## <a name="connection-metrics"></a>Bağlantı ölçümleri

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
|Aktif Bağlantılar |Bir ad alanının yanı sıra bir varlıktaki etkin bağlantı sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Açılan Bağlantılar |Açık bağlantıların sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Bağlantılar Kapalı |Kapalı bağlantı sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|

## <a name="event-hubs-capture-metrics"></a>Olay Hub'ları Yakalama ölçümleri

Etkinlik hub'larınız için Yakalama özelliğini etkinleştirdiğinizde Olay Hub'ları Yakalama ölçümlerini izleyebilirsiniz. Aşağıdaki ölçümler, Capture etkinken neler izleyebilirsiniz açıklar.

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
|Biriktirme Listesi'ni yakalama |Seçilen hedefe henüz yakalanmamış bayt sayısı.<br/><br/> Birim: Bayt <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Yakalanan İletiler |Belirli bir süre içinde seçilen hedefe yakalanan ileti lerin veya olayların sayısı.<br/><br/> Birim: Sayma <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|
|Yakalanan Baytlar |Belirli bir süre içinde seçilen hedefe yakalanan bayt sayısı.<br/><br/> Birim: Bayt <br/> Toplama Türü: Toplam <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Metrik boyutları

Azure Etkinlik Hub'ları, Azure Monitor'daki ölçümler için aşağıdaki boyutları destekler. Ölçümlerinize boyut ekleme isteğe bağlıdır. Boyutlar eklemezseniz, ölçümler ad alanı düzeyinde belirtilir. 

| Metrik Adı | Açıklama |
| ------------------- | ----------------- |
|Varlıkadı| Olay Hub'ları ad alanı altında olay hub varlıklarını destekler.|

## <a name="azure-monitor-integration-with-siem-tools"></a>SIEM araçlarıyla Azure Monitör entegrasyonu
İzleme verilerinizi (etkinlik günlükleri, tanılama günlükleri, vb.) Azure Monitor ile bir etkinlik merkezine yönlendirmeniz, Güvenlik Bilgileri ve Etkinlik Yönetimi (SIEM) araçlarıyla kolayca tümleştirmenizi sağlar. Daha fazla bilgi için aşağıdaki makalelere/blog gönderilerine bakın:

- [Azure izleme verilerini harici bir araç tarafından tüketim için bir olay hub'ına aktarın](../azure-monitor/platform/stream-monitoring-data-event-hubs.md)
- [Azure Günlük Tümleştirmesi’ne giriş](../security/fundamentals/azure-log-integration-overview.md)
- [SIEM araçlarıyla tümleştirmek için Azure İzleyici'yi kullanma](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

Bir SIEM aracının bir olay merkezinden günlük verilerini tükettiği senaryoda, gelen iletiler görmüyorsanız veya gelen iletileri ancak ölçümler grafiğinde giden iletiler görmüyorsanız aşağıdaki adımları izleyin:

- **Gelen ileti yoksa,** Bu, Azure Monitor hizmetinin denetim/tanılama günlüklerini olay hub'ına taşımadığı anlamına gelir. Bu senaryoda Azure Monitor ekibiyle bir destek bileti açın. 
- gelen iletiler varsa, ancak **giden ileti yoksa,** bu SIEM uygulamasının iletileri okumadığı anlamına gelir. Bu uygulamaların olay merkezinin yapılandırmasının doğru olup olmadığını belirlemek için SIEM sağlayıcısına başvurun.


## <a name="next-steps"></a>Sonraki adımlar

* Azure [İzleme genel görünümüne](../monitoring-and-diagnostics/monitoring-overview.md)bakın.
* GitHub'da [.NET örneğiyle Azure Monitör ölçümleri alın.](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) 

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

- Etkinlik Hub'ları öğreticisiyle başlayın
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [Javascript](get-started-java-send-v2.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png



