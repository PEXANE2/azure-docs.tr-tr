---
title: Azure CLı betik örneği-Azure uygulama yapılandırma deposunu silme
titleSuffix: Azure App Configuration
description: Azure uygulama yapılandırma deposunu örnek bir Azure CLı betiği kullanarak silin. Betikte kullanılan komutlara yönelik başvuru makalesine bakın.
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/19/2020
ms.author: lcozzens
ms.custom: devx-track-azurecli
ms.openlocfilehash: e107ba9628eca384f518f7a241be5c00c278ef0a
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88588352"
---
# <a name="delete-an-azure-app-configuration-store"></a>Azure uygulama yapılandırma deposunu silme

Bu örnek betik bir Azure Uygulama yapılandırması örneğini siler.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

```azurecli-interactive
#/bin/bash

# Delete an App Configuration store named myTestAppConfigStore from the Resource Group myResourceGroup
az appconfig delete --name myTestAppConfigStore --resource-group myResourceGroup
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir uygulama yapılandırma deposunu silmek için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az appconfig Delete](/cli/azure/appconfig#az-appconfig-delete) | Bir uygulama yapılandırma deposu kaynağını siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek uygulama yapılandırması CLı betiği örnekleri, [Azure Uygulama YAPıLANDıRMASı CLI örneklerinde](../cli-samples.md)bulunabilir.
