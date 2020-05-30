---
title: 'Azure CLı: elastik havuzu ölçeklendirme'
description: Azure SQL veritabanında esnek havuzu ölçeklendirmek için bir Azure CLı örnek betiği kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1
ms.devlang: azurecli
ms.topic: sample
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 06/25/2019
ms.openlocfilehash: 30862a85796fbe4373a88d80d9a6d26c8cf98901
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84196728"
---
# <a name="use-the-azure-cli-to-scale-an-elastic-pool-in-azure-sql-database"></a>Azure SQL veritabanı 'nda esnek havuzu ölçeklendirmek için Azure CLı 'yi kullanma

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Bu Azure CLı betik örneği, Azure SQL veritabanı 'nda elastik havuzlar oluşturur, havuza alınmış veritabanlarını taşımaktadır ve elastik havuz işlem boyutlarını değiştirir.

Azure CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLı sürüm 2,0 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Move database between pools")]

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
| [az SQL DB](/cli/azure/sql/db) | Veritabanı komutları. |
| [az SQL elastik-havuzlar](/cli/azure/sql/elastic-pool) | Elastik havuz komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../az-cli-script-samples-content-guide.md) içinde bulunabilir.
