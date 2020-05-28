---
title: 'Azure CLı: veritabanlarını elastik havuzlar arasında taşıma'
description: İki elastik havuz oluşturan ve SQL veritabanındaki bir veritabanını Azure CLı kullanarak bir elastik havuzdan diğerine taşırken Azure CLı örnek betiği.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 06/25/2019
ms.openlocfilehash: 2bcf9f4218f535556ff83ccaad130bb1da28ef69
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84054062"
---
# <a name="use-cli-to-move-a-database-in-sql-database-in-a-sql-elastic-pool"></a>CLı kullanarak SQL veritabanı 'ndaki bir veritabanını SQL elastik havuzunda taşıma


Bu Azure CLı betik örneği iki elastik havuz oluşturur ve SQL veritabanı 'ndaki bir veritabanını bir SQL elastik havuzundan başka bir SQL elastik havuzuna taşımaktadır ve sonra veritabanını elastik havuzun dışına tek bir veritabanı işlem boyutuna taşımaktadır.

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

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

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../az-cli-script-samples-content-guide.md) içinde bulunabilir.
