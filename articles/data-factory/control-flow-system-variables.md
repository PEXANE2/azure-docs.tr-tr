---
title: Azure Data Factory sistem değişkenleri
description: Bu makalede Azure Data Factory tarafından desteklenen sistem değişkenleri açıklanır. Data Factory varlıkları tanımlarken bu değişkenleri ifadelerde kullanabilirsiniz.
author: dcstwh
ms.author: weetok
ms.reviewer: maghan
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/12/2018
ms.openlocfilehash: 3da450ce1130b693fa346b4a6f4b42f6a0fd60a3
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102174215"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Azure Data Factory tarafından desteklenen sistem değişkenleri
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory tarafından desteklenen sistem değişkenleri açıklanır. Data Factory varlıkları tanımlarken bu değişkenleri ifadelerde kullanabilirsiniz.

## <a name="pipeline-scope"></a>Ardışık düzen kapsamı

Bu sistem değişkenlerine JSON işlem hattı içinde herhangi bir yerde başvurulabilir.

| Değişken Adı | Açıklama |
| --- | --- |
| @pipeline(). DataFactory |İşlem hattı çalıştırmasının çalıştırıldığı veri fabrikasının adı |
| @pipeline(). Konfigüre |İşlem hattının adı |
| @pipeline(). RunId |Belirli işlem hattı çalıştırmasının KIMLIĞI |
| @pipeline(). TriggerType |İşlem hattını çağıran tetikleyicinin türü (örneğin,, `ScheduleTrigger` `BlobEventsTrigger` ). Desteklenen tetikleyici türlerinin bir listesi için bkz. [Azure Data Factory Işlem hattı yürütme ve Tetikleyiciler](concepts-pipeline-execution-triggers.md). Tetikleyici türü, işlem `Manual` hattının el ile tetiklendiğini gösterir. |
| @pipeline(). Triggerıd|İşlem hattını çağıran tetikleyicinin KIMLIĞI |
| @pipeline(). TriggerName|İşlem hattını çağıran tetikleyicinin adı |
| @pipeline(). TriggerTime|İşlem hattını çağıran tetikleyici çalıştırmasının süresi. Bu, tetikleyicinin işlem hattı çalıştırmasını çağırmak için **aslında** tetiklenme zamanı ve tetikleyicinin zamanlanan zamanından biraz farklı olabilir.  |

>[!NOTE]
>Tetikleyiciyle ilgili tarih/saat sistem değişkenleri (hem ardışık düzen hem de tetikleme kapsamları) ISO 8601 biçiminde UTC tarihleri döndürür, örneğin, `2017-06-01T22:20:00.4061448Z` .

## <a name="schedule-trigger-scope"></a>Tetikleyici kapsamını zamanla

Bu sistem değişkenlerine, [scheduletrigger](concepts-pipeline-execution-triggers.md#schedule-trigger)türünde Tetikleyiciler IÇIN Trigger JSON içinde herhangi bir yerde başvurulabilir.

| Değişken Adı | Açıklama |
| --- | --- |
| @trigger(). scheduledTime |Tetikleyicinin işlem hattı çalıştırmasını çağırmak için zamanlandığı zaman. |
| @trigger(). başlangıçsaati |Tetikleyicinin işlem hattı çalıştırmasını çağırmak için **aslında** tetikleyeceği zaman. Bu, tetikleyicinin zamanlanan zamanından biraz farklı olabilir. |

## <a name="tumbling-window-trigger-scope"></a>Atlayan pencere tetikleme kapsamı

Bu sistem değişkenlerine, [Tumblingwindowtrigger](concepts-pipeline-execution-triggers.md#tumbling-window-trigger)türündeki Tetikleyiciler IÇIN Trigger JSON içinde herhangi bir yerde başvurulabilir.

| Değişken Adı | Açıklama |
| --- | --- |
| @trigger(). çıktılar. windowStartTime |Tetikleyici çalıştırması ile ilişkili pencerenin başlangıcı. |
| @trigger(). çıktılar. Windowbitişsaati |Tetikleyici çalıştırması ile ilişkili pencerenin sonu. |
| @trigger(). scheduledTime |Tetikleyicinin işlem hattı çalıştırmasını çağırmak için zamanlandığı zaman. |
| @trigger(). başlangıçsaati |Tetikleyicinin işlem hattı çalıştırmasını çağırmak için **aslında** tetikleyeceği zaman. Bu, tetikleyicinin zamanlanan zamanından biraz farklı olabilir. |

## <a name="storage-event-trigger-scope"></a>Depolama olayı tetikleme kapsamı

Bu sistem değişkenlerine, [BlobEventsTrigger](concepts-pipeline-execution-triggers.md#event-based-trigger)türündeki Tetikleyiciler IÇIN tetikleyici JSON içinde herhangi bir yerde başvurulabilir.

| Değişken Adı | Açıklama |
| --- | --- |
| @triggerBody(). fileName  |Oluşturma veya silme işlemi tetikleyiciye neden olan dosyanın adı.   |
| @triggerBody(). KlasörAdı  |Tarafından belirtilen dosyayı içeren klasörün yolu `@triggerBody().fileName` . Klasör yolunun ilk segmenti, Azure Blob depolama kapsayıcısının adıdır.  |
| @trigger(). başlangıçsaati |İşlem hattının çalışmasını çağırmak için tetikleyicinin tetiklenme zamanı. |

## <a name="next-steps"></a>Sonraki adımlar

Bu değişkenlerin ifadelerde nasıl kullanıldığı hakkında bilgi için bkz. [ifade dili & işlevleri](control-flow-expression-language-functions.md).
