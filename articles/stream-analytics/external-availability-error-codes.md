---
title: Dış kullanılabilirlik hata kodları-Azure Stream Analytics
description: Dış kullanılabilirlik hata kodlarıyla ilgili Azure Stream Analytics sorunları giderin.
ms.author: mamccrea
author: mamccrea
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: c70a3a2d0630148a077f9c149ba40d48a6b7c0ae
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045289"
---
# <a name="azure-stream-analytics-external-availability-error-codes"></a>Dış kullanılabilirlik hata kodları Azure Stream Analytics

Azure Stream Analytics işinizin beklenmedik davranışlarından hata ayıklamaya yardımcı olması için etkinlik günlüklerini ve kaynak günlüklerini kullanabilirsiniz. Bu makalede, her dış kullanılabilirlik hata kodu için açıklama listelenir. Dış kullanılabilirlik hataları, bağımlı bir hizmet kullanılamadığında oluşur.

## <a name="externalserviceunavailable"></a>ExternalServiceUnavailable

* **Neden**: bir hizmet geçici olarak kullanılamıyor.
* **Öneri**: Stream Analytics hizmete ulaşmaya çalışmaya devam edecektir.

## <a name="eventhubmessagingerror"></a>EventHubMessagingError

* **Neden**: Stream Analytics EventHub ile iletişim kurulurken hatayla karşılaşıldı. 


## <a name="next-steps"></a>Sonraki adımlar

* [Giriş bağlantısı sorunlarını giderme](stream-analytics-troubleshoot-input.md)
* [Azure Stream Analytics çıkışları sorunlarını giderme](stream-analytics-troubleshoot-output.md)
* [Azure Stream Analytics sorgularının sorunlarını giderme](stream-analytics-troubleshoot-query.md)
* [Kaynak günlüklerini kullanarak Azure Stream Analytics sorunlarını giderme](stream-analytics-job-diagnostic-logs.md)
* [Azure Stream Analytics veri hataları](data-errors.md)
