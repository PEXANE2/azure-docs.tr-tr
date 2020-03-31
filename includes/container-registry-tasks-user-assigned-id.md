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
ms.openlocfilehash: ceda7bd6bd165df1eece555c6ce8a9a6c863b2c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77112289"
---
### <a name="create-a-user-assigned-identity"></a>Kullanıcı tarafından atanan kimlik oluşturma

Az kimlik oluşturma komutunu kullanarak aboneliğinizde *myACRTasksId* adlı bir [kimlik oluşturun.][az-identity-create] Bir kapsayıcı kayıt defteri veya farklı bir kayıt defteri oluşturmak için daha önce kullandığınız aynı kaynak grubunu kullanabilirsiniz.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Aşağıdaki adımlarda kullanıcı tarafından atanan kimliği yapılandırmak için, kimliğin kaynak kimliğini, ana kimliğini ve istemci kimliğini değişkenlerde depolamak için [az identity show][az-identity-show] komutunu kullanın.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get principal ID of the task's user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query clientId --output tsv)
```

<!-- LINKS - Internal -->
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show