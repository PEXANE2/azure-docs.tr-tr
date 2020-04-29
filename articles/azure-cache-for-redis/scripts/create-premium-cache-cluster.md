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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75411141"
---
# <a name="create-a-premium-azure-cache-for-redis-with-clustering"></a>Kümeleme ile Reda için Premium Azure önbelleği oluşturma

Bu senaryoda, kümelemenin etkin ve iki parçalama ile redin için 6 GB Premium katman Azure önbelleği oluşturmayı öğreneceksiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Cache for Redis")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu oluşturmak için aşağıdaki komutları kullanır ve redde kümeleme etkinleştirerek Premium katman Azure önbelleği. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az redsıs oluştur](https://docs.microsoft.com/cli/azure/redis) | Redsıs örneği için Azure önbelleği oluşturun. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Redsıs CLı betik örnekleri için ek Azure önbelleği, [redin belgeleri Için Azure önbelleğinde](../cli-samples.md)bulunabilir.
