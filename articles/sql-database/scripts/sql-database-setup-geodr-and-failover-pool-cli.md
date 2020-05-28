---
title: 'Az CLı: esnek havuz için etkin Coğrafi çoğaltmayı yapılandırma'
description: Azure SQL veritabanı 'nda havuza alınmış bir veritabanı için etkin Coğrafi çoğaltmayı ayarlamak ve yük devretmek için Azure CLı örnek betiği.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 03/12/2019
ms.openlocfilehash: a272564684671e38100e5390e95129121fcbd0f8
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84019392"
---
# <a name="use-cli-to-configure-active-geo-replication-for-a-pooled-database-in-azure-sql-database"></a>Azure SQL veritabanı 'nda havuza alınmış bir veritabanı için etkin Coğrafi çoğaltmayı yapılandırmak için CLı kullanma

Bu Azure CLı betik örneği, Azure SQL veritabanı 'nda havuza alınmış bir veritabanı için etkin Coğrafi çoğaltmayı yapılandırır ve veritabanının ikincil çoğaltmasına yük devreder.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/setup-geodr-and-failover/setup-geodr-and-failover-elastic-pool.sh "Set up active geo-replication for elastic pool")]

### <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```azurecli-interactive
az group delete --name $resource
az group delete --name $secondaryResource
```

## <a name="sample-reference"></a>Örnek başvuru

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| | |
|---|---|
| [az SQL elastik havuz](/cli/azure/sql/elastic-pool) | Elastik havuz komutları |
| [az SQL DB Replica](/cli/azure/sql/db/replica) | Veritabanı çoğaltma komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../../azure-sql/database/az-cli-script-samples-content-guide.md) içinde bulunabilir.
