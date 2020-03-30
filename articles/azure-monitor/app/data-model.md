---
title: Azure Uygulama Öngörüleri Telemetri Veri Modeli | Microsoft Dokümanlar
description: Uygulama Öngörüleri veri modeline genel bakış
services: application-insights
documentationcenter: .net
manager: carmonm
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/14/2019
ms.reviewer: sergkanz
ms.openlocfilehash: 94013a12e1cf48a8007fce2547c200d82a657b71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671891"
---
# <a name="application-insights-telemetry-data-model"></a>Uygulama Öngörüleri telemetri veri modeli

[Azure Application Insights,](../../azure-monitor/app/app-insights-overview.md) uygulamanızın performansını ve kullanımını analiz edebilmeniz için web uygulamanızdan Azure portalına telemetri gönderir. Telemetri modeli standartlaştırılmıştır, böylece platform ve dilden bağımsız izleme oluşturulabilir. 

Application Insights tarafından toplanan veriler bu tipik uygulama yürütme deseni modellerini modeller:

![Uygulama Öngörüleri Uygulama Modeli](./media/data-model/application-insights-data-model.png)

Uygulamanızın yürütülmesini izlemek için aşağıdaki telemetri türleri kullanılır. Aşağıdaki üç tür genellikle otomatik olarak Web uygulama çerçevesinden Application Insights SDK tarafından toplanır:

* [**İstek**](data-model-request-telemetry.md) - Uygulamanız tarafından alınan bir isteği günlüğe kaydetmek için oluşturulur. Örneğin, Application Insights web SDK, web uygulamanızın aldığı her HTTP isteği için otomatik olarak bir İstek telemetri öğesi oluşturur. 

    **İşlem,** isteği işleyen yürütme iş parçacıklarıdır. Ayrıca, bir web işinde "uyandırma" veya verileri düzenli olarak işleyen işlev gibi diğer işlem türlerini izlemek için [kod da yazabilirsiniz.](../../azure-monitor/app/api-custom-events-metrics.md#trackrequest)  Her operasyonun bir kimliği vardır. Uygulamanız isteği işlerken oluşturulan tüm telemetrileri [gruplandırmak](../../azure-monitor/app/correlation.md) için kullanılabilen bu kimlik. Her işlem başarılı veya başarısız olur ve bir süreye sahiptir.
* [**Özel Durum**](data-model-exception-telemetry.md) - Genellikle bir işlemin başarısız olmasını neden olan bir özel durum temsil eder.
* [**Bağımlılık**](data-model-dependency-telemetry.md) - Uygulamanızdan rest API veya SQL gibi harici bir hizmete veya depolama alanına yapılan bir çağrıyı temsil eder. ASP.NET'da, SQL'e bağımlılık `System.Data`çağrıları . HTTP uç noktalarına yapılan `System.Net`aramalar . 

Uygulama Öngörüleri özel telemetri için üç ek veri türü sağlar:

* [Trace](data-model-trace-telemetry.md) - doğrudan veya bir bağdaştırıcı aracılığıyla size tanıdık gelen bir enstrümantasyon `Log4Net` `System.Diagnostics`çerçevesi kullanarak tanılama günlüğü uygulamak için kullanılır, gibi veya .
* [Olay](data-model-event-telemetry.md) - genellikle hizmetile kullanıcı etkileşimi yakalamak için, kullanım desenleri analiz etmek için kullanılır.
* [Metrik](data-model-metric-telemetry.md) - periyodik skaler ölçümleri rapor etmek için kullanılır.

Her telemetri öğesi uygulama sürümü veya kullanıcı oturumu kimliği gibi [bağlam bilgilerini](data-model-context.md) tanımlayabilir. Bağlam, belirli senaryoların engelini ortadan açan güçlü bir şekilde yazılan alanlar kümesidir. Uygulama sürümü düzgün bir şekilde başolarak sorulduğunda, Uygulama Öngörüleri yeniden dağıtımla ilişkili uygulama davranışındaki yeni desenleri algılayabilir. Oturum kimliği, kesintinin veya kullanıcı üzerindeki bir sorunun etkisini hesaplamak için kullanılabilir. Belirli başarısız bağımlılık, hata izleme veya kritik özel durum için oturum kimliği değerlerinin farklı sayısının hesaplanması, bir etkiyi iyi anlama olanağı sağlar.

Application Insights telemetri modeli, telemetriyi bir parçası olduğu işlemle [ilişkilendirmek](../../azure-monitor/app/correlation.md) için bir yol tanımlar. Örneğin, bir istek bir SQL Veritabanı aramaları ve kaydedilmiş tanılama bilgileri yapabilir. İstek telemetrisine geri bağlayan bu telemetri öğeleriiçin korelasyon bağlamını ayarlayabilirsiniz.

## <a name="schema-improvements"></a>Şema iyileştirmeleri

Application Insights veri modeli, uygulama telemetrinizi modellemenin basit ve temel ama güçlü bir yoludur. Biz temel senaryoları desteklemek ve gelişmiş kullanım için şema genişletmek için izin modeli basit ve ince tutmak için çalışıyoruz.

Veri modelini veya şema sorunlarını ve önerilerini bildirmek için GitHub [ApplicationInsights-Home](https://github.com/Microsoft/ApplicationInsights-Home/issues) deposunu kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel telemetri yazın](../../azure-monitor/app/api-custom-events-metrics.md)
- [Telemetriyi nasıl genişletip filtreleyeceklerini](../../azure-monitor/app/api-filtering-sampling.md)öğrenin.
- Veri modeline dayalı telemetri miktarını en aza indirmek için [örnekleme](../../azure-monitor/app/sampling.md) kullanın.
- Application Insights tarafından desteklenen [platformlara](../../azure-monitor/app/platforms.md) göz atın.
