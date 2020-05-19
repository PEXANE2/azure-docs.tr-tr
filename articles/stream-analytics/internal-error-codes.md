---
title: Azure Stream Analytics hata kodlarında sorun giderme
description: İç hata kodlarıyla ilgili Azure Stream Analytics sorunları giderin.
ms.author: mamccrea
author: mamccrea
ms.topic: conceptual
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: c8c792d548fc953cb5d059f322ad1f3139c91562
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597740"
---
# <a name="azure-stream-analytics-internal-error-codes"></a>Azure Stream Analytics iç hata kodları

Azure Stream Analytics işinizin beklenmedik davranışlarından hata ayıklamaya yardımcı olması için etkinlik günlüklerini ve kaynak günlüklerini kullanabilirsiniz. Bu makalede, her dahili hata kodu için açıklama listelenir. İç hatalar, Stream Analytics hata bir Iç kullanılabilirlik hatası ya da sistemdeki bir hata olup olmadığını ayıramadığından Stream Analytics platformda oluşturulan genel hatalardır.

## <a name="kafkainvalidrequest"></a>KafkaInvalidRequest

* **Neden**: Kafka sunucusuna gönderilen istek geçersiz.

## <a name="kafkainputerror"></a>KafkaInputError

* **Neden**: Kafka Input bir sorunla karşılaştı.

## <a name="cosmosdboutputbatchsizetoolarge"></a>Cosmosdi Putbatchsizetoolarge

* **Neden**: Cosmos DB yazmak için kullanılan toplu iş boyutu çok büyük. 
* **Öneri**: daha küçük bir toplu iş boyutuyla yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Giriş bağlantısı sorunlarını giderme](stream-analytics-troubleshoot-input.md)
* [Azure Stream Analytics çıkışları sorunlarını giderme](stream-analytics-troubleshoot-output.md)
* [Azure Stream Analytics sorgularının sorunlarını giderme](stream-analytics-troubleshoot-query.md)
* [Kaynak günlüklerini kullanarak Azure Stream Analytics sorunlarını giderme](stream-analytics-job-diagnostic-logs.md)
* [Azure Stream Analytics veri hataları](data-errors.md)