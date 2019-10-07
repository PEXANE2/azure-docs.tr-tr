---
title: Kodu kullanarak C# Azure Time Series Insights GA ortamından veri sorgulama | Microsoft Docs
description: Bu makalede, C# (C-Sharp) .net dilinde yazılmış özel bir uygulamayı kodlayarak bir Azure Time Series Insights ortamından veri sorgulama açıklanmaktadır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
reviewer: jasonwhowell, kfile, tsidocs
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 10/03/2019
ms.custom: seodec18
ms.openlocfilehash: 5928a8ee048b608d691ecea33d186838ec743e4b
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2019
ms.locfileid: "71981135"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Kullanarak Azure Time Series Insights GA ortamından veri sorgulamaC#

Bu C# örnek, Azure Time Series Insights GA ortamından verilerin nasıl sorgulanacağını gösterir.

Örnek, Sorgu API’si kullanımının birkaç temel örneğini gösterir:

1. Bir hazırlık adımı olarak, Azure Active Directory API 'SI aracılığıyla erişim belirtecini alın. Bu belirteci her sorgu API isteğinin `Authorization` üst bilgisinde geçirin. Etkileşimli olmayan uygulamalar ayarlamak için bkz. [kimlik doğrulama ve yetkilendirme](time-series-insights-authentication-and-authorization.md). Ayrıca, örneğin başlangıcında tanımlanan tüm sabitlerin doğru ayarlandığından emin olun.
1. Kullanıcının erişimi olan ortamların listesi alınır. Ortamların biri, ilgilendiğiniz ortam olarak alınır ve bu ortam için daha fazla veri sorgulanır.
1. HTTPS isteğinin bir örneği olarak, ilgilenilen ortam için kullanılabilirlik verileri istenir.
1. Web yuvası isteğinin bir örneği olarak, ilgilenilen ortam için toplam olay verileri istenir. Veriler kullanılabilir oldukları tüm zaman aralığı için istenir.

> [!NOTE]
> Örnek kod [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample)' de mevcuttur.

## <a name="project-dependencies"></a>Proje bağımlılıkları

-0 ve `Newtonsoft.Json` @no__t NuGet paketleri ekleyin.

## <a name="c-example"></a>C#örneğinde

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Sonraki adımlar

- Sorgulama hakkında daha fazla bilgi edinmek için [sorgu API 'si başvurusunu](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api)okuyun.

- Time Series Insights için [istemci SDK 'sını kullanarak bir JavaScript uygulamasının nasıl bağlanacağını](https://github.com/microsoft/tsiclient) okuyun.