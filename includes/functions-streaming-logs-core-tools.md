---
author: ggailey777
ms.author: glenga
ms.date: 7/24/2019
ms.topic: include
ms.service: azure-functions
ms.openlocfilehash: 1928a8238cd73087e3c199675574dd1395f4d76d
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "68881361"
---
#### <a name="built-in-log-streaming"></a>Yerleşik günlük akışı

Azure’da çalışan belirli bir işlev uygulamasının akış günlüklerini almaya başlamak için, aşağıda örnekte gösterildiği gibi `logstream` seçeneğini kullanın:

```bash
func azure functionapp logstream <FunctionAppName>
```

#### <a name="live-metrics-stream"></a>Canlı Ölçüm Akışı

İşlev uygulamanızın [Canlı Ölçüm Akışı](../articles/azure-monitor/app/live-stream.md)’nı, aşağıdaki örnekte gösterildiği gibi `--browser` seçeneğini kullanarak yeni bir tarayıcı penceresinde de görüntüleyebilirsiniz:

```bash
func azure functionapp logstream <FunctionAppName> --browser
```
