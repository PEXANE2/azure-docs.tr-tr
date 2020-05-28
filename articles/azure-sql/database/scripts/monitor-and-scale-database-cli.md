---
title: "Azure CLı: Azure SQL veritabanı 'nda & ölçek veritabanını Izleme"
description: Azure SQL veritabanı 'nda tek bir veritabanını izlemek ve ölçeklendirmek için Azure CLı örnek betiği
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
ms.date: 06/25/2019
ms.openlocfilehash: be57309e4b327027ed0185c8eabf783a18cc957e
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84053726"
---
# <a name="use-cli-to-monitor-and-scale-a-single-database-in-azure-sql-database"></a>Azure SQL veritabanı 'nda tek bir veritabanını izlemek ve ölçeklendirmek için CLı kullanma
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Bu Azure CLı betik örneği, veritabanının boyut bilgilerini sorguladıktan sonra Azure SQL veritabanı 'ndaki tek bir veritabanını farklı bir işlem boyutuna ölçeklendirir.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale a single database in Azure SQL Database")]

> [!TIP]
> Veritabanında gerçekleştirilen işlemlerin bir listesini almak için [az SQL DB op listesini](/cli/azure/sql/db/op?#az-sql-db-op-list) kullanın ve ardından veritabanında bir güncelleştirme işlemini iptal etmek için [az SQL DB op Cancel](/cli/azure/sql/db/op#az-sql-db-op-cancel) .

### <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Örnek başvuru

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| | |
|---|---|
| [az sql server](/cli/azure/sql/server) | Sunucu komutları. |
| [az sql db show-usage](/cli/azure/sql#az-sql-show-usage) | Tek veya havuza alınmış bir veritabanının boyut kullanım bilgilerini gösterir. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek CLı betik örnekleri, [Azure CLI örnek betiklerine](../az-cli-script-samples-content-guide.md)bulunabilir.
