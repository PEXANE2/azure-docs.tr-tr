---
title: Redis için Bir Azure Önbelleği Silme - Azure CLI
description: Bu Azure CLI kod örneği, az redis silme komutunu kullanarak Redis örneği için bir Azure Önbelleğinin nasıl silineceğini gösterir.
author: yegu-ms
ms.author: yegu
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.openlocfilehash: e5178ab4069cc5ffa8607c5feea6ffac86284a5b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75411073"
---
# <a name="delete-an-azure-cache-for-redis"></a>Redis için Azure Önbelleğini Silme

Bu senaryoda, Redis için bir Azure Önbelleği nasıl silineceğinizi öğrenirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, Redis örneği için bir Azure Önbelleği'ni silmek için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redis silme](https://docs.microsoft.com/cli/azure/redis) | Redis örneği için Azure Önbelleğini silin. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Redis CLI komut dosyası örnekleri için ek Azure Önbelleği, [Redis belgeleri için Azure Önbelleği'nde](../cli-samples.md)bulunabilir.
