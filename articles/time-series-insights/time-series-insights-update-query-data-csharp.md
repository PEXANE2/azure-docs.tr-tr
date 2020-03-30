---
title: C# kullanarak Önizleme ortamından verileri sorgula - Azure Time Series Insights | Microsoft Dokümanlar
description: C# ile yazılmış bir uygulamayı kullanarak Azure Time Series Insights ortamından verileri nasıl sorgulayayın öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76980996"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>Azure Zaman Serisi Öngörüleri Önizleme ortamından C kullanarak verileri sorgula #

Bu C# örneği, Azure Zaman Serisi Öngörüleri Önizleme ortamlarında [Önizleme Veri Erişim API'lerinden](https://docs.microsoft.com/rest/api/time-series-insights/preview) gelen verilerin nasıl sorgulanır olduğunu gösterir.

> [!TIP]
> Önizleme C# kod [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample)örneklerini ' de görüntüleyin.

## <a name="summary"></a>Özet

Aşağıdaki örnek kod aşağıdaki özellikleri gösterir:

* [Azure AutoRest'ten](https://github.com/Azure/AutoRest)SDK otomatik üretimi için destek.
* [Microsoft.IdentityModel.Clients.ActiveDirectory'i](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)kullanarak Azure Active Directory aracılığıyla erişim jetonu nasıl elde edilir?
* Sonraki Data Access API isteklerinin `Authorization` üstbilgisinde bu edinilmiş erişim belirteci geçmek için nasıl. 
* Örnek, HTTP isteklerinin aşağıdakilere nasıl yapıldığını gösteren bir konsol arabirimi sağlar:

    * [Önizleme Ortamları API](https://docs.microsoft.com/rest/api/time-series-insights/preview#preview-environments-apis)
        * [Ortamların Kullanılabilirlik API'si alın](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/getavailability) ve [Olay Şeması API'si Alın](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/geteventschema)
    * [Önizleme Sorgusu API'sı](https://docs.microsoft.com/rest/api/time-series-insights/preview#query-apis)
        * [Olaylar API alın,](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getevents) [Seri API alın](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#getseries)ve [Toplu Serisi API alın](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
    * [Zaman Serisi Model API'leri](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/query/execute#aggregateseries)
        * [Hiyerarşiler API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/get) ve [Hiyerarşileri Toplu API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeserieshierarchies/executebatch) alın
        * [Türleri API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/get) ve [Türleri Toplu API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/executebatch) alın
        * [Örnek API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/get) ve [Örnekler Toplu API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriesinstances/executebatch) alın
* Gelişmiş [Arama](https://docs.microsoft.com/rest/api/time-series-insights/preview#search-features) ve [TSX](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) özellikleri.

## <a name="prerequisites-and-setup"></a>Önkoşullar ve kurulum

Örnek kodu derlemeden ve çalıştırmadan önce aşağıdaki adımları tamamlayın:

1. [Önizleme Azure Zaman Serisi Öngörüleri](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-how-to-manage#create-the-environment) ortamını sağlama.
1. Azure Zaman Serisi Öngörüleri ortamınızı Azure Etkin Dizini için [Kimlik Doğrulama ve yetkilendirmede](time-series-insights-authentication-and-authorization.md)açıklandığı şekilde yapılandırın. 
1. Zaman Serisi Öngörüleri Önizleme istemci bağımlılıklarını oluşturmak için [Readme.md](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/Readme.md) belirtildiği gibi [GenerateCode.bat](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClient/GenerateCode.bat) çalıştırın.
1. `TSIPreviewDataPlaneclient.sln` Çözümü açın ve `DataPlaneClientSampleApp` Visual Studio'da varsayılan proje olarak ayarlayın.
1. [Aşağıda](#project-dependencies) açıklanan adımları kullanarak gerekli proje bağımlılıklarını yükleyin ve `.exe` örneği yürütülebilir bir dosyaya derleyin.
1. Dosyayı `.exe` çift tıklatarak çalıştırın.

## <a name="project-dependencies"></a>Proje bağımlılıkları

Visual Studio'nun en yeni sürümünü kullanmanız önerilir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) - Sürüm 16.4.2+

Örnek [kod, packages.config](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClientSampleApp/packages.config) dosyasında görüntülenebilen birkaç gerekli bağımlılık vardır.

**Yapı** > **Çözüm** seçeneğini seçerek paketleri Visual Studio 2019'dan indirin. 

Alternatif olarak, [NuGet 2.12+](https://www.nuget.org/)kullanarak her paketi ekleyin. Örnek:

* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 4.5.1`

## <a name="c-sample-code"></a>C# örnek kodu

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> * Kod örneği varsayılan ortam değişkenleri değiştirilmeden yürütülebilir.
> * Kod örneği bir .NET çalıştırılabilir konsol uygulamasına derlenir.

## <a name="next-steps"></a>Sonraki adımlar

- Sorgulama hakkında daha fazla bilgi edinmek için [Sorgu API başvurularını](https://docs.microsoft.com/rest/api/time-series-insights/preview-query)okuyun.

- [SDK istemcisini kullanarak bir JavaScript uygulamasını](https://github.com/microsoft/tsiclient) Time Series Insights'a nasıl bağlandığımı okuyun.
