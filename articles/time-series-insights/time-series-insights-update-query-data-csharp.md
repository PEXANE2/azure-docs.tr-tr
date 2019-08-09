---
title: Kodu kullanarak C# Azure Time Series Insights önizleme ortamından veri sorgulama | Microsoft Docs
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
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: da4e9626b2bf5b180cddba012af81059dd8b67fe
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68883276"
---
# <a name="query-data-from-the-azure-time-series-insights-preview-environment-using-c"></a>Kullanarak Azure Time Series Insights önizleme ortamından veri sorgulamaC#

Bu C# örnek, Azure Time Series Insights önizleme ortamından verilerin nasıl sorgulanacağını gösterir.

Örnek, Sorgu API’si kullanımının birkaç temel örneğini gösterir:

1. Bir hazırlık adımı olarak, Azure Active Directory API 'SI aracılığıyla erişim belirtecini alın. Bu belirteci `Authorization` her sorgu API isteğinin üstbilgisine geçirin. Etkileşimli olmayan uygulamalar ayarlamak için bkz. [kimlik doğrulama ve yetkilendirme](time-series-insights-authentication-and-authorization.md). Ayrıca, örneğin başlangıcında tanımlanan tüm sabitlerin doğru ayarlandığından emin olun.
1. Kullanıcının erişimi olan ortamların listesi alınır. Ortamların biri, ilgilendiğiniz ortam olarak alınır ve bu ortam için daha fazla veri sorgulanır.
1. HTTPS isteğinin bir örneği olarak, ilgilenilen ortam için kullanılabilirlik verileri istenir.
1. Web yuvası isteğinin bir örneği olarak, ilgilenilen ortam için toplam olay verileri istenir. Veriler kullanılabilir oldukları tüm zaman aralığı için istenir.

> [!NOTE]
> Bu örnek kod, adresinde [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-preview-sample)de mevcuttur.

## <a name="c-example"></a>C#örneğinde

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-preview-sample/DataPlaneClientSampleApp/Program.cs)]

> [!NOTE]
> Yukarıdaki kod örneği, varsayılan ortam değerlerini değiştirmeksizin çalıştırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Sorgulama hakkında daha fazla bilgi edinmek için [sorgu API 'si başvurusunu](https://docs.microsoft.com/rest/api/time-series-insights/preview-query)okuyun.

- [JavaScript tek sayfalı uygulamasını Time Series Insights bağlama](tutorial-create-tsi-sample-spa.md) hakkında bilgi edinin.