---
title: CLı örneği-Azure SQL yönetilen örnek için BYOK TDE 'yı etkinleştirin
description: Azure SQL yönetilen örneğini, PowerShell kullanarak bekleyen şifreleme için BYOK Saydam Veri Şifrelemesi (TDE) kullanmaya başlamak üzere nasıl yapılandıracağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: azurecli
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: vanto, carlrab
ms.date: 11/05/2019
ms.openlocfilehash: 08f0531a99a7e0af42332e349989ce12722be42c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86518801"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Yönetilen bir örnekteki Saydam Veri Şifrelemesi, Azure Key Vault kendi anahtarınızı kullanarak yönetin

Bu Azure CLı betik örneği, Azure Key Vault bir anahtar kullanarak Azure SQL yönetilen örneği için müşteri tarafından yönetilen anahtarla Saydam Veri Şifrelemesi (TDE) yapılandırır. Bu, genellikle TDE Kendi Anahtarını Getir senaryosu olarak adlandırılır. Müşteri tarafından yönetilen anahtarla TDE hakkında daha fazla bilgi edinmek için bkz. [tde kendi anahtarını getir Azure SQL 'e](../../azure-sql/database/transparent-data-encryption-byok-overview.md).

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="prerequisites"></a>Önkoşullar

Mevcut bir yönetilen örnek, bkz. Azure [CLI kullanarak Azure SQL yönetilen örneği oluşturma](sql-database-create-configure-managed-instance-cli.md).

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Betiği çalıştırın

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/transparent-data-encryption/setup-tde-byok-sqlmi.sh "Set up BYOK TDE for SQL Managed Instance")]

### <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanın.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Örnek başvuru

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Açıklama |
|---|---|
| [az SQL DB](/cli/azure/sql/db) | Veritabanı komutları. |
| [az SQL yük devretme-Grup](/cli/azure/sql/failover-group) | Yük devretme grubu komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../../azure-sql/database/az-cli-script-samples-content-guide.md) içinde bulunabilir.
