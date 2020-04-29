---
title: Azure Izleyici 'de ölçümler-Azure Event Hubs | Microsoft Docs
description: Bu makalede Azure Event Hubs izlemek için Azure Izleme 'nin nasıl kullanılacağı hakkında bilgi verilmektedir.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77162659"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor"></a>Azure İzleyici'deki Azure Event Hubs ölçümleri

Event Hubs ölçümler, Azure aboneliğinizdeki Event Hubs kaynaklarının durumunu sağlar. Zengin bir ölçüm verileri kümesiyle, Olay Hub 'larınızın genel durumunu yalnızca ad alanı düzeyinde değil, varlık düzeyinde değerlendirebilirsiniz. Bu istatistikler, Olay Hub 'larınızın durumunu izlemenize yardımcı olduğu için önemli olabilir. Ölçümler Ayrıca, Azure desteği ile iletişim kurmaya gerek kalmadan, kök neden sorunları gidermenize yardımcı olabilir.

Azure Izleyici, çeşitli Azure hizmetleri genelinde izleme için birleştirilmiş kullanıcı arabirimleri sağlar. Daha fazla bilgi için bkz. [Microsoft Azure 'de izleme](../monitoring-and-diagnostics/monitoring-overview.md) ve GitHub 'da [.net örneği ile Azure izleyici ölçümlerini alma](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) .

## <a name="access-metrics"></a>Erişim ölçümleri

Azure Izleyici, ölçümlere erişmek için birden çok yol sağlar. Ölçümlere [Azure Portal](https://portal.azure.com)aracılığıyla erişebilir veya Azure Izleyici API 'LERINI (REST ve .net) ve Log Analytics ve Event Hubs gibi çözümleme çözümlerini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici tarafından toplanan verileri izleme](../azure-monitor/platform/data-platform.md).

Ölçümler varsayılan olarak etkinleştirilmiştir ve en son 30 güne kadar verilere erişebilirsiniz. Verileri daha uzun bir süre tutmanız gerekiyorsa, ölçüm verilerini bir Azure depolama hesabına arşivleyebilirsiniz. Bu, Azure Izleyici 'de [Tanılama ayarları](../azure-monitor/platform/diagnostic-settings.md) 'nda yapılandırılır.


## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümleri

[Azure Portal](https://portal.azure.com)ölçümleri zaman içinde izleyebilirsiniz. Aşağıdaki örnek, başarılı isteklerin ve gelen isteklerin hesap düzeyinde nasıl görüntüleneceğini göstermektedir:

![Başarılı ölçümleri görüntüleme][1]

Ölçümlere doğrudan ad alanı aracılığıyla da erişebilirsiniz. Bunu yapmak için ad alanınızı seçin ve ardından **ölçümler**' e tıklayın. Olay Hub 'ının kapsamına filtrelenen ölçümleri göstermek için Olay Hub 'ını seçin ve ardından **ölçümler**' e tıklayın.

Boyutları destekleyen ölçümler için, aşağıdaki örnekte gösterildiği gibi istenen boyut değeriyle filtrelemeniz gerekir:

![Boyut değeriyle filtrele][2]

## <a name="billing"></a>Faturalandırma

Azure Izleyici 'de ölçümlerin kullanılması şu anda ücretsizdir. Ancak, ölçüm verilerini alan ek çözümler kullanıyorsanız, bu çözümler tarafından faturalandırılabiliriz. Örneğin, ölçüm verilerini bir Azure depolama hesabına arşivlerseniz Azure Storage tarafından faturalandırılırsınız. Ayrıca, ölçüm verilerini gelişmiş analizler için Azure Izleyici günlüklerine akıdıysanız Azure tarafından da faturalandırılırsınız.

Aşağıdaki ölçümler, hizmetinizin sistem durumuna ilişkin bir genel bakış sağlar. 

> [!NOTE]
> Farklı bir ad altında taşındığı için birkaç ölçümü kullanımdan kaldıracağız. Bu işlem, başvurularınızı güncelleştirmenizi gerektirebilir. "Kullanım dışı" anahtar sözcüğüyle işaretlenen ölçümler bundan sonra desteklenmez.

Tüm ölçüm değerleri her dakika Azure Izleyici 'ye gönderilir. Zaman ayrıntı düzeyi, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm Event Hubs ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="request-metrics"></a>İstek ölçümleri

Veri ve yönetim işlemleri isteklerinin sayısını sayar.

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
| Gelen Istekler  | Belirli bir süre boyunca Azure Event Hubs hizmetine yapılan isteklerin sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName |
| Başarılı Istekler    | Azure Event Hubs hizmetine belirli bir süre boyunca yapılan başarılı istek sayısı. <br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName |
| Sunucu hataları  | Belirli bir süre boyunca Azure Event Hubs hizmetindeki bir hata nedeniyle işlenmemiş istek sayısı. <br/><br/>Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName |
|Kullanıcı hataları |Belirtilen bir süre içinde Kullanıcı hataları nedeniyle işlenmeyen isteklerin sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Kotanın sayısı aşıldı |İstek sayısı, kullanılabilir kotayı aştı. Event Hubs kotaları hakkında daha fazla bilgi için [Bu makaleye](event-hubs-quotas.md) bakın.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

## <a name="throughput-metrics"></a>Verimlilik ölçümleri

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
|Kısıtlanmış Istekler |İşleme birimi kullanımı aşıldığı için daraltılmış olan istek sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

## <a name="message-metrics"></a>İleti ölçümleri

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
|Gelen Iletiler |Belirli bir süre boyunca Event Hubs gönderilen olay veya ileti sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Giden Iletiler |Belirli bir süre içinde Event Hubs alınan olay veya ileti sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Gelen bayt sayısı |Belirli bir süre boyunca Azure Event Hubs hizmetine gönderilen bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Giden bayt sayısı |Belirli bir süre içinde Azure Event Hubs hizmetinden alınan baytların sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

## <a name="connection-metrics"></a>Bağlantı ölçümleri

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
|ActiveConnection sayısı |Bir ad alanındaki etkin bağlantıların yanı sıra bir varlık.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Açılan bağlantılar |Açık bağlantı sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Kapalı bağlantılar |Kapalı bağlantı sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

## <a name="event-hubs-capture-metrics"></a>Event Hubs yakalama ölçümleri

Olay Hub 'larınız için yakalama özelliğini etkinleştirdiğinizde, Event Hubs yakalama ölçümlerini izleyebilirsiniz. Aşağıdaki ölçümler yakalama etkinken neleri izleyebileceğinize ilişkin öğeleri anlatmaktadır.

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
|Biriktirme listesi |Seçilen hedefe henüz yakalanamayan bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Yakalanan Iletiler |Belirli bir süre içinde seçilen hedefe yakalanan ileti veya olay sayısı.<br/><br/> Birim: sayım <br/> Toplama türü: Toplam <br/> Boyut: EntityName|
|Yakalanan baytlar |Seçilen hedefe belirtilen bir süre içinde yakalanan bayt sayısı.<br/><br/> Birim: bayt <br/> Toplama türü: Toplam <br/> Boyut: EntityName|

## <a name="metrics-dimensions"></a>Ölçüm boyutları

Azure Event Hubs, Azure Izleyici 'de ölçümler için aşağıdaki boyutları destekler. Ölçümlere boyutlar eklemek isteğe bağlıdır. Boyut eklemeyin, ölçümler ad alanı düzeyinde belirtilir. 

| Ölçüm adı | Açıklama |
| ------------------- | ----------------- |
|EntityName| Event Hubs ad alanı altında Olay Hub 'ı varlıklarını destekler.|

## <a name="azure-monitor-integration-with-siem-tools"></a>SıEM araçlarıyla Azure Izleyici tümleştirmesi
İzleme verilerinizi (etkinlik günlükleri, tanılama günlükleri vb.) Azure Izleyici ile bir olay hub 'ına yönlendirmek, güvenlik bilgileri ve olay yönetimi (SıEM) araçlarıyla kolayca tümleştirmenize olanak sağlar. Daha fazla bilgi için aşağıdaki makalelere/blog postalarına bakın:

- [Dış bir araçla tüketim için Azure izleme verilerini bir olay hub 'ına akış](../azure-monitor/platform/stream-monitoring-data-event-hubs.md)
- [Azure Günlük Tümleştirmesi’ne giriş](../security/fundamentals/azure-log-integration-overview.md)
- [SIEM araçlarıyla tümleştirmek için Azure İzleyici'yi kullanma](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

Bir SıEM aracının bir olay hub 'ından günlük verilerini tükettiği senaryoda, hiçbir gelen ileti görmüyorsanız veya ölçüm grafiğinde giden iletiler görmüyorsanız, şu adımları izleyin:

- **Gelen ileti**yoksa, Azure izleyici hizmetinin denetim/tanılama günlüklerini Olay Hub 'ına taşımayacağı anlamına gelir. Bu senaryoda Azure Izleyici ekibi ile bir destek bileti açın. 
- gelen iletiler varsa, ancak **giden iletiler yoksa**SIEM uygulamasının iletileri okumamasından dolayı olur. Olay Hub 'ının yapılandırmasının doğru olup olmadığını öğrenmek için SıEM sağlayıcısına başvurun.


## <a name="next-steps"></a>Sonraki adımlar

* Bkz. [Azure izlemeye genel bakış](../monitoring-and-diagnostics/monitoring-overview.md).
* GitHub 'da [.net örneği Ile Azure izleyici ölçümlerini alın](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) . 

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

- Event Hubs öğreticisi ile çalışmaya başlama
    - [.NET Core](get-started-dotnet-standard-send-v2.md)
    - [Java](get-started-java-send-v2.md)
    - [Python](get-started-python-send-v2.md)
    - [JavaScript](get-started-java-send-v2.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png



