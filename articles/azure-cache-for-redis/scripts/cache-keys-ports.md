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
ms.openlocfilehash: 199c5707e9ecd887af64e271184e151ce0966745
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75411296"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>Ana bilgisayar adı, bağlantı noktalarını ve anahtarları, Azure önbelleği için Redis için Al

Bu senaryoda, ana bilgisayar adı, bağlantı noktaları ve bir Azure önbelleği için Redis örneği bağlanmak için kullanılan anahtarlarını alma hakkında bilgi edinin.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Cache for Redis")]


## <a name="script-explanation"></a>Betik açıklaması

Bu betik, ana bilgisayar adı, anahtarlar ve bağlantı noktaları, bir Azure önbelleği için Redis örneği almak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redis show](https://docs.microsoft.com/cli/azure/redis) | Bir Azure önbelleği için Redis örneğinin ayrıntılarını alın. |
| [az redis anahtarları-Listele](https://docs.microsoft.com/cli/azure/redis) | Bir Azure önbelleği için Redis örneği için erişim anahtarlarını alır. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure önbelleği için Redis CLI betiği örnekleri, içinde bulunabilir [Azure önbelleği için Redis belgeleri](../cli-samples.md).
