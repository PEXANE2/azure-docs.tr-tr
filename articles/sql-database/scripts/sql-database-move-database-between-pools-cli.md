---
title: CLı örneği-Azure SQL veritabanını taşıma-SQL elastik havuzu
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80061780"
---
# <a name="use-cli-to-move-an-azure-sql-database-in-a-sql-elastic-pool"></a>CLI kullanarak SQL elastik havuzunda bir Azure SQL veritabanını taşıma

Bu Azure CLı betik örneği iki elastik havuz oluşturur ve bir Azure SQL veritabanını bir SQL elastik havuzundan başka bir SQL elastik havuzuna taşımaktadır ve sonra veritabanını elastik havuzun dışına tek bir veritabanı işlem boyutuna taşımaktadır.

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

## <a name="sample-reference"></a>Örnek başvuru

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| | |
|---|---|
| [az sql server](/cli/azure/sql/server) | Sunucu komutları. |
| [az SQL elastik-havuzlar](/cli/azure/sql/elastic-pool) | Elastik havuz komutları. |
| [az SQL DB](/cli/azure/sql/db) | Veritabanı komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../sql-database-cli-samples.md) içinde bulunabilir.
