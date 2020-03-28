---
title: Redis için Azure Önbelleği Oluşturma - Azure CLI
description: Bu Azure CLI kod örneği, az redis'in oluşturduğu komutu kullanarak Redis örneği için bir Azure Önbelleği'nin nasıl oluşturulacağını gösterir.
author: yegu-ms
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.author: yegu
ms.openlocfilehash: 79b749c0d02a21c1225ee0d046d73ed3fdd98904
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75411311"
---
# <a name="create-an-azure-cache-for-redis"></a>Redis için Azure Cache oluşturma

Bu senaryoda, Redis için bir Azure Önbelleği oluşturmayı öğrenirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-cache/create-cache.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, bir kaynak grubu ve Redis için bir Azure Önbelleği oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az redis oluşturmak](https://docs.microsoft.com/cli/azure/redis) | Redis örneği için Azure Önbelleği oluşturun. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Redis CLI komut dosyası örnekleri için ek Azure Önbelleği, [Redis belgeleri için Azure Önbelleği'nde](../cli-samples.md)bulunabilir.
