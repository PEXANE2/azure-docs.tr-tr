---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 06/15/2020
ms.author: glenga
ms.openlocfilehash: 49818cf59da2d63cef4bb0bdca38d38a2feafca5
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86169926"
---
| | |
|--|--|
|**`<DESTINATION>`**| Günlüklerin gönderildiği hedef. Geçerli değerler `AppInsights` ve ' dir `Blob` .<br/>Kullandığınızda `AppInsights` , [işlev uygulamanızda Application Insights etkinleştirildiğinden](../articles/azure-functions/functions-monitoring.md#enable-application-insights-integration)emin olun.<br/>Hedefi olarak ayarladığınızda `Blob` , Günlükler `azure-functions-scale-controller` uygulama ayarında ayarlanan varsayılan depolama hesabı içinde adlı bir blob kapsayıcısında oluşturulur `AzureWebJobsStorage` . |
|**`<VERBOSITY>`** | Günlüğe kaydetme düzeyini belirtir. Desteklenen değerler `None` , `Warning` , ve `Verbose` .<br/>Olarak ayarlandığında `Verbose` , ölçek denetleyicisi çalışan sayındaki her değişiklik için bir neden ve bu kararlara yönelik Tetikleyiciler hakkındaki bilgileri günlüğe kaydeder. Ayrıntılı Günlükler, tetikleyici uyarıları ve ölçek denetleyicisi çalıştırılmadan önce ve sonra Tetikleyiciler tarafından kullanılan karmaları içerir. |

> [!CAUTION]
> Ölçek denetleyicisi günlüğünü etkin bırakmayın. Ölçek denetleyicisinin nasıl davrandığını anlamak için yeterli veri toplanana kadar günlüğe kaydetmeyi etkinleştirin ve sonra devre dışı bırakın.