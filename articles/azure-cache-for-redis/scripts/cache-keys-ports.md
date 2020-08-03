---
title: Ana bilgisayar adı, bağlantı noktaları, anahtarlar-Redsıs için Azure önbelleği-Azure CLı 'yı alma
description: Bu Azure CLı kod örneği, Redsıs örneği için bir Azure önbelleği için ana bilgisayar adı, bağlantı noktaları ve anahtarların nasıl alınacağını gösterir.
author: yegu-ms
ms.author: yegu
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.custom: devx-track-azurecli
ms.openlocfilehash: 9fef834e550f144075ec9cc72cac3b11cc1e99e4
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87504220"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>Redsıs için Azure önbelleği için ana bilgisayar adı, bağlantı noktaları ve anahtarlar alın

Bu senaryoda, Redsıs örneği için bir Azure önbelleğine bağlanmak üzere kullanılan ana bilgisayar adını, bağlantı noktalarını ve anahtarları almayı öğrenirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Cache for Redis")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, Redsıs örneği için bir Azure önbelleğinin ana bilgisayar adını, anahtarlarını ve bağlantı noktalarını almak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redin Show](https://docs.microsoft.com/cli/azure/redis) | Redsıs örneği için Azure önbelleğinin ayrıntılarını alın. |
| [az redsıs List-Keys](https://docs.microsoft.com/cli/azure/redis) | Redsıs örneği için bir Azure önbelleği erişim anahtarlarını alın. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Redsıs CLı betik örnekleri için ek Azure önbelleği, [redin belgeleri Için Azure önbelleğinde](../cli-samples.md)bulunabilir.
