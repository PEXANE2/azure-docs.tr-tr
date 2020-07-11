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
ms.openlocfilehash: ace22a6896a121f5cd8af838c7b0e427bd0287dc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188053"
---
## <a name="enable-event-grid-resource-provider"></a>Event Grid kaynak sağlayıcısını etkinleştirme

Azure aboneliğinizde daha önce Event Grid kullanmadıysanız Event Grid kaynak sağlayıcısına kaydolmanız gerekebilir. Sağlayıcıyı kaydetmek için şu komutu çalıştırın:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Kayıt işleminin tamamlanması biraz sürebilir. Durumu denetlemek için şunu çalıştırın:

```azurecli-interactive
az provider show --namespace Microsoft.EventGrid --query "registrationState"
```

`registrationState``Registered` olduğu zaman devam edebilirsiniz.