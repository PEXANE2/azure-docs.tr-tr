---
title: include dosyası
description: include dosyası
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 08/17/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 579ea74c4767ba1afb6e0128c9f12ff83f7f3998
ms.sourcegitcommit: e7179fa4708c3af01f9246b5c99ab87a6f0df11c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97826689"
---
## <a name="enable-the-event-grid-resource-provider"></a>Event Grid Kaynak sağlayıcısını etkinleştirin

Daha önce Azure aboneliğinizde Event Grid kullanmadıysanız, Event Grid Kaynak sağlayıcısını kaydetmeniz gerekebilir. Sağlayıcıyı kaydetmek için şu komutu çalıştırın:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Kaydın tamamlanması biraz zaman alabilir. Durumu denetlemek için şunu çalıştırın:

```azurecli-interactive
az provider show --namespace Microsoft.EventGrid --query "registrationState"
```

`registrationState``Registered` olduğu zaman devam edebilirsiniz.
