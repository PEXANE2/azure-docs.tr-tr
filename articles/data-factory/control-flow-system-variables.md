---
title: Azure Veri Fabrikası'nda sistem değişkenleri
description: Bu makalede, Azure Veri Fabrikası tarafından desteklenen sistem değişkenleri açıklanmaktadır. Veri Fabrikası varlıklarını tanımlarken bu değişkenleri ifadelerde kullanabilirsiniz.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/12/2018
ms.openlocfilehash: 0a5237336530d30c3801b13b910171e236e87a23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73679285"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Azure Veri Fabrikası tarafından desteklenen sistem değişkenleri
Bu makalede, Azure Veri Fabrikası tarafından desteklenen sistem değişkenleri açıklanmaktadır. Veri Fabrikası varlıklarını tanımlarken bu değişkenleri ifadelerde kullanabilirsiniz.

## <a name="pipeline-scope"></a>Boru hattı kapsamı
Bu sistem değişkenleri, JSON boru hattının herhangi bir yerine başvurulabilir.

| Değişken Adı | Açıklama |
| --- | --- |
| @pipeline(). Datafactory |Boru hattı nın içinde çalışan veri fabrikasının adı |
| @pipeline(). Boru hattı |Boru hattının adı |
| @pipeline(). Runid | Belirli bir boru hattı çalıştırımının kimliği |
| @pipeline(). Tetikleyici Türü | Ardışık lığı çağıran tetikleyicinin türü (Manuel, Zamanlayıcı) |
| @pipeline(). TriggerId| Boru hattını çağıran tetikleyicinin kimliği |
| @pipeline(). TriggerName| Ardışık hattı çağıran tetikleyicinin adı |
| @pipeline(). Tetikleme Zamanı| Boru hattını çağıran tetikleyicinin zamanı. Tetikleme süresi, zamanlanan saat değil, gerçek ateşlenen saattir. Örneğin, `13:20:08.0149599Z` yerine döndürülür`13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Zamanlama Tetikleyici kapsamı
Tetikleyici "ScheduleTrigger" türündeyse, bu sistem değişkenleri tetikleyici JSON'un herhangi bir yerine başvurulabilir.

| Değişken Adı | Açıklama |
| --- | --- |
| @trigger().scheduledTime |Tetikleyicinin ardışık hatlar çalışmasını çağırmak için zamanlanması zamanı. Örneğin, her 5 dakikada bir ateşleyen bir tetikleyici `2017-06-01T22:20:00Z` `2017-06-01T22:25:00Z`için, bu değişken `2017-06-01T22:30:00Z` sırasıyla , döndürecek.|
| @trigger().startTime |Tetikin boru hattı nı çağırmak için **ateşlediği** zaman. Örneğin, her 5 dakikada bir ateşleyen bir tetikleyici için, `2017-06-01T22:20:00.4061448Z` `2017-06-01T22:25:00.7958577Z`bu `2017-06-01T22:30:00.9935483Z` değişken sırasıyla bu gibi bir şeyi döndürebilir. (Not: Zaman damgası ISO 8601 formatında varsayılan olarak)|

## <a name="tumbling-window-trigger-scope"></a>Yuvarlanan Pencere Tetikkapsamı
Tetikleyici "TumblingWindowTrigger" türündeyse, bu sistem değişkenleri tetikleyici JSON'un herhangi bir yerine başvurulabilir.
(Not: Zaman damgası ISO 8601 formatında varsayılan olarak)

| Değişken Adı | Açıklama |
| --- | --- |
| @trigger().outputs.windowStartTime |Tetikleyici, ardışık hatlar çalışmasını çağırmak için zamanlandığında pencerenin başlangıcı. Yuvarlanan pencere tetikleyicisi "saatlik" bir frekansa sahipse, bu saatin başındaki saat olacaktır.|
| @trigger().outputs.windowEndTime |Tetikleyici, ardışık hatlar çalışmasını çağırmak için zamanlandığında pencerenin sonu. Yuvarlanan pencere tetikleyicisi "saatlik" bir frekansa sahipse, saatin sonundaki saat budur.|
## <a name="next-steps"></a>Sonraki adımlar
Bu değişkenlerin ifadelerde nasıl kullanıldığı hakkında bilgi için, [İfade dili & işlevlerine](control-flow-expression-language-functions.md)bakın.
