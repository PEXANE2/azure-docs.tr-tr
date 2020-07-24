---
title: Azure Işlevlerinde çalışan uygulamaları Application Insights Azure Izleyici ile izleme | Microsoft Docs
description: Azure Izleyici, Azure Işlevleri üzerinde çalışan uygulamanızla sorunsuz bir şekilde tümleşir ve performansı izlemenize ve uygulamalarınızın sorunlarını hiç zaman fark etmenize olanak tanır.
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 06/26/2020
ms.openlocfilehash: 6d790a6f02fdc07609e374639c6e452b9088262e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87024580"
---
# <a name="monitoring-azure-functions-with-azure-monitor-application-insights"></a>Azure Izleme Application Insights Azure Işlevleri 'Ni izleme

[Azure işlevleri](../../azure-functions/functions-overview.md) , işlevleri Izlemek için Azure Application Insights ile yerleşik tümleştirme sunar. 

Application Insights günlük, performans ve hata verilerini toplar ve performans bozuklularını otomatik olarak algılar. Application Insights sorunları tanılamanıza ve işlevlerinizin nasıl kullanıldığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Uygulama verilerinize ilişkin görünürlük varsa, performansı ve kullanılabilirliği sürekli olarak geliştirebilirsiniz. Application Insights, yerel işlev uygulama projesi geliştirme sırasında bile kullanabilirsiniz. 

Gerekli Application Insights araçları Azure Işlevleri 'nde yerleşik olarak bulunur. Tek yapmanız gereken tek şey, işlev uygulamanızı bir Application Insights kaynağına bağlamak için geçerli bir izleme anahtarıdır. İşlev uygulaması kaynağınız Azure 'da oluşturulduğunda, izleme anahtarı uygulama ayarlarınıza eklenmelidir. İşlev uygulamanız zaten bu anahtara sahip değilse, el ile ayarlayabilirsiniz. Daha fazla bilgi için [Azure işlevlerini izleme](../../azure-functions/functions-monitoring.md?tabs=cmd)hakkında daha fazla bilgi edinin.

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

* İzleme [Azure işlevlerini](../../azure-functions/functions-monitoring.md) izleme hakkında daha fazla yönerge ve bilgi okuyun
* [Dağıtılmış izlemeye](./distributed-tracing.md) genel bakış alın
* İşletmeniz için [uygulama Haritalarınızın](./app-map.md?tabs=net) neler yapabileceğini görün
* [Java uygulamaları için İstekler ve bağımlılıklar](./java-in-process-agent.md) hakkında bilgi edinin
* [Azure izleyici](../overview.md) ve [Application Insights](./app-insights-overview.md) hakkında daha fazla bilgi edinin
