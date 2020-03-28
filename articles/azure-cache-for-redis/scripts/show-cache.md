---
title: Redis için Azure Önbelleği ayrıntılarını alın - Azure CLI
description: Bu Azure CLI kodu örneği, sağlama durumu da dahil olmak üzere Redis için bir Azure Önbelleğinin ayrıntılarını nasıl alınacağını gösterir.
author: yegu-ms
ms.author: yegu
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.openlocfilehash: 54920017f4a0598a74a54114e3d1e0a5392d7d2a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75411070"
---
# <a name="get-details-of-an-azure-cache-for-redis"></a>Redis için Azure Önbelleği hakkında ayrıntılı bilgi alın

Bu senaryoda, Redis örneği için bir Azure Önbelleğinin ayrıntılarını sağlama durumu da dahil olmak üzere nasıl alınacağını öğrenirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/show-cache/show-cache.sh "Azure Cache for Redis")]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, Redis örneği için bir Azure Önbelleği ayrıntılarını almak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redis gösterisi](https://docs.microsoft.com/cli/azure/redis) | Redis örneği için bir Azure Önbelleği ayrıntılarını alın. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Redis CLI komut dosyası örnekleri için ek Azure Önbelleği, [Redis belgeleri için Azure Önbelleği'nde](../cli-samples.md)bulunabilir.
