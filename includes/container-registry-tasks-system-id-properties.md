---
title: dosya dahil etme
description: dosya dahil etme
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/06/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 1b7c8487eb42204f2741679c9ef6eb2717c272cd
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86057372"
---
Komut çıkışında, `identity` bölümünde türünde bir kimlik `SystemAssigned` ayarlanır. , `principalId` Görev kimliğinin asıl kimliğidir:

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
principalID=$(az acr task show \
  --name <task_name> --registry <registry_name> \
  --query identity.principalId --output tsv)
```

<!-- LINKS - Internal -->
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
