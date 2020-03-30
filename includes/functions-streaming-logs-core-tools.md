---
author: ggailey777
ms.author: glenga
ms.date: 7/24/2019
ms.topic: include
ms.service: azure-functions
ms.openlocfilehash: 1928a8238cd73087e3c199675574dd1395f4d76d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68881361"
---
#### <a name="built-in-log-streaming"></a>Yerleşik günlük akışı

Aşağıdaki `logstream` örnekte olduğu gibi, Azure'da çalışan belirli bir işlev uygulamasının akış günlüklerini almaya başlamak için seçeneği kullanın:

```bash
func azure functionapp logstream <FunctionAppName>
```

#### <a name="live-metrics-stream"></a>Canlı Ölçüm Akışı

Aşağıdaki örnekte olduğu gibi `--browser` seçeneği ekleyerek işlev uygulamanız için Canlı Ölçümler [Akışı'nı](../articles/azure-monitor/app/live-stream.md) da yeni bir tarayıcı penceresinde görüntüleyebilirsiniz:

```bash
func azure functionapp logstream <FunctionAppName> --browser
```
