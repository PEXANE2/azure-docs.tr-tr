---
title: Redsıs için Azure önbelleğini silme-Azure CLı
description: Bu Azure CLı kod örneği, az redin Delete komutunu kullanarak Redsıs örneği için bir Azure önbelleğinin nasıl silineceğini gösterir.
author: yegu-ms
ms.author: yegu
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.openlocfilehash: e5178ab4069cc5ffa8607c5feea6ffac86284a5b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75411073"
---
# <a name="delete-an-azure-cache-for-redis"></a>Redsıs için Azure önbelleğini silme

Bu senaryoda, redin için bir Azure önbelleğinin nasıl silineceğini öğrenirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/delete-cache/delete-cache.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, Redsıs örneği için bir Azure önbelleğini silmek üzere aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redsıs Delete](https://docs.microsoft.com/cli/azure/redis) | Redsıs örneği için Azure önbelleğini silin. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Redsıs CLı betik örnekleri için ek Azure önbelleği, [redin belgeleri Için Azure önbelleğinde](../cli-samples.md)bulunabilir.
