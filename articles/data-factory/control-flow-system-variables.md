---
title: Azure Data Factory sistem değişkenleri
description: Bu makalede Azure Data Factory tarafından desteklenen sistem değişkenleri açıklanır. Data Factory varlıkları tanımlarken bu değişkenleri ifadelerde kullanabilirsiniz.
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
ms.openlocfilehash: 2690ded0ac45719cb1082c85ab535c91ad491172
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81417972"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Azure Data Factory tarafından desteklenen sistem değişkenleri
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory tarafından desteklenen sistem değişkenleri açıklanır. Data Factory varlıkları tanımlarken bu değişkenleri ifadelerde kullanabilirsiniz.

## <a name="pipeline-scope"></a>Ardışık düzen kapsamı
Bu sistem değişkenlerine JSON işlem hattı içinde herhangi bir yerde başvurulabilir.

| Değişken Adı | Description |
| --- | --- |
| @pipeline(). DataFactory |İşlem hattı çalıştırmasının çalıştırıldığı veri fabrikasının adı |
| @pipeline(). Konfigüre |İşlem hattının adı |
| @pipeline(). RunId | Belirli işlem hattı çalıştırmasının KIMLIĞI |
| @pipeline(). TriggerType | İşlem hattını çağıran tetikleyicinin türü (El Ile, zamanlayıcı) |
| @pipeline(). Triggerıd| İşlem hattını çağıran tetikleyicinin KIMLIĞI |
| @pipeline(). TriggerName| İşlem hattını çağıran tetikleyicinin adı |
| @pipeline(). TriggerTime| İşlem hattını çağıran tetikleyicinin süresi. Tetikleyici süresi, zamanlanan zamandan değil, gerçek tetikleme zamandır. Örneğin, `13:20:08.0149599Z` yerine şunu döndürür`13:20:00.00Z` |

## <a name="schedule-trigger-scope"></a>Tetikleyici kapsamını zamanla
Tetikleyici türü "ScheduleTrigger" ise bu sistem değişkenlerine, JSON tetikleyicisi içinde herhangi bir yerde başvurulabilir.

| Değişken Adı | Description |
| --- | --- |
| @trigger(). scheduledTime |Tetikleyicinin işlem hattı çalıştırmasını çağırmak için zamanlandığı zaman. Örneğin, her 5 dakikada bir tetiklenen bir tetikleyici için, bu değişken `2017-06-01T22:20:00Z` `2017-06-01T22:25:00Z` sırasıyla döndürülür `2017-06-01T22:30:00Z` .|
| @trigger(). başlangıçsaati |Tetikleyicinin işlem hattı çalıştırmasını çağırmak için **aslında** tetikleyeceği zaman. Örneğin, her 5 dakikada bir tetiklenen bir tetikleyici için, bu değişken sırasıyla bu gibi bir şey döndürebilir `2017-06-01T22:20:00.4061448Z` `2017-06-01T22:25:00.7958577Z` `2017-06-01T22:30:00.9935483Z` . (Note: zaman damgası varsayılan olarak ISO 8601 biçimindedir)|

## <a name="tumbling-window-trigger-scope"></a>Atlayan pencere tetikleme kapsamı
Tetikleyici türü "TumblingWindowTrigger" ise bu sistem değişkenlerine, JSON tetikleyicisi içinde herhangi bir yerde başvurulabilir.
(Note: zaman damgası varsayılan olarak ISO 8601 biçimindedir)

| Değişken Adı | Description |
| --- | --- |
| @trigger(). çıktılar. windowStartTime |Tetikleyici, işlem hattı çalıştırmasını çağırmak üzere zamanlandığında pencerenin başlangıcı. Atlayan pencere tetikleyicisi "saatlik" bir sıklık içeriyorsa, bu saat, saatin başlangıcında olur.|
| @trigger(). çıktılar. Windowbitişsaati |Tetikleyici, işlem hattı çalıştırmasını çağırmak üzere zamanlandığında pencerenin sonu. Atlayan pencere tetikleyicisi "saatlik" bir sıklık içeriyorsa, saat sonunda saat olur.|
## <a name="next-steps"></a>Sonraki adımlar
Bu değişkenlerin ifadelerde nasıl kullanıldığı hakkında bilgi için bkz. [ifade dili & işlevleri](control-flow-expression-language-functions.md).
