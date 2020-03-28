---
title: Azure CLI Script Örneği - Azure Uygulama Yapılandırma Deposundan Dışa Aktarma
titleSuffix: Azure App Configuration
description: Azure Uygulama Yapılandırmasından yapılandırma yı dışa aktarmak için Azure CLI komut dosyasını kullanma
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/19/2020
ms.author: lcozzens
ms.openlocfilehash: 3e014504fc1e94c3b323a49b408fa992de00d14a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77523639"
---
# <a name="export-from-an-azure-app-configuration-store"></a>Azure Uygulama Yapılandırma mağazasından dışa aktarma

Bu örnek komut dosyası, bir Azure Uygulama Yapılandırma deposundan anahtar değerleri dışa dışa aktarmaz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

```azurecli-interactive
#!/bin/bash

# Export all key-values
az appconfig kv export --name myTestAppConfigStore --file ~/Export.json
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, bir Uygulama Yapılandırma deposundan dışa aktarmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az appconfig kv ihracat](/cli/azure/appconfig/kv#az-appconfig-kv-export) | Bir Uygulama Yapılandırma deposu kaynağından dışa aktarma. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Uygulama Yapılandırması CLI komut dosyası örnekleri [Azure App Configuration CLI örnekleri](../cli-samples.md)bulunabilir.
