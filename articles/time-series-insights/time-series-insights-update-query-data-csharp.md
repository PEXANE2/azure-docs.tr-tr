---
title: Azure Time Series Insights | kullanarak C# bir önizleme ortamından veri sorgulama Microsoft Docs
description: "' De C#yazılmış bir uygulamayı kullanarak Azure Time Series Insights ortamından veri sorgulama hakkında bilgi edinin."
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/03/2020
ms.custom: seodec18
ms.openlocfilehash: 76e3ac85a6725976ebd14dac1805079613c94ec6
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980996"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>Kullanarak Azure Time Series Insights önizleme ortamından veri sorgulamaC#

Bu C# örnek, Azure Time Series Insights Preview ortamlarında veri [erişim API 'lerinde](https://docs.microsoft.com/rest/api/time-series-insights/preview) verilerin nasıl sorgulanacağını gösterir.

> [!TIP]
> Önizleme C# kodu örneklerini [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample)görüntüleyin.

## <a name="summary"></a>Özet

Aşağıdaki örnek kod aşağıdaki özellikleri göstermektedir:

* [Azure otomatik Rest](https://github.com/Azure/AutoRest)'ten SDK otomatik oluşturma desteği.
* [Microsoft. IdentityModel. clients. ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)kullanarak Azure Active Directory aracılığıyla erişim belirteci alma.
* Bu alınan erişim belirtecini sonraki veri erişim API isteklerinin `Authorization` üst bilgisinde geçirme. 
* Örnek, HTTP isteklerinin ' de nasıl yapıldığını gösteren bir konsol arabirimi sağlar:

    * [Önizleme ortamları API 'SI](https://docs.microsoft.com/rest/api/time-series-insights/preview#preview-environments-apis)
        * [Ortamların kullanılabilirlik API 'Sini al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability) ve [olay şeması API 'si al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema)
    * [Preview sorgu API 'SI](https://docs.microsoft.com/rest/api/time-series-insights/preview#query-apis)
        * [Olayları al API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents), [seri API 'Si al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries)ve [toplama serisi API 'si al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
    * [Zaman serisi modeli API 'Leri](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
        * [Hiyerarşilerin API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/get) ve [hiyerarşileri al Batch API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/executebatch)
        * [Türleri al API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/get) ve [türler Batch API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/executebatch)
        * [Örnekleri al API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/get) ve [örnekleri Batch API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/executebatch)
* Gelişmiş [arama](https://docs.microsoft.com/rest/api/time-series-insights/preview#search-features) ve [TSX](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) özellikleri.

## <a name="prerequisites-and-setup"></a>Önkoşullar ve kurulum

Örnek kodu derleyip çalıştırmadan önce aşağıdaki adımları gerçekleştirin:

1. [Bir önizleme Azure Time Series Insights ortamı sağlayın](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-how-to-manage#create-the-environment) .
1. [Kimlik doğrulama ve yetkilendirme](time-series-insights-authentication-and-authorization.md)bölümünde açıklandığı gibi, Azure Active Directory için Azure Time Series Insights ortamınızı yapılandırın. 
1. Time Series Insights Preview istemci bağımlılıklarını oluşturmak için [README.MD](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/Readme.md) Içinde belirtilen [GenerateCode. bat](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/GenerateCode.bat) dosyasını çalıştırın.
1. `TSIPreviewDataPlaneclient.sln` çözümünü açın ve Visual Studio 'da varsayılan proje olarak `DataPlaneClientSampleApp` ayarlayın.
1. [Aşağıda](#project-dependencies) açıklanan adımları kullanarak gerekli proje bağımlılıklarını yükleyip örneği yürütülebilir bir `.exe` dosyasına derleyin.
1. `.exe` dosyasını çift tıklayarak çalıştırın.

## <a name="project-dependencies"></a>Proje bağımlılıkları

Visual Studio 'nun en yeni sürümünü kullanmanız önerilir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) -sürüm 16.4.2 +

Örnek kodun, [Packages. config](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClientSampleApp/packages.config) dosyasında görüntülenebilen birkaç gerekli bağımlılığı vardır.

**Yapılandırma** > **Build Solution** seçeneğini belirleyerek paketleri Visual Studio 2019 ' de indirin. 

Alternatif olarak, [NuGet 2.12 +](https://www.nuget.org/)kullanarak her bir paketi ekleyin. Örneğin:

* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 4.5.1`

## <a name="c-sample-code"></a>C#örnek kod

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> * Kod örneği, varsayılan ortam değişkenlerini değiştirmeksizin yürütülebilir.
> * Kod örneği bir .NET yürütülebilir konsol uygulamasına Derlenecek.

## <a name="next-steps"></a>Sonraki adımlar

- Sorgulama hakkında daha fazla bilgi edinmek için [sorgu API 'si başvurusunu](https://docs.microsoft.com/rest/api/time-series-insights/preview-query)okuyun.

- Time Series Insights için [istemci SDK 'sını kullanarak bir JavaScript uygulamasının nasıl bağlanacağını](https://github.com/microsoft/tsiclient) okuyun.
