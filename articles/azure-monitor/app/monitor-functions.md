---
title: Azure Işlevlerinde çalışan uygulamaları Application Insights Azure Izleyici ile izleme | Microsoft Docs
description: Azure Izleyici, Azure Işlevleri üzerinde çalışan uygulamanızla sorunsuz bir şekilde tümleşir ve performansı izlemenize ve uygulamalarınızın sorunlarını hiç zaman fark etmenize olanak tanır.
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 06/26/2020
ms.openlocfilehash: 093448ad0280ada039f1d4e5abd0e83e4be19b03
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482114"
---
# <a name="monitoring-azure-functions-with-azure-monitor-application-insights"></a>Azure Izleme Application Insights Azure Işlevleri 'Ni izleme

[Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) , işlevleri Izlemek için Azure Application Insights ile yerleşik tümleştirme sunar. 

Application Insights günlük, performans ve hata verilerini toplar ve performans bozuklularını otomatik olarak algılar. Application Insights sorunları tanılamanıza ve işlevlerinizin nasıl kullanıldığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Uygulama verilerinize ilişkin görünürlük varsa, performansı ve kullanılabilirliği sürekli olarak geliştirebilirsiniz. Application Insights, yerel işlev uygulama projesi geliştirme sırasında bile kullanabilirsiniz. 

Gerekli Application Insights araçları Azure Işlevleri 'nde yerleşik olarak bulunur. Tek yapmanız gereken tek şey, işlev uygulamanızı bir Application Insights kaynağına bağlamak için geçerli bir izleme anahtarıdır. İşlev uygulaması kaynağınız Azure 'da oluşturulduğunda, izleme anahtarı uygulama ayarlarınıza eklenmelidir. İşlev uygulamanız zaten bu anahtara sahip değilse, el ile ayarlayabilirsiniz. Daha fazla bilgi için [Azure işlevlerini izleme](https://docs.microsoft.com/azure/azure-functions/functions-monitoring?tabs=cmd)hakkında daha fazla bilgi edinin.

## <a name="distributed-tracing-for-java-applications-on-windows-public-preview"></a>Windows 'da Java uygulamaları için dağıtılmış izleme (Genel Önizleme)

> [!IMPORTANT]
> Bu özellik şu anda Windows 'daki Java Azure Işlevleri için genel önizleme aşamasındadır ve Linux 'ta Java Azure Işlevleri için dağıtılmış izleme desteklenmez. Tüketim planı için 8-9 saniyelik soğuk bir başlangıç süresi vardır.

Uygulamalarınız Java 'da yazılmışsa,, istekler, bağımlılıklar, Günlükler ve ölçümler dahil olmak üzere işlevlerinizin daha zengin verilerini görüntüleyebilirsiniz. Ek veriler ayrıca, uçtan uca işlemleri görmenizi ve tanılamanıza ve sistemin nasıl etkileşime gireceğini ve ortalama performans ve hata tarifelerinin ne olduğunu gösteren bir topik görünümünü göstermek için çok sayıda işlem toplayan uygulama haritasını görmenizi sağlar.

Uçtan uca Tanılamalar ve uygulama eşlemesi tek bir işlem/istek için görünürlük sağlar. Bu iki özellik birlikte, güvenilirlik sorunlarının kök nedenini ve her istek için performans sorunlarını bulmada çok yararlı olur.

### <a name="how-to-enable-distributed-tracing-for-java-function-apps"></a>Java Işlev uygulamaları için dağıtılmış izleme nasıl etkinleştirilir?

İşlevler uygulamasına genel bakış dikey penceresine gidin, Konfigürasyonlar ' a gidin. Uygulama ayarları altında "+ yeni uygulama ayarı" na tıklayın. Aşağıdaki değerlere sahip aşağıdaki iki uygulama ayarını ekleyin ve sol üstteki Kaydet ' e tıklayın. YAPıLDıĞıNı!

```
XDT_MicrosoftApplicationInsights_Java -> 1
ApplicationInsightsAgent_EXTENSION_VERSION -> ~2
```

## <a name="next-steps"></a>Sonraki Adımlar

* İzleme [Azure işlevlerini](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) izleme hakkında daha fazla yönerge ve bilgi okuyun
* [Dağıtılmış izlemeye](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing) genel bakış alın
* İşletmeniz için [uygulama Haritalarınızın](https://docs.microsoft.com/azure/azure-monitor/app/app-map?tabs=net) neler yapabileceğini görün
* [Java uygulamaları için İstekler ve bağımlılıklar](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent) hakkında bilgi edinin
* [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) ve [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) hakkında daha fazla bilgi edinin