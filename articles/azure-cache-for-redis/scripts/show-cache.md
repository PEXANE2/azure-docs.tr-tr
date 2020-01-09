---
title: Redin-Azure CLı için Azure önbelleğinin ayrıntılarını alma
description: Bu Azure CLı kod örneği, sağlama durumu da dahil olmak üzere Redsıs örneği için Azure önbelleğinin ayrıntılarının nasıl alınacağını gösterir.
author: yegu-ms
ms.author: yegu
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.openlocfilehash: 54920017f4a0598a74a54114e3d1e0a5392d7d2a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75411070"
---
# <a name="get-details-of-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis ayrıntılarınıza

Bu senaryoda, bir Azure önbelleği için Redis örneği, sağlama durumu'dahil olmak üzere ayrıntılarını alma hakkında bilgi edinin.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Cache for Redis")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir Azure önbelleği için Redis örneğinin ayrıntılarını almak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis) | Bir Azure önbelleği için Redis örneğinin ayrıntılarını alın. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure önbelleği için Redis CLI betiği örnekleri, içinde bulunabilir [Azure önbelleği için Redis belgeleri](../cli-samples.md).
