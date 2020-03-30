---
title: Azure Uygulama Öngörüleri - Bağımlılık Otomatik Toplama | Microsoft Dokümanlar
description: Uygulama Öngörüleri bağımlılıkları otomatik olarak toplar ve görselleştirir
ms.topic: reference
author: nikmd23
ms.author: nimolnar
ms.date: 04/29/2019
ms.reviewer: mbullwin
ms.openlocfilehash: eaafe19f5112b433d50a34aa551aa84d196726a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665825"
---
# <a name="dependency-auto-collection"></a>Bağımlılık otomatik toplama

Aşağıda, uygulamanızın kodunda ek bir değişiklik gerektirmeden otomatik olarak bağımlılık olarak algılanan bağımlılık çağrılarının şu anda desteklenen listesi verilmiştir. Bu bağımlılıklar Uygulama Öngörüleri [Uygulama haritası](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) ve [İşlem tanılama görünümlerinde](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics) görselleştirilmiştir. Bağımlılığınız aşağıdaki listede yoksa, [bir parça bağımlılık çağrısıyla](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency)el ile takip edebilirsiniz.

## <a name="net"></a>.NET

| Uygulama çerçeveleri| Sürümler |
| ------------------------|----------|
| ASP.NET WebFormları | 4,5+ |
| ASP.NET MVC | 4+ |
| ASP.NET WebAPI | 4,5+ |
| ASP.NET Core | 1.1+ |
| <b>İletişim kütüphaneleri</b> |
| [httpİsteC](https://www.microsoft.com/net/) | 4.5+, .NET Çekirdek 1.1+ |
| [Sqlclient](https://www.nuget.org/packages/System.Data.SqlClient) | .NET Çekirdek 1.0+, NuGet 4.3.0 |
| [EventHubs İstemci SDK](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1.1.0 |
| [ServiceBus Müşteri SDK](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) | 3.0.0 |
| <b>Depolama istemcileri</b>|  |
| ADO.NET | 4,5+ |

## <a name="java"></a>Java
| Uygulama sunucuları | Sürümler |
|-------------|----------|
| [Tomcat](https://tomcat.apache.org/) | 7, 8 | 
| [JBoss EAP](https://developers.redhat.com/products/eap/download/) | 6, 7 |
| [Jetty](https://www.eclipse.org/jetty/) | 9 |
| <b>Uygulama çerçeveleri</b> |  |
| [Spring](https://spring.io/) | 3,0 |
| [Spring Boot](https://spring.io/projects/spring-boot) | 1.5.9+<sup>*</sup> |
| Java Servlet | 3.1+ |
| <b>İletişim kütüphaneleri</b> |  |
| [Apache Http İstemci](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) | 4.3+<sup>†</sup> |
| <b>Depolama istemcileri</b> | |
| [SQL Server]( https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc) | 1+<sup>† †</sup> |
| [PostgreSQL (Beta Desteği)](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/CHANGELOG.md#version-240-beta) | |
| [Oracle]( https://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html) | 1+<sup>† †</sup> |
| [Mysql]( https://mvnrepository.com/artifact/mysql/mysql-connector-java) | 1+<sup>† †</sup> |
| <b>Günlük kitaplıkları</b> | |
| [Giriş](https://logback.qos.ch/) | 1+ |
| [Log4j](https://logging.apache.org/log4j/) | 1.2+ |
| <b>Metrik kitaplıklar</b> |  |
| JMX | 1.0+ |

> [!NOTE]
> *Reaktif programlama desteği hariç.
> <br>[†JVM Aracısının](https://docs.microsoft.com/azure/application-insights/app-insights-java-agent#install-the-application-insights-agent-for-java)yüklenmesini gerektirir.

## <a name="nodejs"></a>Node.js

| İletişim kütüphaneleri | Sürümler |
| ------------------------|----------|
| [HTTP](https://nodejs.org/api/http.html), [HTTPS](https://nodejs.org/api/https.html) | 0,10+ |
| <b>Depolama istemcileri</b> | |
| [Redis](https://www.npmjs.com/package/redis) | 2.x |
| [MongoDb](https://www.npmjs.com/package/mongodb); [MongoDb Çekirdek](https://www.npmjs.com/package/mongodb-core) | 2.x - 3.x |
| [Mysql](https://www.npmjs.com/package/mysql) | 2.0.0 - 2.16.x |
| [PostgreSql](https://www.npmjs.com/package/pg); | 6.x - 7.x |
| [pg-havuz](https://www.npmjs.com/package/pg-pool) | 1.x - 2.x |
| <b>Günlük kitaplıkları</b> | |
| [Konsol](https://nodejs.org/api/console.html) | 0,10+ |
| [Bunyan](https://www.npmjs.com/package/bunyan) | 1.x |
| [Winston](https://www.npmjs.com/package/winston) | 2.x - 3.x |

## <a name="javascript"></a>JavaScript

| İletişim kütüphaneleri | Sürümler |
| ------------------------|----------|
| [XMLHttpİstek](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) | Tümü |

## <a name="next-steps"></a>Sonraki adımlar

- [.NET](../../azure-monitor/app/asp-net-dependencies.md)için özel bağımlılık izleme ayarlayın.
- [Java](../../azure-monitor/app/java-agent.md)için özel bağımlılık izleme ayarlayın.
- [OpenCensus Python](../../azure-monitor/app/opencensus-python-dependency.md)için özel bağımlılık izleme ayarlayın.
- [Özel bağımlılık telemetrisi yazma](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Uygulama Öngörüleri türleri ve veri modeli için [veri modeline](../../azure-monitor/app/data-model.md) bakın.
- Application Insights tarafından desteklenen [platformlara](../../azure-monitor/app/platforms.md) göz atın.
