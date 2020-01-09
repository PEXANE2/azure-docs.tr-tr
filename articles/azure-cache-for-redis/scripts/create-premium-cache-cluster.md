---
title: Kümeleme ile Redsıs için Premium Azure önbelleği oluşturma-Azure CLı
description: Bu Azure CLı kod örneği, kümelemenin etkin ve iki parçalama ile redin için 6 GB Premium katman Azure önbelleği oluşturmayı gösterir.
author: yegu-ms
ms.author: yegu
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.openlocfilehash: 68203fe2e054f32ce5764fe4f1b07013b0806104
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75411141"
---
# <a name="create-a-premium-azure-cache-for-redis-with-clustering"></a>Premium Azure önbelleği için Redis Kümeleme ile oluşturma

Bu senaryoda, Redis kümeleme özelliği etkinleştirilmiş olan ve iki parça için 6 GB Premium katmanı Azure önbelleği oluşturma hakkında bilgi edinin.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu oluşturmak için aşağıdaki komutları kullanır ve bir Premium katmanı Azure önbelleği için Redis Kümeleme ile etkinleştirebilirsiniz. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az redis oluşturma](https://docs.microsoft.com/cli/azure/redis) | Azure önbelleği için Redis örneği oluşturun. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure önbelleği için Redis CLI betiği örnekleri, içinde bulunabilir [Azure önbelleği için Redis belgeleri](../cli-samples.md).
