---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 10/18/2020
ms.author: glenga
ms.openlocfilehash: 9213bdd8e63fc1e8ab7bdba8ac7a569be0dfe6be
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93425148"
---
Neredeyse gerçek zamanlı [akış günlüklerini](../articles/azure-functions/functions-run-local.md#enable-streaming-logs)görüntülemek için aşağıdaki komutu çalıştırın:

```console
func azure functionapp logstream <APP_NAME> 
```

Ayrı bir Terminal penceresinde veya tarayıcıda, uzak işlevi yeniden çağırın. Azure 'da işlev yürütmenin ayrıntılı günlüğü terminalde gösterilir. 