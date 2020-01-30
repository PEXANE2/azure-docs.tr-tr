---
title: include dosyası
description: include dosyası
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/12/2019
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 94adac6ba232f8931d00083432c027ddccb2ce64
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842512"
---
Komut çıkışında, `identity` bölümünde `SystemAssigned` türünde bir kimlik, görevde ayarlanır. `principalId`, kimliğin hizmet sorumlusu KIMLIĞIDIR:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 
Daha sonraki komutlarda kullanmak için PrincipalId 'yi bir değişkende depolamak üzere [az ACR Task Show][az-acr-task-show] komutunu kullanın. Aşağıdaki komutta, göreviniz ve kayıt defterinizin adını değiştirin:

```azurecli
principalID=$(az acr task show --name mytask --registry myregistry --query identity.principalId --output tsv)
```

<!-- LINKS - Internal -->
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
