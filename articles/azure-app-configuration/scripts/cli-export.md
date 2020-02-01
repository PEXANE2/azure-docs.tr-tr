---
title: Azure CLı betik örneği-Azure uygulama yapılandırma deposundan dışarı aktarma
titleSuffix: Azure App Configuration
description: Azure uygulama yapılandırma deposundan dışarı aktarmak için bilgi ve örnek komut dosyaları sağlar
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 0cdefaa60c9e7a1aab418042c037ef76b63e7804
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76898674"
---
# <a name="export-from-an-azure-app-configuration-store"></a>Azure uygulama yapılandırma deposundan dışarı aktarma

Bu örnek betik, bir Azure uygulama yapılandırma deposundan anahtar değerlerini dışarı aktarır.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli).

Aşağıdaki komutu yürüterek önce Azure uygulama yapılandırma CLı uzantısını yüklemeniz gerekir:

        az extension add -n appconfig

## <a name="sample-script"></a>Örnek betik

```azurecli-interactive
#!/bin/bash

# Export all key-values
az appconfig kv export --name myTestAppConfigStore --file ~/Export.json
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir uygulama yapılandırma deposundan dışarı aktarmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az appconfig kV Export](/cli/azure/ext/appconfig/appconfig/kv#ext-appconfig-az-appconfig-kv-export) | Uygulama yapılandırma deposu kaynağından dışarı aktarır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek uygulama yapılandırması CLı betiği örnekleri, [Azure Uygulama YAPıLANDıRMASı CLI örneklerinde](../cli-samples.md)bulunabilir.
