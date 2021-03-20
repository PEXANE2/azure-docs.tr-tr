---
title: include dosyası
description: include dosyası
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/12/2019
ms.author: danlep
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: d81b6f5367efa92c9249956faa058441edf98561
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92755942"
---
### <a name="create-a-user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik oluşturma

[Az Identity Create][az-identity-create] komutunu kullanarak aboneliğinizde *Myacrtasksıd* adlı bir kimlik oluşturun. Daha önce bir kapsayıcı kayıt defteri oluşturmak için kullandığınız kaynak grubunu kullanabilir veya farklı bir tane oluşturabilirsiniz.

```azurecli
az identity create \
  --resource-group myResourceGroup \
  --name myACRTasksId
```

Aşağıdaki adımlarda Kullanıcı tarafından atanan kimliği yapılandırmak için, kimliğin kaynak KIMLIĞI, asıl KIMLIĞI ve istemci KIMLIĞINI değişkenlerde depolamak üzere [az Identity Show][az-identity-show] komutunu kullanın.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query id --output tsv)

# Get principal ID of the task's user-assigned identity
principalID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query clientId --output tsv)
```

<!-- LINKS - Internal -->
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
