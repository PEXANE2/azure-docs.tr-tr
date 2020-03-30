---
title: Azure Monitör Uygulama Öngörüleri NuGet paketleri
description: Azure Monitörü Uygulama Öngörüleri NuASP.NET, ASP.NET Core, Python için paket listeleri alın
ms.topic: reference
ms.date: 10/16/2018
ms.openlocfilehash: 5675c0fad61f1b75aa7e93db02246eed43967ec2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670007"
---
# <a name="application-insights-nuget-packages"></a>Uygulama Öngörüleri NuGet paketleri

Aşağıda, Uygulama Öngörüleri için kararlı sürüm NuGet Paketleri'nin geçerli listesi verilmiştir.

## <a name="common-packages-for-aspnet"></a>ASP.NET için ortak paketler

| Paket Adı | Kararlı Sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights | 2.12.0 | Tüm Uygulama Öngörüleri Telemetri Türlerinin iletimi için temel işlevsellik sağlar ve diğer tüm Application Insights paketleri için bağımlı bir pakettir | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
|Microsoft.ApplicationInsights.Agent.Intercept | 2.4.0 | Yöntem çağrılarının engellenmesini sağlar | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent.Intercept/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.12.0 | .NET uygulamaları için Uygulama Öngörüleri Bağımlılık Toplayıcısı. Bu, Application Insights platformuna özgü paketler için bağımlı bir pakettir ve bağımlılık telemetrisinin otomatik olarak toplanmasını sağlar. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.12.0 | Uygulama Öngörüleri Performans Sayaçları Toplayıcı, Performans Sayaçları tarafından toplanan verileri Uygulama Öngörülerine göndermenize olanak tanır. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.Web | 2.12.0 | .NET web uygulamaları için Uygulama Öngörüleri | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.12.0 | Application Insights Windows Server NuGet paketi ,NET uygulamaları için uygulama öngörüleri telemetriotomatik toplama sağlar. Bu paket, Application Insights platformuna özel paketler için bağımlı bir paket olarak veya platforma özel paketler kapsamında olmayan .NET uygulamaları için bağımsız bir paket olarak kullanılabilir (.NET çalışanı rolleri gibi). | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.12.0 | Uygulama Öngörüleri Windows Server SDK'ya çevrimdışı senaryolarda telemetriyi koruyacak bir telemetri kanalı sağlar. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="common-packages-for-aspnet-core"></a>ASP.NET Core için ortak paketler

| Paket Adı | Kararlı Sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AspNetCore | 2.5.0 | ASP.NET Core web uygulamaları için Uygulama Öngörüleri. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) |
| Microsoft.ApplicationInsights | 2.12.0 | Bu paket, tüm Uygulama Öngörüleri Telemetri Türlerinin iletimi için temel işlevsellik sağlar ve diğer tüm Uygulama Öngörüleri paketleri için bağımlı bir pakettir | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.12.0 | .NET uygulamaları için Uygulama Öngörüleri Bağımlılık Toplayıcısı. Bu, Application Insights platformuna özgü paketler için bağımlı bir pakettir ve bağımlılık telemetrisinin otomatik olarak toplanmasını sağlar. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.12.0 | Uygulama Öngörüleri Performans Sayaçları Toplayıcı, Performans Sayaçları tarafından toplanan verileri Uygulama Öngörülerine göndermenize olanak tanır. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.12.0 | Application Insights Windows Server NuGet paketi ,NET uygulamaları için uygulama öngörüleri telemetriotomatik toplama sağlar. Bu paket, Application Insights platformuna özel paketler için bağımlı bir paket olarak veya platforma özel paketler kapsamında olmayan .NET uygulamaları için bağımsız bir paket olarak kullanılabilir (.NET çalışanı rolleri gibi). | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  |
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.12.0 | Uygulama Öngörüleri Windows Server SDK'ya çevrimdışı senaryolarda telemetriyi koruyacak bir telemetri kanalı sağlar. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="common-packages-for-python-using-opencensus"></a>OpenCensus kullanarak Python için ortak paketler
| Paket Adı | Kararlı Sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| opencensus-ext-azure | 1.0.0 | OpenCensus aracılığıyla Azure Monitor altında Python uygulamaları için Uygulama Öngörüleri. | [Paketi İndir](https://pypi.org/project/opencensus-ext-azure/) |
| opencensus-ext-django | 0.7.2 | Bu paket Python [django](https://pypi.org/project/django/) kitaplığı ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-django/) |
| opencensus-ext-flask | 0.7.3 | Bu paket Python [şişe](https://pypi.org/project/flask/) kitaplığı ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-flask/) |
| opencensus-ext-httplib | 0.7.2 | Bu paket Python3 için Python [http.client](https://docs.python.org/3/library/http.client.html) kitaplığı ve Python2 için [httplib](https://docs.python.org/2/library/httplib.html) ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-httplib/) |
| opencensus-ext-tomruk | 0.1.0 | Bu paket, izleme verileriyle günlük kayıtlarını zenginleştirir. | [Paketi İndir](https://pypi.org/project/opencensus-ext-logging/) |
| opencensus-ext-mysql | 0.1.2 | Bu paket Python [mysql-bağlayıcı](https://pypi.org/project/mysql-connector/) kitaplığı ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-mysql/) |
| opencensus-ext-postgresql | 0.1.2 | Bu paket Python [psycopg2](https://pypi.org/project/psycopg2/) kitaplığı ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-postgresql/) |
| opencensus-ext-pymongo | 0.7.1 | Bu paket Python [pymongo](https://pypi.org/project/pymongo/) kitaplığı ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-pymongo/) |
| opencensus-ext-pymysql | 0.1.2 | Bu paket Python [PyMySQL](https://pypi.org/project/PyMySQL/) kitaplığı ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-pymysql/) |
| opencensus-ext-piramit | 0.7.1 | Bu paket Python [piramit](https://pypi.org/project/pyramid/) kitaplığı ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-pyramid/) |
| opencensus-ext-requests | 0.7.2 | Bu paket Python [istekleri](https://pypi.org/project/requests/) kitaplığı ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-requests/) |
| opencensus-ext-sqlalchemy | 0.1.2 | Bu paket Python [SQLAlchemy](https://pypi.org/project/SQLAlchemy/) kitaplığı ile tümleştirme sağlar. | [Paketi İndir](https://pypi.org/project/opencensus-ext-sqlalchemy/) |

## <a name="listenerscollectorsappenders"></a>Dinleyiciler/koleksiyoncular/toplayıcılar

| Paket Adı | Kararlı Sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.DiagnosticSourceListener | 2.7.2 |  Olayların DiagnosticSource'dan Application Insights'a iletilmesine olanak tanır. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/) |
| Microsoft.ApplicationInsights.EventSourceListener | 2.7.2 | Uygulama Öngörüleri EventSourceListener, EventSource etkinliklerinden Uygulama Öngörülerine veri göndermenize olanak tanır. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/) |
| Microsoft.ApplicationInsights.EtwCollector | 2.7.2 | Uygulama Öngörüleri EtwCollector, Windows için Olay İzleme'den (ETW) Application Insights'a veri göndermenize olanak tanır. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/) |
| Microsoft.ApplicationInsights.TraceListener | 2.7.2 | Uygulama Öngörüleri'ne izleme günlüğü iletileri göndermenize olanak tanıyan özel bir TraceListener. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/) |
| Microsoft.ApplicationInsights.Log4NetAppender | 2.7.2 | Application Insights'a Log4Net günlük iletileri göndermenize olanak tanıyan özel bir uygulama. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
| Microsoft.ApplicationInsights.NLogTarget | 2.7.2 |  Uygulama Öngörüleri'ne NLog günlük iletileri göndermenize olanak tanıyan özel bir hedeftir. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
| Microsoft.ApplicationInsights.SnapshotCollector | 1.3.1 | Uygulamanızdaki özel durumları izler ve çevrimdışı çözümleme için anlık görüntüleri otomatik olarak toplar. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector/)

## <a name="service-fabric"></a>Service Fabric

| Paket Adı | Kararlı Sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.ServiceFabric | 2.2.0 | Bu paket, uygulamanın içinde çalıştığı servis kumaşı bağlamı ile telemetrinin otomatik dekorasyonunu sağlar. Bu NuGet'i Yerel Hizmet Kumaşı uygulamaları için kullanmayın. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric/) |
| Microsoft.ApplicationInsights.ServiceFabric.Native | 2.2.0 | Servis kumaşı uygulamaları için Uygulama Öngörüleri modülü. Bu NuGet'i yalnızca Yerel Hizmet Kumaşı uygulamaları için kullanın. Kapsayıcılarda çalışan uygulamalar için Microsoft.ApplicationInsights.ServiceFabric paketini kullanın. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric.Native/) |  

## <a name="status-monitor"></a>Durum monitörü

| Paket Adı | Kararlı Sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.Agent_x64 | 2.2.1 |  x64 uygulamaları için çalışma zamanı veri toplamayı sağlar | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x64/) |
| Microsoft.ApplicationInsights.Agent_x86 | 2.2.1 |  x86 uygulamaları için çalışma zamanı veri toplamayı sağlar. | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x86/) |

Bu paketler Durum İzleyicisi'ndeki çalışma zamanı [Status Monitor](../../azure-monitor/app/monitor-performance-live-website-now.md)izlemenin temel işlevselliğinin bir parçasını içerir. Bu paketleri doğrudan indirmeniz gerekmez, sadece Durum Monitörü yükleyicisini kullanın. Eğer bu paketlerin başlık altında nasıl çalıştığını hakkında daha fazla anlamak istiyorsanız bizim geliştiricilerden biri bu [blog yazısı](https://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/) iyi bir başlangıçtır.

## <a name="additional-packages"></a>Ek paketler

| Paket Adı | Kararlı Sürüm | Açıklama | İndirme |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AzureWebSiteleri | 2.6.5 | Bu uzantı, Bir Azure Uygulama Hizmeti'nde Uygulama Öngörüleri izleme olanağı sağlar. SDK sürüm 2.6.1. Talimatlar: ikey'inizle 'APPINSIGHTS_INSTRUMENTATIONKEY' uygulama ayarlarını ekleyin ve etkili olmak için webapp'ı yeniden başlatın.| [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AzureWebSites/) |
| Microsoft.ApplicationInsights.Injector | 2.6.7 | Bu paket, kodsuz Application Insights enjeksiyonu için gerekli dosyaları içerir | [Paketi İndir](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Injector/) |

## <a name="next-steps"></a>Sonraki adımlar

- [Monitör ASP.NET Çekirdek](../../azure-monitor/app/asp-net-core.md).
- Core [Azure Linux web uygulamaları](../../azure-monitor/app/profiler-aspnetcore-linux.md)ASP.NET profil.
- Hata ayıklama ASP.NET [anlık görüntüler.](../../azure-monitor/app/snapshot-debugger.md)
