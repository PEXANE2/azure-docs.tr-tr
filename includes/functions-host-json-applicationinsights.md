---
title: dosya dahil etme
description: dosya dahil etme
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a925076dfccd30c73febb2aadc8692667ea01525
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "76279524"
---
[Application Insights 'de örnekleme özelliğini](../articles/azure-functions/functions-monitoring.md#configure-sampling)denetler.

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
|IsEnabled|true|Örneklemeyi etkinleştirilir veya devre dışı bırakır.| 
|maxTelemetryItemsPerSecond|5|Örneklemenin başladığı eşik.| 
