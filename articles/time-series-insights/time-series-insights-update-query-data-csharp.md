---
title: C# kullanarak Gen2 ortamından veri sorgulama-Azure Time Series Insights | Microsoft Docs
description: C# dilinde yazılmış bir uygulama kullanarak Azure Time Series Insights Gen2 ortamından verileri sorgulamayı öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/14/2020
ms.custom: seodec18
ms.openlocfilehash: 19c21f1e95127d064d90d5c6915b4dd67871adaa
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88263495"
---
# <a name="query-data-from-the-azure-time-series-insights-gen2-environment-using-c-sharp"></a>C Sharp kullanarak Azure Time Series Insights Gen2 ortamından veri sorgulama

Bu C# örneği, Azure Time Series Insights Gen2 ortamlarındaki [Gen2 veri erişimi API 'lerinden](https://docs.microsoft.com/rest/api/time-series-insights/reference-data-access-overview) nasıl veri sorgulanacağını gösterir.

> [!TIP]
> Gen2 C# kod örneklerini şurada görüntüleyin: [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/gen1-sample/csharp-tsi-gen1-sample) .

## <a name="summary"></a>Özet

Aşağıdaki örnek kod aşağıdaki özellikleri göstermektedir:

* [Azure otomatik Rest](https://github.com/Azure/AutoRest)'ten SDK otomatik oluşturma desteği.
* [Microsoft. IdentityModel. clients. ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/)kullanarak Azure Active Directory aracılığıyla erişim belirteci alma.
* Bu alınan erişim belirtecini `Authorization` sonraki veri ERIŞIMI API isteklerinin üst bilgisinde geçirme.
* Örnek, HTTP isteklerinin aşağıdakilere nasıl yapıldığını gösteren bir konsol arabirimi sağlar:
  * [Gen2 ortamları API 'SI](https://docs.microsoft.com/rest/api/time-series-insights/reference-environments-apis)
    * [Ortamların kullanılabilirlik API 'Sini al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/getavailability) ve [olay şeması API 'si al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/geteventschema)
  * [Gen2 sorgu API 'SI](https://docs.microsoft.com/rest/api/time-series-insights/reference-query-apis)
    * [Olayları al API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#getevents), [seri API 'Si al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#getseries)ve [toplama serisi API 'si al](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#aggregateseries)
  * [Zaman serisi modeli API 'Leri](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/query/execute#aggregateseries)
    * [Hiyerarşilerin API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeserieshierarchies) ve [hiyerarşileri al Batch API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeserieshierarchies/executebatch)
    * [Türleri al API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriestypes) ve [türler Batch API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriestypes/executebatch)
    * [Örnekleri al API](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances) ve [örnekleri Batch API 'si](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriesinstances/executebatch)

* Gelişmiş [arama](https://docs.microsoft.com/rest/api/time-series-insights/reference-model-apis#search-features) ve [TSX](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax) özellikleri.

## <a name="prerequisites-and-setup"></a>Önkoşullar ve kurulum

Örnek kodu derleyip çalıştırmadan önce aşağıdaki adımları gerçekleştirin:

1. [Bir Gen2 Azure Time Series Insights ortamı sağlayın](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-update-how-to-manage#create-the-environment) .
1. [Kimlik doğrulama ve yetkilendirme](time-series-insights-authentication-and-authorization.md)bölümünde açıklandığı gibi, Azure Active Directory için Azure Time Series Insights ortamınızı yapılandırın.
1. Azure Time Series Insights Gen2 istemci bağımlılıklarını oluşturmak için [GenerateCode.bat](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/gen2-sample/csharp-tsi-gen2-sample/DataPlaneClient/GenerateCode.bat) , [README.MD](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/gen2-sample/csharp-tsi-gen2-sample/DataPlaneClient/Readme.md) içinde belirtilen şekilde çalıştırın.
1. Çözümü açın `TSIPreviewDataPlaneclient.sln` ve `DataPlaneClientSampleApp` Visual Studio 'da varsayılan proje olarak ayarlayın.
1. [Aşağıda](#project-dependencies) açıklanan adımları kullanarak gerekli proje bağımlılıklarını yükler ve örneği yürütülebilir bir `.exe` dosyaya derleyin.
1. `.exe`Dosyayı çift tıklayarak çalıştırın.

## <a name="project-dependencies"></a>Proje bağımlılıkları

Visual Studio 'nun en yeni sürümünü kullanmanız önerilir:

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) -sürüm 16.4.2 +

Örnek kodun, [packages.config](https://github.com/Azure-Samples/Azure-Time-Series-Insights/blob/master/gen2-sample/csharp-tsi-gen2-sample/DataPlaneClientSampleApp/packages.config) dosyasında görüntülenebilen birkaç zorunlu bağımlılığı vardır.

**Build**  >  **Build Solution** seçeneğini seçerek paketleri Visual Studio 2019 ' de indirin.

Alternatif olarak, [NuGet 2.12 +](https://www.nuget.org/)kullanarak her bir paketi ekleyin. Örneğin:

* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 4.5.1`

## <a name="c-sample-code"></a>C# örnek kodu

C# örnek kodu, [csharpquery-example](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/gen2-sample/csharp-tsi-gen2-sample) adresinde bulunabilir

> [!NOTE]
>
> * Kod örneği, varsayılan ortam değişkenlerini değiştirmeksizin yürütülebilir.
> * Kod örneği bir .NET yürütülebilir konsol uygulamasına Derlenecek.

## <a name="next-steps"></a>Sonraki adımlar

* Sorgulama hakkında daha fazla bilgi edinmek için [sorgu API 'si başvurusunu](https://docs.microsoft.com/rest/api/time-series-insights/reference-query-apis)okuyun.

* Azure Time Series Insights için [istemci SDK 'sını kullanarak bir JavaScript uygulamasının nasıl bağlanacağını](https://github.com/microsoft/tsiclient) okuyun.
