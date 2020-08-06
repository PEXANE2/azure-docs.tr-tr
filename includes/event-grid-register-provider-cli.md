---
title: dosya dahil etme
description: dosya dahil etme
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 08/17/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: b48e5e28b15d17796138d493a7d9c628791df2e6
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87830146"
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