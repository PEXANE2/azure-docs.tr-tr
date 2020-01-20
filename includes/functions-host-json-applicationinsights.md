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
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
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
|isEnabled|doğru|Örneklemeyi etkinleştirilir veya devre dışı bırakır.| 
|maxTelemetryItemsPerSecond|5|Örneklemenin başladığı eşik.| 
