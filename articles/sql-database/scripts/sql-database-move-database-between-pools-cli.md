---
title: CLI örnek taşıma Azure SQL veritabanı-SQL elastik havuzu
description: Bir SQL elastik havuzunda SQL veritabanını taşımaya yönelik Azure CLI örnek betiği
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 06/25/2019
ms.openlocfilehash: b50f873977357522d2db87fd5132176b55874fbf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80061780"
---
# <a name="use-cli-to-move-an-azure-sql-database-in-a-sql-elastic-pool"></a>CLI kullanarak SQL elastik havuzunda bir Azure SQL veritabanını taşıma

Bu Azure CLI komut dosyası örneği iki elastik havuz oluşturur ve bir Azure SQL veritabanını bir SQL elastik havuzundan başka bir SQL elastik havuzuna taşır ve veritabanını elastik havuzdan tek bir veritabanı bilgi işlem boyutuna taşır.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

### <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Örnek referans

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| | |
|---|---|
| [az sql server](/cli/azure/sql/server) | Sunucu komutları. |
| [az sql elastik havuzlar](/cli/azure/sql/elastic-pool) | Elastik havuz komutları. |
| [az sql db](/cli/azure/sql/db) | Veritabanı komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../sql-database-cli-samples.md) içinde bulunabilir.
