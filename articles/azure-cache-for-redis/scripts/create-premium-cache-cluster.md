---
title: Kümeleme ile Redis için Premium Azure Önbelleği Oluşturma - Azure CLI
description: Bu Azure CLI kod örneği, kümeleme etkin leştirilmiş ve iki parçayı içeren Redis için 6 GB Premium katmanazure Önbelleğinin nasıl oluşturulacağını gösterir.
author: yegu-ms
ms.author: yegu
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.openlocfilehash: 68203fe2e054f32ce5764fe4f1b07013b0806104
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75411141"
---
# <a name="create-a-premium-azure-cache-for-redis-with-clustering"></a>Kümeleme ile Redis için Premium Azure Önbelleği Oluşturma

Bu senaryoda, kümeleme etkin leştirilmiş ve iki parçayı içeren Redis için 6 GB Premium katmanazure Önbelleği oluşturmayı öğrenirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, kümeleme etkinleştirme ile bir kaynak grubu ve Redis için Premium katman Azure Önbelleği oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az redis oluşturmak](https://docs.microsoft.com/cli/azure/redis) | Redis örneği için Azure Önbelleği oluşturun. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Redis CLI komut dosyası örnekleri için ek Azure Önbelleği, [Redis belgeleri için Azure Önbelleği'nde](../cli-samples.md)bulunabilir.
