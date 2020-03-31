---
title: Azure Zamanlayıcısı'nda sınırlar, kotalar ve eşikler
description: Azure Zamanlayıcısı için sınırlar, kotalar, varsayılan değerler ve azaltma eşikleri hakkında bilgi edinin
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 610232dab776648bb3dcc7c301ec292e9acad9fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898520"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Azure Zamanlayıcısı'nda sınırlar, kotalar ve azaltma eşikleri

> [!IMPORTANT]
> [Azure Logic Apps,](../logic-apps/logic-apps-overview.md) [kullanımdan kaldırılan](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date)Azure Zamanlayıcısı'nın yerini alıyor. Zamanlayıcı'da ayarladığınız işlerle çalışmaya devam etmek için lütfen mümkün olan en kısa sürede [Azure Logic Apps'a geçirin.](../scheduler/migrate-from-scheduler-to-logic-apps.md) 
>
> Zamanlayıcı artık Azure portalında kullanılamıyor, ancak işlerinizi ve iş koleksiyonlarınızı yönetebilmeniz için [ŞU anda REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlets'i](scheduler-powershell-reference.md) kullanılabilir durumda kalır.

## <a name="limits-quotas-and-thresholds"></a>Sınırlar, kotalar ve eşikler

[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="x-ms-request-id-header"></a>x-ms-request-id üstbilgi

Zamanlayıcı hizmetine karşı yapılan her istek **x-ms-request-id**adlı bir yanıt üstbilgisini döndürür. Bu üstbilgi, isteği benzersiz olarak tanımlayan opak bir değer içerir. Bu nedenle, bir istek sürekli olarak başarısız olursa ve isteğin düzgün biçimlendirilmiş olduğunu doğruladıysanız, **x-ms-request-id** yanıt üstbilgisi değerini sağlayarak ve şu ayrıntıları da ekleyerek hatayı Microsoft'a bildirebilirsiniz: 

* **x-ms-request-id** değeri
* İsteğin yapıldığı yaklaşık süre 
* Azure aboneliği, iş toplama ve iş tanımlayıcıları 
* İsteğin denediği işlem türü

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Scheduler kavramları, terminolojisi ve varlık hiyerarşisi](scheduler-concepts-terms.md)
* [Azure Zamanlayıcısı için planlar ve faturalandırma](scheduler-plans-billing.md)
* [Azure Scheduler REST API başvurusu](/rest/api/scheduler)
* [Azure Scheduler PowerShell cmdlet’leri başvurusu](scheduler-powershell-reference.md)