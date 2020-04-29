---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 046c5c5e32c71364e1bf54551989e19eec4937e4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67712610"
---
Ayar `ApplicationInsights` , kapsayıcınıza [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) telemetri desteği eklemenize olanak tanır. Application Insights kapsayıcının derinlemesine izlenmesini sağlar. Kapsayıcınızı kullanılabilirlik, performans ve kullanım açısından kolayca izleyebilirsiniz. Ayrıca, kapsayıcıınızda hataları hızlıca tanımlayabilir ve tanılayabilirsiniz.

Aşağıdaki tabloda, `ApplicationInsights` bölümünde desteklenen yapılandırma ayarları açıklanmaktadır.

|Gerekli| Adı | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Hayır| `InstrumentationKey` | Dize | Kapsayıcı için Telemetri verilerinin gönderildiği Application Insights örneğinin izleme anahtarı. Daha fazla bilgi için bkz. [ASP.NET Core Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Örnek:<br>`InstrumentationKey=123456789`|

