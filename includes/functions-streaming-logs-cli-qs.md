---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: 9213bdd8e63fc1e8ab7bdba8ac7a569be0dfe6be
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93425148"
---
Neredeyse gerçek zamanlı [akış günlüklerini](../articles/azure-functions/functions-run-local.md#enable-streaming-logs)görüntülemek için aşağıdaki komutu çalıştırın:

```console
func azure functionapp logstream <APP_NAME> 
```

Ayrı bir Terminal penceresinde veya tarayıcıda, uzak işlevi yeniden çağırın. Azure 'da işlev yürütmenin ayrıntılı günlüğü terminalde gösterilir. 