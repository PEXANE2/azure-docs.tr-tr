---
title: CLI örneği- BYOK TDE'yi etkinleştir - Azure SQL Veritabanı Yönetilen Örneği
description: PowerShell'i kullanarak istirahatte şifreleme için BYOK Saydam Veri Şifrelemesini (TDE) kullanmaya başlamak için Azure SQL Yönetilen Örneği'ni nasıl yapılandırarak nasıl yapılandırılayarılamayı öğrenin.
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
ms.openlocfilehash: 8e8c0e2db1f87cca52c44d33ce14d7ce4f00e895
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061727"
---
# <a name="manage-transparent-data-encryption-in-a-managed-instance-using-your-own-key-from-azure-key-vault"></a>Azure Anahtar Kasası'ndan kendi anahtarınızı kullanarak Yönetilen Örnekte Saydam Veri Şifrelemeyi Yönetin

Bu Azure CLI komut dosyası örneği, Azure Anahtar Kasası'ndan bir anahtar kullanarak Azure SQL Yönetilen Örneği için müşteri tarafından yönetilen anahtarla Saydam Veri Şifrelemesini (TDE) yapılandırır. Bu genellikle TDE için Kendi Anahtarınızı Getir senaryosu olarak adlandırılır. Müşteri tarafından yönetilen anahtarla TDE hakkında daha fazla bilgi edinmek için TDE'ye bakın [Kendi Anahtarınızı Azure SQL'e Getirin.](../transparent-data-encryption-byok-azure-sql.md)

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

### <a name="prerequisites"></a>Ön koşullar

Varolan yönetilen [bir](sql-database-create-configure-managed-instance-cli.md)Örnek, bkz.

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

## <a name="sample-reference"></a>Örnek referans

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| | |
|---|---|
| [az sql db](/cli/azure/sql/db) | Veritabanı komutları. |
| [az sql failover-group](/cli/azure/sql/failover-group) | Failover grup komutları. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek SQL Veritabanı CLI betiği örnekleri, [Azure SQL Veritabanı belgeleri](../sql-database-cli-samples.md) içinde bulunabilir.
