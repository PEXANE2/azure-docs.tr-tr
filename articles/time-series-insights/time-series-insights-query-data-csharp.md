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
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: 28f255c66c0e4a929914d037ee9c93b509a3602b
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68677774"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Kullanarak Azure Time Series Insights GA ortamından veri sorgulamaC#

Bu C# örnek, Azure Time Series Insights GA ortamından verilerin nasıl sorgulanacağını gösterir.

Örnek, Sorgu API’si kullanımının birkaç temel örneğini gösterir:

1. Bir hazırlık adımı olarak, Azure Active Directory API 'SI aracılığıyla erişim belirtecini alın. Bu belirteci `Authorization` her sorgu API isteğinin üstbilgisine geçirin. Etkileşimli olmayan uygulamalar ayarlamak için bkz. [kimlik doğrulama ve yetkilendirme](time-series-insights-authentication-and-authorization.md). Ayrıca, örneğin başlangıcında tanımlanan tüm sabitlerin doğru ayarlandığından emin olun.
1. Kullanıcının erişimi olan ortamların listesi alınır. Ortamların biri, ilgilendiğiniz ortam olarak alınır ve bu ortam için daha fazla veri sorgulanır.
1. HTTPS isteğinin bir örneği olarak, ilgilenilen ortam için kullanılabilirlik verileri istenir.
1. Web yuvası isteğinin bir örneği olarak, ilgilenilen ortam için toplam olay verileri istenir. Veriler kullanılabilir oldukları tüm zaman aralığı için istenir.

> [!NOTE]
> Örnek kodu adresinde [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample)bulabilirsiniz.

## <a name="project-dependencies"></a>Proje bağımlılıkları

NuGet paketleri `Microsoft.IdentityModel.Clients.ActiveDirectory` ve `Newtonsoft.Json`ekleyin.

## <a name="c-example"></a>C#örneğinde

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Sonraki adımlar

- Sorgulama hakkında daha fazla bilgi edinmek için [sorgu API 'si başvurusunu](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api)okuyun.

- [JavaScript tek sayfalı uygulamasını Time Series Insights bağlama](tutorial-create-tsi-sample-spa.md) hakkında bilgi edinin.