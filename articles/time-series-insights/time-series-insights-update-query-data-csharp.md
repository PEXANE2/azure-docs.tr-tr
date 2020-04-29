---
title: C# Azure Time Series Insights kullanarak bir önizleme ortamından veri sorgulama | Microsoft Docs
description: C# dilinde yazılmış bir uygulama kullanarak Azure Time Series Insights ortamından verileri sorgulamayı öğrenin.
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
ms.openlocfilehash: fbc2cbc29cb23a21e7d3713091fc22f01bb1b15a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81379824"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>C kullanarak Azure Time Series Insights Preview ortamından veri sorgulama #

Bu C# örneği, Azure Time Series Insights Preview ortamlarındaki veri [erişimi API 'lerinde](https://docs.microsoft.com/rest/api/time-series-insights/preview) verilerin nasıl sorgulanacağını gösterir.

> [!TIP]
> Preview C# kod örneklerini adresinde [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample)görüntüleyin.

## <a name="summary"></a>Özet

Aşağıdaki örnek kod aşağıdaki özellikleri göstermektedir:

* [Azure otomatik Rest](https://github.com/Azure/AutoRest)'ten SDK otomatik oluşturma desteği.
* [Microsoft. IdentityModel. clients. ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)kullanarak Azure Active Directory aracılığıyla erişim belirteci alma.
* Bu alınan erişim belirtecini sonraki veri erişimi API isteklerinin `Authorization` üst bilgisinde geçirme. 
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
1. `TSIPreviewDataPlaneclient.sln` Çözümü açın ve Visual Studio `DataPlaneClientSampleApp` 'da varsayılan proje olarak ayarlayın.
1. [Aşağıda](#project-dependencies) açıklanan adımları kullanarak gerekli proje bağımlılıklarını yükler ve örneği yürütülebilir `.exe` bir dosyaya derleyin.
1. `.exe` Dosyayı çift tıklayarak çalıştırın.

## <a name="project-dependencies"></a>Proje bağımlılıkları

Visual Studio 'nun en yeni sürümünü kullanmanız önerilir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) -sürüm 16.4.2 +

Örnek kodun, [Packages. config](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/csharp-tsi-preview-sample/DataPlaneClientSampleApp/packages.config) dosyasında görüntülenebilen birkaç gerekli bağımlılığı vardır.

**Build** > **Build Solution** seçeneğini seçerek paketleri Visual Studio 2019 ' de indirin. 

Alternatif olarak, [NuGet 2.12 +](https://www.nuget.org/)kullanarak her bir paketi ekleyin. Örneğin:

* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 4.5.1`

## <a name="c-sample-code"></a>C# örnek kodu

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> * Kod örneği, varsayılan ortam değişkenlerini değiştirmeksizin yürütülebilir.
> * Kod örneği bir .NET yürütülebilir konsol uygulamasına Derlenecek.

## <a name="next-steps"></a>Sonraki adımlar

- Sorgulama hakkında daha fazla bilgi edinmek için [sorgu API 'si başvurusunu](https://docs.microsoft.com/rest/api/time-series-insights/preview-query)okuyun.

- Time Series Insights için [istemci SDK 'sını kullanarak bir JavaScript uygulamasının nasıl bağlanacağını](https://github.com/microsoft/tsiclient) okuyun.
