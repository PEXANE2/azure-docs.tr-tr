---
title: Kod Azure Time Series Insights kullanarak C# bir GA ortamından veri sorgulama | Microsoft Docs
description: "' De C#yazılmış özel bir uygulama kullanarak Azure Time Series Insights ortamından veri sorgulama hakkında bilgi edinin."
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/31/2020
ms.custom: seodec18
ms.openlocfilehash: a5cb435b38a776ba652854592bdc7d3e833742d1
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76935088"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Kullanarak Azure Time Series Insights GA ortamından veri sorgulamaC#

Bu C# örnek, Azure Time Series Insights GA ortamlarından verileri sorgulamak Için [ga sorgu API 'lerinin](https://docs.microsoft.com/rest/api/time-series-insights/ga-query) nasıl kullanılacağını gösterir.

## <a name="summary"></a>Özet

Aşağıdaki örnek kod aşağıdaki özellikleri göstermektedir:

* [Microsoft. IdentityModel. clients. ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)kullanarak Azure Active Directory aracılığıyla erişim belirteci alma.

* Bu alınan erişim belirtecini sonraki sorgu API isteklerinin `Authorization` üst bilgisinde geçirme. 

* Örnek, HTTP isteklerinin ' de nasıl yapıldığını gösteren bir GA sorgu API 'lerinin her birini çağırır:
    * Kullanıcının erişimi olan ortamları döndürmesi için [ortamlar API 'Si al](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environments-api)
    * [Ortam kullanılabilirliği API 'sini al](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-availability-api)
    * Ortam meta verilerini almak için [ortam meta verileri API 'Sini alma](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-metadata-api)
    * [Ortam olayları API 'SI al](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-api)
    * [Ortam toplamaları API 'SI al](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-api)
    
* WSS kullanarak GA sorgu API 'Leriyle etkileşim kurma:

   * [Ortam olaylarını akışlı API al](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-streamed-api)
   * [Ortam toplamlarını akışlı API al](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-streamed-api)

> [!NOTE]
> Örnek kod [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample)adresinde bulunabilir.

## <a name="prerequisites-and-setup"></a>Önkoşullar ve kurulum

Örnek kodu derleyip çalıştırmadan önce aşağıdaki adımları gerçekleştirin:

1. [BIR GA Azure Time Series Insights ortamı sağlayın](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started) .

1. [Kimlik doğrulama ve yetkilendirme](time-series-insights-authentication-and-authorization.md)bölümünde açıklandığı gibi, Azure Active Directory için Azure Time Series Insights ortamınızı yapılandırın. 

1. Gerekli proje bağımlılıklarını yükler.

1. Her **#DUMMY #** öğesini uygun ortam tanımlayıcısı ile değiştirerek aşağıdaki örnek kodu düzenleyin.

1. Visual Studio içinde kodu yürütün.

> [!TIP]
> * https://github.com/Azure-Samples/Azure-Time-Series-Insights diğer GA C# kod örneklerini görüntüleyin [ ](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample).

## <a name="project-dependencies"></a>Proje bağımlılıkları

Visual Studio 'nun en yeni sürümünü kullanmanız önerilir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) -sürüm 16.4.2 +

Örnek kodun iki zorunlu bağımlılığı vardır:

* [Microsoft. IdentityModel. clients. ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) -3.13.9 paketi.
* [Newtonsoft. JSON](https://www.nuget.org/packages/Newtonsoft.Json) -9.0.1 paketi.

[NuGet 2.12 +](https://www.nuget.org/)kullanarak paketleri ekleyin:

* `dotnet add package Newtonsoft.Json --version 9.0.1`
* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 3.13.9`

## <a name="c-sample-code"></a>C#örnek kod

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Sonraki adımlar

- Sorgulama hakkında daha fazla bilgi edinmek için [sorgu API 'si başvurusunu](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api)okuyun.

- Time Series Insights için [istemci SDK 'sını kullanarak bir JavaScript uygulamasının nasıl bağlanacağını](https://github.com/microsoft/tsiclient) okuyun.
