---
title: Azure Izleyici 'de ölçümler-Azure Event Hubs | Microsoft Docs
description: Bu makalede Azure Event Hubs izlemek için Azure Izleme 'nin nasıl kullanılacağı hakkında bilgi verilmektedir.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 5b055c02783c40d844d1c6306bbb71cb23d602f2
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98118804"
---
# <a name="azure-event-hubs-metrics-in-azure-monitor"></a>Azure İzleyici'deki Azure Event Hubs ölçümleri

Event Hubs ölçümler, Azure aboneliğinizdeki Event Hubs kaynaklarının durumunu sağlar. Zengin bir ölçüm verileri kümesiyle, Olay Hub 'larınızın genel durumunu yalnızca ad alanı düzeyinde değil, varlık düzeyinde değerlendirebilirsiniz. Bu istatistikler, Olay Hub 'larınızın durumunu izlemenize yardımcı olduğu için önemli olabilir. Ölçümler Ayrıca, Azure desteği ile iletişim kurmaya gerek kalmadan, kök neden sorunları gidermenize yardımcı olabilir.

Azure Izleyici, çeşitli Azure hizmetleri genelinde izleme için birleştirilmiş kullanıcı arabirimleri sağlar. Daha fazla bilgi için bkz. [Microsoft Azure 'de izleme](../azure-monitor/overview.md) ve GitHub 'da [.net örneği ile Azure izleyici ölçümlerini alma](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) .

## <a name="access-metrics"></a>Erişim ölçümleri

Azure Izleyici, ölçümlere erişmek için birden çok yol sağlar. Ölçümlere [Azure Portal](https://portal.azure.com)aracılığıyla erişebilir veya Azure Izleyici API 'LERINI (REST ve .net) ve Log Analytics ve Event Hubs gibi çözümleme çözümlerini kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure izleyici tarafından toplanan verileri izleme](../azure-monitor/platform/data-platform.md).

Ölçümler varsayılan olarak etkinleştirilmiştir ve en son 30 güne kadar verilere erişebilirsiniz. Verileri daha uzun bir süre tutmanız gerekiyorsa, ölçüm verilerini bir Azure depolama hesabına arşivleyebilirsiniz. Bu, Azure Izleyici 'de [Tanılama ayarları](../azure-monitor/platform/diagnostic-settings.md) 'nda yapılandırılır.


## <a name="access-metrics-in-the-portal"></a>Portalda erişim ölçümleri

[Azure Portal](https://portal.azure.com)ölçümleri zaman içinde izleyebilirsiniz. Aşağıdaki örnek, başarılı isteklerin ve gelen isteklerin hesap düzeyinde nasıl görüntüleneceğini göstermektedir:

![Başarılı ölçümleri görüntüleme][1]

Ölçümlere doğrudan ad alanı aracılığıyla da erişebilirsiniz. Bunu yapmak için ad alanınızı seçin ve ardından **ölçümler**' e tıklayın. Olay Hub 'ının kapsamına filtrelenen ölçümleri göstermek için Olay Hub 'ını seçin ve ardından **ölçümler**' e tıklayın.

Boyutları destekleyen ölçümler için, aşağıdaki örnekte gösterildiği gibi istenen boyut değeriyle filtrelemeniz gerekir:

![Boyut değeriyle filtrele][2]

## <a name="billing"></a>Faturalama

Azure Izleyici 'de ölçümlerin kullanılması şu anda ücretsizdir. Ancak, ölçüm verilerini alan ek çözümler kullanıyorsanız, bu çözümler tarafından faturalandırılabiliriz. Örneğin, ölçüm verilerini bir Azure depolama hesabına arşivlerseniz Azure Storage tarafından faturalandırılırsınız. Ayrıca, ölçüm verilerini gelişmiş analizler için Azure Izleyici günlüklerine akıdıysanız Azure tarafından da faturalandırılırsınız.

Aşağıdaki ölçümler, hizmetinizin sistem durumuna ilişkin bir genel bakış sağlar. 

> [!NOTE]
> Farklı bir ad altında taşındığı için birkaç ölçümü kullanımdan kaldıracağız. Bu işlem, başvurularınızı güncelleştirmenizi gerektirebilir. "Kullanım dışı" anahtar sözcüğüyle işaretlenen ölçümler bundan sonra desteklenmez.

Tüm ölçüm değerleri her dakika Azure Izleyici 'ye gönderilir. Zaman ayrıntı düzeyi, ölçüm değerlerinin sunulduğu zaman aralığını tanımlar. Tüm Event Hubs ölçümleri için desteklenen zaman aralığı 1 dakikadır.

## <a name="azure-event-hubs-metrics"></a>Azure Event Hubs ölçümleri
Hizmet tarafından desteklenen ölçümlerin bir listesi için bkz. [Azure Event Hubs](../azure-monitor/platform/metrics-supported.md#microsofteventhubnamespaces)

## <a name="azure-monitor-integration-with-siem-tools"></a>SıEM araçlarıyla Azure Izleyici tümleştirmesi
İzleme verilerinizi (etkinlik günlükleri, tanılama günlükleri vb.) Azure Izleyici ile bir olay hub 'ına yönlendirmek, güvenlik bilgileri ve olay yönetimi (SıEM) araçlarıyla kolayca tümleştirmenize olanak sağlar. Daha fazla bilgi için aşağıdaki makalelere/blog postalarına bakın:

- [Dış bir araçla tüketim için Azure izleme verilerini bir olay hub 'ına akış](../azure-monitor/platform/stream-monitoring-data-event-hubs.md)
- [Azure Günlük Tümleştirmesi’ne giriş](/previous-versions/azure/security/fundamentals/azure-log-integration-overview)
- [SIEM araçlarıyla tümleştirmek için Azure İzleyici'yi kullanma](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

Bir SıEM aracının bir olay hub 'ından günlük verilerini tükettiği senaryoda, hiçbir gelen ileti görmüyorsanız veya ölçüm grafiğinde giden iletiler görmüyorsanız, şu adımları izleyin:

- **Gelen ileti** yoksa, Azure izleyici hizmetinin denetim/tanılama günlüklerini Olay Hub 'ına taşımayacağı anlamına gelir. Bu senaryoda Azure Izleyici ekibi ile bir destek bileti açın. 
- gelen iletiler varsa, ancak **giden iletiler yoksa** SIEM uygulamasının iletileri okumamasından dolayı olur. Olay Hub 'ının yapılandırmasının doğru olup olmadığını öğrenmek için SıEM sağlayıcısına başvurun.


## <a name="next-steps"></a>Sonraki adımlar

* Bkz. [Azure izlemeye genel bakış](../azure-monitor/overview.md).
* GitHub 'da [.net örneği Ile Azure izleyici ölçümlerini alın](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) . 

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

- Event Hubs öğreticisi ile çalışmaya başlama
    - [.NET Core](event-hubs-dotnet-standard-getstarted-send.md)
    - [Java](event-hubs-java-get-started-send.md)
    - [Python](event-hubs-python-get-started-send.md)
    - [JavaScript](event-hubs-java-get-started-send.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor1.png
[2]: ./media/event-hubs-metrics-azure-monitor/event-hubs-monitor2.png
