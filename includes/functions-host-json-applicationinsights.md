---
title: include dosyası
description: include dosyası
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a925076dfccd30c73febb2aadc8692667ea01525
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76279524"
---
Uygulama [Öngörüleri'ndeki örnekleme özelliğini](../articles/azure-functions/functions-monitoring.md#configure-sampling)denetler.

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|ısenabled|true|Örneklemeyi etkinleştiriveya devre dışı kılabilir.| 
|maxTelemetryItemsPerSecond|5|Örneklemenin başladığı eşik.| 
