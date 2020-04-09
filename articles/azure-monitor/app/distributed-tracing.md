---
title: Azure Uygulama Öngörülerinde Dağıtılmış İzleme | Microsoft Dokümanlar
description: OpenCensus projesindeki ortaklığımız aracılığıyla dağıtılmış izleme için Microsoft'un desteği hakkında bilgi sağlar
ms.topic: conceptual
author: nikmd23
ms.author: nimolnar
ms.date: 09/17/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 83575aa7f9b63615f453e00bd06b00a5540b9a9e
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80892266"
---
# <a name="what-is-distributed-tracing"></a>Dağıtılmış İzleme Nedir?

Modern bulut ve [mikro hizmet](https://azure.com/microservices) mimarilerinin ortaya çıkışı, kullanılabilirliği ve üretimi artırırken maliyetleri azaltmaya yardımcı olabilecek basit, bağımsız olarak dağıtılabilir hizmetlere yol açmıştır. Ancak bu hareketler, bireysel hizmetlerin bir bütün olarak anlaşılmasını kolaylaştırmış olsa da, genel sistemlerin akıl yürütmesini ve hata ayıklamasını zorlaştırmış.

Yekpare mimarilerde, çağrı yığınlarıyla hata ayıklama yapmaya alıştık. Çağrı yığınları, yürütme akışını (Yöntem C olarak adlandırılan Yöntem B olarak adlandırılan Yöntem B) ve bu çağrıların her biri hakkında ayrıntılar ve parametreleri göstermek için parlak araçlardır. Bu, tek bir işlemde çalışan monolitler veya hizmetler için harikadır, ancak çağrı yalnızca yerel yığındaki bir başvuru değil, bir işlem sınırının ötesindeyken nasıl hata ayıklama yaparız? 

İşte burada dağıtılmış izleme devreye giriyor.  

Dağıtılmış izleme, modern bulut ve mikro hizmet mimarileri için çağrı yığınlarına eşdeğerdir ve basit bir performans profilleyicisi de eklenir. Azure Monitor'da, dağıtılmış izleme verilerini tüketmek için iki deneyim sağlarız. Birincisi, zaman boyutu eklendiği bir çağrı yığını gibi olan [işlem tanılama](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics) görünümüdür. Hareket tanılama görünümü tek bir işlem/istekte görünürlük sağlar ve her istek bazında güvenilirlik sorunlarının ve performans darboğazlarının temel nedenini bulmak için yararlıdır.

Azure Monitor ayrıca, sistemlerin nasıl etkileşime girdiği ve ortalama performans ve hata oranlarının ne olduğu hakkında topolojik bir görünüm göstermek için birçok işlemi bir araya getiren bir [uygulama haritası](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) görünümü de sunar. 

## <a name="how-to-enable-distributed-tracing"></a>Dağıtılmış İzleme Nasıl Etkinleştirilir?

Bir uygulamada hizmetler arasında dağıtılmış izlemeyi etkinleştirmek, hizmetin uygulandığı dile göre her hizmete uygun aracıyı, SDK'yı veya kitaplığı eklemek kadar kolaydır.

## <a name="enabling-via-application-insights-through-auto-instrumentation-or-sdks"></a>Otomatik enstrümantasyon veya SDK'lar aracılığıyla Uygulama Öngörüleri ile etkinleştirme

.NET, .NET Core, Java, Node.js ve JavaScript için Bulunan Application Insights aracıları ve/veya SDK'ları tüm destek yerel olarak izleme olarak dağıtılır. Her Uygulama Öngörüleri SDK'nın yüklenmesi ve yapılandırılmasına ilişkin talimatlaraşağıda mevcuttur:

* [.NET](https://docs.microsoft.com/azure/application-insights/quick-monitor-portal)
* [.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-dotnetcore-quick-start)
* [Java](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent)
* [Node.js](https://docs.microsoft.com/azure/application-insights/app-insights-nodejs-quick-start)
* [JavaScript](https://docs.microsoft.com/azure/application-insights/app-insights-javascript)
* [Python](opencensus-python.md)

Uygun Uygulama Öngörüleri SDK yüklü ve yapılandırılmış, izleme bilgileri otomatik olarak popüler çerçeveler, kütüphaneler ve teknolojiler için SDK bağımlılık otomatik toplayıcıları tarafından toplanır. Desteklenen teknolojilerin tam listesi Bağımlılık [otomatik toplama belgelerinde](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies)kullanılabilir.

 Ayrıca, herhangi bir teknoloji [TelemetryClient](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics) [TrackDependency](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics) bir çağrı ile el ile izlenebilir.

## <a name="enable-via-opencensus"></a>OpenCensus ile etkinleştirme

Application Insights SDK'larına ek olarak, Application Insights [OpenCensus](https://opencensus.io/)aracılığıyla dağıtılmış izlemeyi de destekler. OpenCensus açık kaynak kodlu, satıcı-agnostik, ölçümler toplama ve hizmetler için dağıtılmış izleme sağlamak için kitaplıkların tek dağıtım. Ayrıca, açık kaynak topluluğunun Redis, Memcached veya MongoDB gibi popüler teknolojilerle dağıtılmış izleme olanağı sağlamasına olanak tanır. [Microsoft, OpenCensus'da diğer birçok izleme ve bulut ortağıyla işbirliği yapmaktadır.](https://open.microsoft.com/2018/06/13/microsoft-joins-the-opencensus-project/)

[Python](opencensus-python.md) 

OpenCensus web sitesi [Python](https://opencensus.io/api/python/trace/usage.html) ve [Go](https://godoc.org/go.opencensus.io)için API başvuru belgelerinin yanı sıra OpenCensus'ı kullanmak için çeşitli kılavuzları da tutar. 

## <a name="next-steps"></a>Sonraki adımlar

* [OpenCensus Python kullanım kılavuzu](https://opencensus.io/api/python/trace/usage.html)
* [Uygulama haritası](./../../azure-monitor/app/app-map.md)
* [Uçuça performans izleme](./../../azure-monitor/learn/tutorial-performance.md)
