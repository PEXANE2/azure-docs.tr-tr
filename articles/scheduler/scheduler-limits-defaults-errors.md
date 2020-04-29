---
title: Azure Scheduler 'da sınırlamalar, Kotalar ve eşikler
description: Azure Scheduler için sınırlar, kotalar, varsayılan değerler ve kısıtlama eşikleri hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 610232dab776648bb3dcc7c301ec292e9acad9fc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78898520"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Azure Scheduler 'da sınırlamalar, Kotalar ve kısıtlama eşikleri

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) , [devre dışı bırakılmakta](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)olan Azure Scheduler 'ı değiştiriyor. Zamanlayıcı 'da ayarladığınız işlerle çalışmaya devam etmek için lütfen en kısa sürede [Azure Logic Apps geçirin](../scheduler/migrate-from-scheduler-to-logic-apps.md) . 
>
> Zamanlayıcı artık Azure portal kullanılamıyor, ancak iş ve iş koleksiyonlarınızı yönetebilmeniz için [REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlet 'leri](scheduler-powershell-reference.md) Şu anda kullanılabilir durumda kalır.

## <a name="limits-quotas-and-thresholds"></a>Sınırlar, Kotalar ve eşikler

[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="x-ms-request-id-header"></a>x-MS-istek kimliği üst bilgisi

Zamanlayıcı hizmetine yapılan her istek, **x-MS-Request-ID**adlı bir yanıt üst bilgisi döndürür. Bu üstbilgi, isteği benzersiz bir şekilde tanımlayan donuk bir değer içerir. Bu nedenle, bir istek sürekli olarak başarısız olursa ve isteğin düzgün şekilde biçimlendirildiğini onayladıysanız, **x-MS-Request-id** yanıt üst bilgisi değerini sağlayarak ve bu ayrıntıları dahil ederek hatayı Microsoft 'a bildirebilirsiniz: 

* **X-MS-Request-ID** değeri
* İsteğin yapıldığı yaklaşık süre 
* Azure aboneliği, iş koleksiyonu ve iş için tanımlayıcılar 
* İsteğin denendiği işlem türü

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Scheduler için planlar ve faturalandırma](scheduler-plans-billing.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)