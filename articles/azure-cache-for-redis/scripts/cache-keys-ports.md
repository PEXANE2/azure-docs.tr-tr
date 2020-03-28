---
title: Ana bilgisayar adını, bağlantı noktalarını, anahtarları alın - Redis için Azure Önbelleği - Azure CLI
description: Bu Azure CLI kod örneği, Redis örneği için bir Azure Önbelleği için ana bilgisayar adını, bağlantı noktalarını ve anahtarlarını nasıl alacağımı gösterir.
author: yegu-ms
ms.author: yegu
tags: azure-service-management
ms.service: cache
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/30/2017
ms.openlocfilehash: 199c5707e9ecd887af64e271184e151ce0966745
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75411296"
---
# <a name="get-the-hostname-ports-and-keys-for-azure-cache-for-redis"></a>Redis için Azure Önbelleği için ana bilgisayar adını, bağlantı noktalarını ve anahtarlarını alın

Bu senaryoda, Redis örneği için bir Azure Önbelleğine bağlanmak için kullanılan ana bilgisayar adını, bağlantı noktalarını ve anahtarları nasıl alabilirsiniz.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Cache for Redis")]


## <a name="script-explanation"></a>Betik açıklaması

Bu komut dosyası, Redis örneği için bir Azure Önbelleğinin ana bilgisayar adını, anahtarlarını ve bağlantı noktalarını almak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az redis gösterisi](https://docs.microsoft.com/cli/azure/redis) | Redis örneği için bir Azure Önbelleği ayrıntılarını alın. |
| [az redis liste anahtarları](https://docs.microsoft.com/cli/azure/redis) | Redis örneği için bir Azure Önbelleği için erişim anahtarlarını alın. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Redis CLI komut dosyası örnekleri için ek Azure Önbelleği, [Redis belgeleri için Azure Önbelleği'nde](../cli-samples.md)bulunabilir.
