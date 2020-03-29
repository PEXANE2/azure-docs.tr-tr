---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 046c5c5e32c71364e1bf54551989e19eec4937e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67712610"
---
Ayar, `ApplicationInsights` kapsayıcınıza [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) telemetri desteği eklemenize olanak tanır. Application Insights, kabınızın derinlemesine izlenmesini sağlar. Kullanılabilirlik, performans ve kullanım için kabınızı kolayca izleyebilirsiniz. Ayrıca, kapsayıcınızdaki hataları hızlı bir şekilde tanımlayabilir ve tanılayabilirsiniz.

Aşağıdaki tabloda `ApplicationInsights` bölüm altında desteklenen yapılandırma ayarları açıklanmaktadır.

|Gerekli| Adı | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Hayır| `InstrumentationKey` | Dize | Kapsayıcı için telemetri verilerinin gönderildiği Application Insights örneğinin enstrümantasyon anahtarı. Daha fazla bilgi [için ASP.NET Core için Uygulama Öngörüleri'ne](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core)bakın. <br><br>Örnek:<br>`InstrumentationKey=123456789`|

