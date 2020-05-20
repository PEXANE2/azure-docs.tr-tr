---
title: Azure Stream Analytics hata kodlarında sorun giderme
description: İç hata kodlarıyla ilgili Azure Stream Analytics sorunları giderin.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: 2e101747322baac860cbde02aca341171aa0cc1d
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83658353"
---
# <a name="azure-stream-analytics-internal-error-codes"></a>Azure Stream Analytics iç hata kodları

Azure Stream Analytics işinizin beklenmedik davranışlarından hata ayıklamaya yardımcı olması için etkinlik günlüklerini ve kaynak günlüklerini kullanabilirsiniz. Bu makalede, her dahili hata kodu için açıklama listelenir. İç hatalar, Stream Analytics hata bir Iç kullanılabilirlik hatası ya da sistemdeki bir hata olup olmadığını ayıramadığından Stream Analytics platformda oluşturulan genel hatalardır.

## <a name="cosmosdboutputbatchsizetoolarge"></a>Cosmosdi Putbatchsizetoolarge

* **Neden**: Cosmos DB yazmak için kullanılan toplu iş boyutu çok büyük. 
* **Öneri**: daha küçük bir toplu iş boyutuyla yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Giriş bağlantısı sorunlarını giderme](stream-analytics-troubleshoot-input.md)
* [Azure Stream Analytics çıkışları sorunlarını giderme](stream-analytics-troubleshoot-output.md)
* [Azure Stream Analytics sorgularının sorunlarını giderme](stream-analytics-troubleshoot-query.md)
* [Kaynak günlüklerini kullanarak Azure Stream Analytics sorunlarını giderme](stream-analytics-job-diagnostic-logs.md)
* [Azure Stream Analytics veri hataları](data-errors.md)