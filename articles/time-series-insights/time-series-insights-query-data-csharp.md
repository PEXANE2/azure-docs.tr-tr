---
title: C# kodu kullanarak Gen1 ortamından veri sorgulama-Azure Time Series Insights Gen1 | Microsoft Docs
description: C# dilinde yazılmış özel bir uygulama kullanarak Azure Time Series Insights Gen1 ortamından verileri sorgulamayı öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/12/2020
ms.custom: seodec18
ms.openlocfilehash: 0e1976f51251913197eeec1a342eb1e891ddcaa6
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88206307"
---
# <a name="query-data-from-the-azure-time-series-insights-gen1-environment-using-c-sharp"></a>C Sharp kullanarak Azure Time Series Insights Gen1 ortamından veri sorgulama

Bu C# örneği, Azure Time Series Insights Gen1 ortamlarından verileri sorgulamak için [Gen1 sorgu API 'lerinin](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query) nasıl kullanılacağını gösterir.

> [!TIP]
> Gen1 C# kod örneklerini şurada görüntüleyin: [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/gen1-sample/csharp-tsi-gen1-sample) .

## <a name="summary"></a>Özet

Aşağıdaki örnek kod aşağıdaki özellikleri göstermektedir:

* [Microsoft. IdentityModel. clients. ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)kullanarak Azure Active Directory aracılığıyla erişim belirteci alma.

* Bu alınan erişim belirtecini `Authorization` Sonraki sorgu API 'si isteklerinin üst bilgisinde geçirme.

* Örnek, HTTP isteklerinin ' de nasıl yapıldığını gösteren Gen1 sorgu API 'lerinin her birini çağırır:
  * Kullanıcının erişimi olan ortamları döndürmesi için [ortamlar API 'Si al](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api#get-environments-api)
  * [Ortam kullanılabilirliği API 'sini al](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api#get-environment-availability-api)
  * Ortam meta verilerini almak için [ortam meta verileri API 'Sini alma](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api#get-environment-metadata-api)
  * [Ortam olayları API 'SI al](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api#get-environment-events-api)
  * [Ortam toplamaları API 'SI al](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api#get-environment-aggregates-api)

* Gen1 sorgu API 'Leriyle nasıl etkileşim kuracağınızı öğrenmek için WSS 'yi kullanarak:

  * [Ortam olaylarını akışlı API al](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api#get-environment-events-streamed-api)
  * [Ortam toplamlarını akışlı API al](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api#get-environment-aggregates-streamed-api)

## <a name="prerequisites-and-setup"></a>Önkoşullar ve kurulum

Örnek kodu derleyip çalıştırmadan önce aşağıdaki adımları gerçekleştirin:

1. [Bir Gen1 Azure Time Series Insights ortamı sağlayın](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started) .
1. [Kimlik doğrulama ve yetkilendirme](time-series-insights-authentication-and-authorization.md)bölümünde açıklandığı gibi, Azure Active Directory için Azure Time Series Insights ortamınızı yapılandırın.
1. Gerekli proje bağımlılıklarını yükler.
1. Her **#DUMMY #** öğesini uygun ortam tanımlayıcısı ile değiştirerek aşağıdaki örnek kodu düzenleyin.
1. Visual Studio içinde kodu yürütün.

## <a name="project-dependencies"></a>Proje bağımlılıkları

Visual Studio 'nun en yeni sürümünü kullanmanız önerilir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) -sürüm 16.4.2 +

Örnek kodun iki zorunlu bağımlılığı vardır:

* [Microsoft. IdentityModel. clients. ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) -3.13.9 paketi.
* [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json) -9.0.1 Package.

**Build**  >  **Build Solution** seçeneğini seçerek paketleri Visual Studio 2019 ' de indirin.

Alternatif olarak, [NuGet 2.12 +](https://www.nuget.org/)kullanarak paketleri ekleyin:

* `dotnet add package Newtonsoft.Json --version 9.0.1`
* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 3.13.9`

## <a name="c-sample-code"></a>C# örnek kodu

Time Series Insights Gen1 örnekleri, [csharpquery-example](https://github.com/Azure-Samples/Azure-Time-Series-Insights#tsi-gen1) yolunda bulunabilir

## <a name="next-steps"></a>Sonraki adımlar

* Sorgulama hakkında daha fazla bilgi edinmek için [sorgu API 'si başvurusunu](https://docs.microsoft.com/rest/api/time-series-insights/gen1-query-api)okuyun.

* Time Series Insights için [istemci SDK 'sını kullanarak bir JavaScript uygulamasının nasıl bağlanacağını](https://github.com/microsoft/tsiclient) okuyun.
Azure-Samples/Azure-Time-Series-Insights/Gen1-Sample/CSharp-TSİ-Gen1-Sample/program. cs
