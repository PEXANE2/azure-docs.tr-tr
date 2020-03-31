---
title: Azure Hizmet Kumaş Uygulama Düzeyi İzleme
description: Azure Hizmet Dokusu kümelerini izlemek ve tanılamak için kullanılan uygulama ve hizmet düzeyi olayları ve günlükleri hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 97c3be391dfbee7301ea47bf7234a9549d373370
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464729"
---
# <a name="application-logging"></a>Uygulama günlüğüne kaydetme

Kodunuzu programlamak yalnızca kullanıcılarınız hakkında bilgi edinmenin değil, aynı zamanda uygulamanızda bir şeylerin yanlış olup olmadığını bilmenin ve düzeltilmesi gerekenleri teşhis etmenin tek yoludur. Teknik olarak bir hata ayıklamayı üretim hizmetine bağlamak mümkün olsa da, bu yaygın bir uygulama değildir. Bu nedenle, ayrıntılı enstrümantasyon verilerine sahip olmak önemlidir.

Bazı ürünler kodunuzu otomatik olarak algılar. Bu çözümler iyi çalışsa da, manuel enstrümantasyonun işletme mantığınıza özgü olması hemen hemen her zaman gereklidir. Sonunda, adli olarak başvuruyu ayıklamak için yeterli bilgiye sahip olmalısınız. Servis Kumaş uygulamaları herhangi bir günlük çerçevesi ile enstrümanted olabilir. Bu belge, kodunuzu aygıta ve bir yaklaşımı diğerine göre ne zaman seçeceğinize yönelik birkaç farklı yaklaşımı açıklar. 

Bu önerilerin nasıl kullanılacağına ilişkin örnekler [için](service-fabric-how-to-diagnostics-log.md)bkz.

## <a name="application-insights-sdk"></a>Uygulama Öngörüleri SDK

Application Insights, Servis Kumaşı ile zengin bir entegrasyona sahiptir. Kullanıcılar AI Service Fabric nuget paketlerini ekleyebilir ve Azure portalında oluşturulan ve toplanan veri ve günlükleri alabilir. Ayrıca, kullanıcıların uygulamalarını tanılamak ve hata ayıklamak ve uygulamalarının hangi hizmetlerinin ve bölümlerinin en çok kullanıldığını izlemek için kendi telemetrilerini eklemeleri teşvik edilir. SDK'daki [TelemetryClient](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient?view=azure-dotnet) sınıfı, uygulamalarınızda telemetriyi izlemenin birçok yolunu sağlar. [Bir .NET uygulamasını izleme](service-fabric-tutorial-monitoring-aspnet.md) ve tanılama öğreticimizde uygulamanız için nasıl araç ve uygulama öngörüleri ekleyeceğinize ilişkin bir örneğe göz atın

## <a name="eventsource"></a>EventSource

Visual Studio'daki bir şablondan Bir Hizmet Kumaşı çözümü oluşturduğunuzda, **EventSource**türetilmiş bir sınıf **(ServiceEventSource** veya **ActorEventSource)** oluşturulur. Uygulamanız veya hizmetiniz için olaylar ekleyebileceğiniz bir şablon oluşturulur. **EventSource** adı benzersiz **olmalıdır** ve varsayılan şablon dizesi&lt;MyCompany-çözüm&gt;-&lt;projesiyeniden&gt;adlandırılmalıdır. Aynı adı kullanan birden çok **EventSource** tanımına sahip olmak, çalışma zamanında bir soruna neden olur. Tanımlanan her olayın benzersiz bir tanımlayıcısı olmalıdır. Bir tanımlayıcı benzersiz değilse, çalışma zamanı hatası oluşur. Bazı kuruluşlar, ayrı geliştirme ekipleri arasındaki çakışmaları önlemek için tanımlayıcılar için değer aralıklarını önceden atar. Daha fazla bilgi için [Vance'in bloguna](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) veya [MSDN belgelerine](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx)bakın.

## <a name="aspnet-core-logging"></a>ASP.NET Çekirdek günlüğü

Kodunuzu nasıl algılatacağınızı dikkatlice planlamak önemlidir. Doğru enstrümantasyon planı, kod tabanınızın istikrarını bozabilir ve ardından kodu yeniden aygıtlandırmanıza gerek kalmadan kaçınmanıza yardımcı olabilir. Riski azaltmak için, Microsoft ASP.NET Core'un bir parçası olan [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/)gibi bir enstrümantasyon kitaplığı seçebilirsiniz. ASP.NET Core, mevcut kod üzerindeki etkisini en aza indirirken, seçtiğiniz sağlayıcıile kullanabileceğiniz bir [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) arabirimine sahiptir. Kodu Windows ve Linux'ta ASP.NET Core'da ve tam .NET Framework'de kullanabilirsiniz, böylece enstrümantasyon kodunuz standartlaştırılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınızı ve hizmetlerinizi enstrüman lamak için günlük sağlayıcınızı seçtikten sonra, günlüklerinizin ve etkinliklerinizin herhangi bir analiz platformuna gönderilmeden önce bir araya gelmeleri gerekir. Azure Monitor önerilen seçeneklerden bazılarını daha iyi anlamak için [Uygulama Öngörüleri](service-fabric-diagnostics-event-analysis-appinsights.md) ve [Etkinlik Akışı](service-fabric-diagnostics-event-aggregation-eventflow.md) hakkında bilgi edinin.
