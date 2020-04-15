---
title: C# kodunu kullanarak GA ortamından gelen verileri sorgula - Azure Time Series Insights | Microsoft Dokümanlar
description: C# ile yazılmış özel bir uygulamayı kullanarak Azure Time Series Insights ortamından verileri nasıl sorgulayayın öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/14/2020
ms.custom: seodec18
ms.openlocfilehash: 754d1b80236d138693987cccee7a218ccd96b16b
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383879"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Azure Time Series Insights GA ortamından C kullanarak verileri sorgula #

Bu C# örneği, Azure Zaman Serisi Öngörüleri GA ortamlarından gelen verileri sorgulamak için [GA Sorgu API'lerinin](https://docs.microsoft.com/rest/api/time-series-insights/ga-query) nasıl kullanılacağını gösterir.

> [!TIP]
> GA C# kod [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample)örneklerini ' de görüntüleyin.

## <a name="summary"></a>Özet

Aşağıdaki örnek kod aşağıdaki özellikleri gösterir:

* [Microsoft.IdentityModel.Clients.ActiveDirectory'i](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)kullanarak Azure Active Directory aracılığıyla erişim jetonu nasıl elde edilir?

* Sonraki Sorgu API isteklerinin `Authorization` üstbilgisinde bu edinilmiş erişim belirteci geçmek için nasıl. 

* Örnek, HTTP isteklerinin nasıl yapıldığını gösteren GA Sorgu API'lerinin her birini çağırır:
    * [Kullanıcının](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environments-api) erişediği ortamları döndürmek için Ortam API'sini alma
    * [Ortam Kullanılabilirliği API'si alın](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-availability-api)
    * Ortam meta verilerini almak için [Ortam Meta veri API'sini alın](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-metadata-api)
    * [Ortamlar Olaylar API alın](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-api)
    * [Çevre Agregaları API alın](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-api)
    
* WSS'yi kullanarak GA Sorgu API'leri ile nasıl etkileşime girilir:

   * [Çevre Olayları Akışlı API alın](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-streamed-api)
   * [Çevre Agregaları Akışlı API alın](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-streamed-api)

## <a name="prerequisites-and-setup"></a>Önkoşullar ve kurulum

Örnek kodu derlemeden ve çalıştırmadan önce aşağıdaki adımları tamamlayın:

1. [GA Azure Zaman Serisi Öngörüleri](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started) ortamını sağlama.
1. Azure Zaman Serisi Öngörüleri ortamınızı Azure Etkin Dizini için [Kimlik Doğrulama ve yetkilendirmede](time-series-insights-authentication-and-authorization.md)açıklandığı şekilde yapılandırın. 
1. Gerekli proje bağımlılıklarını yükleyin.
1. Aşağıdaki örnek kodu, her **#DUMMY uygun** ortam tanımlayıcısıyla değiştirerek düzenleme.
1. Visual Studio içinde kodu çalıştırın.

## <a name="project-dependencies"></a>Proje bağımlılıkları

Visual Studio'nun en yeni sürümünü kullanmanız önerilir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) - Sürüm 16.4.2+

Örnek kodun iki gerekli bağımlılığı vardır:

* [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) - 3.13.9 paketi.
* [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json) - 9.0.1 paketi.

**Yapı** > **Çözüm** seçeneğini seçerek paketleri Visual Studio 2019'dan indirin.

Alternatif olarak, [NuGet 2.12+](https://www.nuget.org/)kullanarak paketleri ekleyin:

* `dotnet add package Newtonsoft.Json --version 9.0.1`
* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 3.13.9`

## <a name="c-sample-code"></a>C# örnek kodu

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Sonraki adımlar

- Sorgulama hakkında daha fazla bilgi edinmek için [Sorgu API başvurularını](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api)okuyun.

- [SDK istemcisini kullanarak bir JavaScript uygulamasını](https://github.com/microsoft/tsiclient) Time Series Insights'a nasıl bağlandığımı okuyun.
