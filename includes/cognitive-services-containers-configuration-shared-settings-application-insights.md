---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 046c5c5e32c71364e1bf54551989e19eec4937e4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67712610"
---
`ApplicationInsights`Ayar, kapsayıcınıza [Azure Application Insights](https://docs.microsoft.com/azure/application-insights) telemetri desteği eklemenize olanak tanır. Application Insights kapsayıcının derinlemesine izlenmesini sağlar. Kapsayıcınızı kullanılabilirlik, performans ve kullanım açısından kolayca izleyebilirsiniz. Ayrıca, kapsayıcıınızda hataları hızlıca tanımlayabilir ve tanılayabilirsiniz.

Aşağıdaki tabloda, bölümünde desteklenen yapılandırma ayarları açıklanmaktadır `ApplicationInsights` .

|Gerekli| Name | Veri türü | Açıklama |
|--|------|-----------|-------------|
|Hayır| `InstrumentationKey` | Dize | Kapsayıcı için Telemetri verilerinin gönderildiği Application Insights örneğinin izleme anahtarı. Daha fazla bilgi için bkz. [ASP.NET Core Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). <br><br>Örnek:<br>`InstrumentationKey=123456789`|

