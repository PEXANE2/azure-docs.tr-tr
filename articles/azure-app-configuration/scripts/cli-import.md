---
title: Azure CLI komut dosyası örneği - Uygulama Yapılandırma deposuna alma
titleSuffix: Azure App Configuration
description: Azure CLI komut dosyasını kullanma - Yapılandırmayı Azure Uygulama Yapılandırmasına aktarma
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.date: 02/19/2020
ms.author: lcozzens
ms.openlocfilehash: 71d6aafa82f647b9c6164ee9a06b43ed7e9a66af
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77523636"
---
# <a name="import-to-an-azure-app-configuration-store"></a>Azure Uygulama Yapılandırma deposuna aktarma

Bu örnek komut dosyası, bir Azure Uygulama Yapılandırma deposuna anahtar değer ayarlarını içeri aktarım.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürüm 2.0 veya sonraki sürümlerini çalıştırdığınız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemek veya yükseltmek için [Azure CLI'yi yükleyin'e](/cli/azure/install-azure-cli)bakın.

## <a name="sample-script"></a>Örnek betik

```azurecli-interactive
#!/bin/bash

# Import key-values from a file
az appconfig kv import --name myTestAppConfigStore --source file --path ~/Import.json
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, bir Uygulama Yapılandırma deposuna almak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az appconfig kv ithalat](/cli/azure/appconfig/kv#az-appconfig-kv-import) | Bir Uygulama Yapılandırma deposu kaynağına aktarım. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için [Azure CLI belgelerine](/cli/azure)bakın.

Ek Uygulama Yapılandırması CLI komut dosyası örnekleri [Azure App Configuration CLI örnekleri](../cli-samples.md)bulunabilir.
